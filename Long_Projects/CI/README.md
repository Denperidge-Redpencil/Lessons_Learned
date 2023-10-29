# CI
Redpencil was moving from Drone to Woodpecker due to the formers' license change (see [Drone, Haskell & Woodpecker header below](#drone-haskell-and-woodpecker)), frustrated with the vendor lock in and the migration process and difficulty updating every single repo. Here are some notes about [CI platforms](#platform-notes), and subsequent projects to facilitate [CI migration & usage](#projects) 

## Platform notes

### GitHub Actions
I love GitHub Actions. I don't make a profit of my personal projects, and Actions allows me to build & deploy to Pages and Surge without having to worry about building everything locally and then ftp/ssh'ing it into another location. I bring that knowledge into my professional work, because streamlining & automating development is a very worthwhile pursuit.

That being said, if GitHub decides to change Actions/Pages, that would not be ideal, so I feel apprehensive about recommending it too hard. Not to mention that it does lock you into the GitHub ecosystem. Moving your git hosting will not allow you to easily keep using GH Actions.

#### Locally running Actions
This is more of a band-aid if anything in terms of safety around GitHub Actions (as it is merely a local implementation of GitHub Actions), but it's still good to write down: https://github.com/nektos/act.

### Drone, Haskell and Woodpecker
- Woodpecker is an open source fork from the last Drone commit before their license became proprietary.
- Drone =/= Harness, however, Harness *did* [buy Drone](https://www.prnewswire.com/news-releases/harness-acquires-continuous-integration-pioneer-droneio-and-commits-to-open-source-301106473.html) and Drone is becoming Harness Community Edition.
- Gitea.com notes that it uses gitea to developer gitea which is great and it also notes that this practice is called [dogfooding](https://en.wikipedia.org/wiki/Eating_your_own_dog_food) which brightens my day.


## Projects
### CICD-Templates
https://github.com/Denperidge-Redpencil/cicd-templates

A decent amount of developers don't have the time or desire to learn devops. To help alleviate this, CICD templates was a collection of Drone & Woodpecker files and a README that gave human readable descriptions and click-throughs to any files you may have needed.

### CICD-Tester
https://github.com/Denperidge-Redpencil/cicd-tester

This one is mostly a concept, and is unfinished to the point of having an accidental Ember project within it. The idea was a super weird dogfooding: running every CICD-Templates file against a CI system to ensure their functionality. However, the tests that would need to be written alongside having example repos to run them against would be quite substantial. The project was shelved for this reason.

### CI-Quickstart
https://github.com/Denperidge-Redpencil/ci-quickstart

This is a combination of CICD-Templates and allowing easier updates. A one-line command with a console interface, allowing for quick choosing & automatic adding of CI templates. I quite like this concept, but it is at this moment unfinished.
