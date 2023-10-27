# Learning.md: Notes from Rock And Roll With Ember Octane

Some commands & principles I've learned from the [Rock And Roll with Ember Octane](https://balinterdi.com/rock-and-roll-with-emberjs/) that I might want to have in an overview!

## Design principles
### The rule of Least Power
For Ember, (I think) it boils down to this:
- You should use the least powerful language to fix problems
- No JavaScript in templating/handlebars means that future updates will have minimal breaking changes

Also see 11:30 in this [Emberweekend episode](https://emberweekend.com/episodes/the-rule-of-least-power/) and the [W3C piece on it](https://www.w3.org/2001/tag/doc/leastPower.html).

### Handlebars' fail-softness
undefined/null properties get ignored in Handlebars. This gives cleaner output and less conditional code, but is something to keep in mind when debugging.

### Handlebars expressions

| Expression    | ...is a...     | Definition location |
| ------------- | -------------- | ------------------- |
| {{data}}      | Block variable | Nearest block       |
| {{this.data}} | Property       | Template's context  |
| {{@data}}     | Argument       | Call site           |

(See pages 772 of [Rock and Roll with Ember Octane](https://balinterdi.com/rock-and-roll-with-emberjs/))

### Routing & outlet
- Everything gets routed through the application.hbs, useful for:
    - Setting the language
    - Fetching data to render a common layout
    - Basically to define structure & common markup across your application
- Subroutes render their content into the parent template {{outlet}}, & the child routes also inherit their parents' model()
- ```js
    this.route('bands'); == this.route('bands', { path: 'bands' }, function () {});
  ```
- `bands.band.songs` = full name. Entities in:
    - `app/routes/bands/band/songs.js`
    - `app/templates/bands/band/songs.hbs`
- The top-level route will be executed once, and by using `this.modelFor("modelname");` things can be passed down to the lower levels
    - This has some implications! If you load in things in bands.band, navigate to bands.band.details, and then to `<LinkTo @route="bands.band>`, it will actually link to bands.band.index. That does *not* require it to pass bands.band, and thus it won't, and the loading happening within bands.band won't execute
        - But don't change the LinkTo to bands.band.index, because then it will goof some CSS stuff; bands.band won't be the active route then, but bands.band.index. Bands.band.index is not a parent of bands.band.songs or bands.band.details, so those links won't be marked as active and that's just goofy and also this is super hard to explain so check page 166 of Rock & Roll with EmberJS in case of doubt.
        - Also, if you do `<LinkTo @model={{item.band}}>` instead of `<LinkTo @model={{item.band.id}}>` (thus the dynamic segment of :id and a context object (band object)) the model hook will only fire on a fresh reload, and not from pressing the link
- You can use `modelFor` to get the parent routes' model.
    ```js
    export default class BandsBandSongsRoute extends Route {
        model() {
            let band = this.modelFor('bands.band');
            return band.songs;
        }
    }
    ```

### Components
- Mostly self contained so they can be moved across applications
- Call using `<CamelisedName>` to distinguish from html
- Previously used to only be called using {curly braces}

### Controllers
- The `@action` decorator (from `'@ember/object'`) is only needed if the function gets called from the template
- Controllers are singletons (single instances). If that gives any issues, use `resetController(controller) { /* reset values here */ }`
- The `on` method creates listeners. On components, you have to use `off` to unregister, but controllers are only created once and remain until the app is closed, so `off` is not necessary.
- Controllers seem to be in a weird limbo state in terms of how long they'll remain useful or best practice, as often a component or route can better be utilised. The best rule of thumb I've found is from Rock & Roll with Ember: `The best way to think about [controllers] is
that they are top-level components for a specific route, with a few peculiarities...`

### Routes hook order
1. [`beforeModel(transition){}`](https://api.emberjs.comember/release/classes/Route/methods/?anchor=beforeModel)
2. [`model(params, transition){}`](https://api.emberjs.comember/release/classes/Route/methods/?anchor=model)
3. [`afterModel(model, transition){}`](https://api.emberjscom/ember/release/classes/Route/methods/?anchor=afterModel)
4. [`setupController(controller, model, transitions)`](https://api.emberjs.com/ember/release/classes/Route/methodsanchor=setupController)

Sequence example: application(beforeModel -> model ->afterModel) --> route(beforeModel --> model --> afterModel)--> subroute(beforeModel --> model --> afterModel) -->application(setupController) --> route(setupController) --> subroute(setupController)

There's also activate (fires when new route entered, but not when the model changes) and deactivate (fired when a route is completely exited, and not fired when only the model changes)

### Loading/Error templates funky town
(All the following is written for `loading`, but it can normally just be interswitched with `error` for, errors)
- Okay so putting a `loading.hbs` on any level in templates/ seems to automatically use it while any of its siblings in the route are loading and it just works I think help.
- To override this behaviour, you can use 
    ```js
    export default class ExampleRoute extends Route {
        // ...
        @action
        loading() {
            // Do things here
        }
        // ...
    }
    ```
- For a route specific one, you can make `routename-loading.hbs`
- The lookup order works from most specific > least specific:
    - route.sub.place-loading
    - route.sub.loading / route.sub-loading
    - route.loading / route-loading
    - loading / application.loading

### Data down actions up
The best explanation is this diagram created by [Andy del Valle](https://medium.com/swlh/understanding-information-flow-in-react-data-down-action-up-b6c792a8b010)!
![The actions-up-data-down diagram created by Andy del Valle](/assets/learning-md/actions-up-data-down-Andy-del-Valle.webp)

### Testing
Okay this is a bit funky town.
- As with testing any project, Ember test can be written before or after developing things.
- You can seemingly create any folder structure you want, as long as your filename follows `*-test.js`.
- The default testing framework is `qunit` but this can be replaced.
    - The [Ember.js docs](https://guides.emberjs.com/release/testing/testing-tools/) mention `Mocha/Chai DOM`.
    - I've seen a lot of people online use [Sinonjs](https://sinonjs.org/), especially due to the time manipulation capabilities.
- You can use Ember(cli) Mirage to stub servers.

There are a few types of tests, with examples imported & simplified from Ember docs
- Unit tests
    - Fast, tests specific functions & code.
    - Default for adapters, controllers, initializers, models, serializers, services & utilities according to the docs. But it seems that these are also the default for routes
    - Looks like the following:
        ```js
        module('Unit | Controller | posts', function(hooks) {
            setupTest(hooks);

            test('should update A and B on setProps action', function(assert) {
                assert.expect(4);
                let controller = this.owner.lookup('controller:posts');
                controller.send('setProps', 'Testing Rocks!');
                assert.equal(controller.propB, 'Testing Rocks!', 'propB updated');
            });
        });
        ```

- Rendering/integration tests
    - Okay speed, test individual rendering
    - Default for components & helpers 
    - Rendering is the new name, but currently ember cli seems to still use integration.
    - Looks like the following:
        ```js
        module('Integration | Helper | format currency', function(hooks) {
            setupRenderingTest(hooks);

            test('formats 199 with $ as currency sign', async function(assert) {
                this.set('value', 199);
                this.set('sign', '$');
                await render(hbs`{{format-currency value sign=sign}}`);
                assert.equal(this.element.textContent.trim(), '$1.99');
            });
        });
        ```

- Application/acceptance tests
    - Slow, tests the total package
    - View how different things interact with eachother.
    - Application is the new name, but currently ember cli seems to still use acceptance.
    - Looks like the following:
        ```js
        module('Acceptance | posts', function(hooks) {
            setupApplicationTest(hooks);

            test('should add new post', async function(assert) {
                await visit('/posts/new');
                await fillIn('[data-test-field="Title"]', 'My new post');
                await click('[data-test-button="Save"]');
                assert
                    .dom('[data-test-post-title]')
                    .hasText('My new post', 'The user sees the correct title.');
            });
        });
        ```


#### Testing route-specific rendering
Imagine this: you have a route that works independant of anything else. A good example would be a contact form!
Testing its internal stuff would be ideal for unit tests. But what if you wanna test the *rendering* of the route, independant of the rest of the application?

Unit tests are out of the question: they do not have any rendering capabilities. Unit route testing is for internal functions & actions, not the rendering.

So instead, with the information gained from the Ember docs/written above, my instinct would say "rendering test". But I haven't been able to see any way to render routes independently à la ```render(hbs`{{route}}`)```.


So acceptance testing is the only option left, but Ember doesn't suggest nor facilitate route-specific acceptance testing by default. `ember generate route-test` creates a unit test. The auto-generated test is also unit. The `visit` helper (that seems to be the main method of selecting & rendering what to test in acceptance testing) does *not* take a route name. It only takes a path, and that would be an incredibly shakey way to test. I did find [a workaround for this](#ember-test-helper-visit-with-route-name), so at the moment that seems like the 'best' option.



---



## Commands

```sh
# Install Ember-cli
npm install -g ember-cli

# New project with embroider enabled
ember new $projectname --no-welcome --embroider

# Run Ember project
ember server  
ember s

# Common ember generators
ember generate {route,template,controller,service,helper} $routename
ember g {route,template,controller,service,helper} $routename

# Generate component...
ember g component $componentname  # ...hbs
ember g component $componentname --with-component-class  # ...hbs & .js
ember g component-class $componentclassname

# Generate dynamic route
ember g route bands/band --path=':id'

# Destroy route (removes corresponding routes/, templates/, tests/)
ember destroy route $routename

# Test
ember test
ember t
ember t --server  # Open a browser & rerun tests with every change

ember g acceptance-test $testname

# Mirage (back-end mocking)
ember install ember-cli-mirage
ember g mirage-model $modelname
```

## How-to

### Tracked property
- "If a property is used in the template, it should be marked as tracked"
- Getters (in components at least) are auto-tracked
- Make sure to track the deepest object:
    - `this.storage.bands.push()` --> `this.storage` doesn't get changed, so tracking that won't work; track `this.storage.bands` instead.
    - You might have to use `this.storage.bands = tracked([])`, which you can get after running and importing `ember i tracked-built-ins`

Also, in my personal experience, objects + tracked-built-ins works the best. But I might be goofing things.

```js
import { tracked } from '@glimmer/tracking';

class Band {
    @tracked name;
    // ...
```

### LinkTo dynamic path/model
```handlebars
{{!-- LinkTo using a single dynamic segment --}}
<LinkTo @route="bands.band.songs" @model={{band.id}}>
    {{band.name}}
</LinkTo>

{{!-- LinkTo using multiple dynamic segments --}}
<LinkTo @route="bands.band.songs" @model={{array band1.id, band2.id}}>
    {{band.name}}
</LinkTo>
```

### Component with optional block
```handlebars
{{#if (has-block)}}
    {{yield this.data}}
{{else}}
    {{#each this.data as |data|}}
        <input type="text"
            placeholder={{data}}
        />
    {{/each}}
{{/if}}
```

### Install & use a CSS framework through NPM & PostCSS
```sh
npm install -D ember-cli-postcss  # Allows using postcss
npm install -D $npm-css-framework
```

```js
// ember-cli-build.js
const EmberApp = require('ember-cli/lib/broccoli/ember-app');

module.exports = function (defaults) {
    let app = new EmberApp(defaults, {
        // ...
        postcssOptions: {
            compile: {
                plugins: [{ module: require('tailwindcss') }],
            },
        },
        // ...
    });
    // ...
}
// ...
```

```css
/* app/styles/app.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### Router service
```js
import { inject as service } from '@ember/service';

export default class NewController extends Controller {
    @service router;

    @action
    save() {
        // ...
        this.router.transitionTo('models.model.property', model.id);
    }
}
```

### Query parameters
```js
export default class BandsBandSongsRoute extends Route {
    // ...
    queryParams = {
        sortBy: {
            as: 's',
        }
    }
    // ...
}
```

### Change controller value from a component
Components are isolated, but you can still do this with a bit of a roundabout way:
```hbs
<!-- Template with controller -->
 <SortButton
    @sortBy={{fn (set this 'sortBy' '-rating')}}
/>

<!-- Component -->
<button 
    type="button" 
    {{on "click" (fn @sortBy)}}
></button>
```

### Testing
Use `ember install ember-test-selectors`! It puts data-test- automatically on components, but strips those in production.

For assert.dom() functions, check the [qunit-dom api docs](https://github.com/mainmatter/qunit-dom/blob/master/API.md).

#### Ember Test Helper visit with route name
```js
let router = this.owner.lookup('service:router');
await visit(router.urlFor('route.name'));
```


#### Custom helper function

```js
export function testSelector(name, psuedoClass = '') {
  let selector = `[data-test-rr="${name}"]`;
  if (psuedoClass != '') {
    selector += `${psuedoClass}`;
  }
  return selector;
}

export async function dataTestSteps(...args) {
  for (let i = 0; i < args.length; i += 2) {
    let func = args[i];
    let target = args[i + 1];
    let selector;
    if (target.startsWith('[')) {
      selector = target;
    } else {
      selector = testSelector(target);
    }

    // If no additional parameter
    if (typeof args[i + 2] === 'function') {
      await func(selector);
    }
    // If additional parameter
    else {
      await func(selector, args[i + 2]);
      i++; // Skip the additional parameter next step
    }
  }
}

/* This code allows the following syntax:
    await dataTestSteps(
      click,
      testSelector('band-link', ':first-child'),
      click,
      'songs-nav-item'
    );
*/
```

### Using a helper in templates & js simulteanously
```js
// app/helpers/example.js
// Original: export default helper(function example(positional) {
// Replace with:
export function example(positional)  {
    // ...
    return "";
}

export default helper(example);

```
Source: [Rarwe](https://balinterdi.com/rock-and-roll-with-emberjs/)


### Deploying on surge using ember-cli-surge
```yaml
    - name: Use ember-cli-surge to build & deploy
      env:
        SURGE_LOGIN: ${{ secrets.surgeLogin }}
        SURGE_TOKEN: ${{ secrets.surgeToken }}
      run: ./deploy.sh
      shell: bash
```
```bash
#!/bin/bash
# This fixes ember surge cli not auto exiting, thus forever keeping the GitHub Action going
npx ember surge &
sleep 180
```


## Links
- Ember Inspector for [Chrome](https://chrome.google.com/webstore/detail/ember-inspector/bmdblncegkenkacieihfhpjfppoconhi) & [Firefox](https://addons.mozilla.org/en-US/firefox/addon/ember-inspector/)
- Thanks to [Rock And Roll with Ember Octane](https://balinterdi.com/rock-and-roll-with-emberjs/)
