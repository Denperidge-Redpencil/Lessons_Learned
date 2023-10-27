# Project creation: masterclass-import @ du-project

This is the second time writing this, as I did not commit and instead removed the original. It hurts.

*Note: the commits listed are of the initial imports, and the final product may differ. For the final product, see [github.com/Denperidge-Redpencil/du-project](https://github.com/Denperidge-Redpencil/du-project)*

## 01: How and why (pt1 & 2)
These contained the design philosophy of semantic.works! I added this in the following format:
1. The core values of the semantic.works stack and their benefits (e.g. the desire for efficient development, maximum freedom and longevity)
2. How our design decisions reflect those values (e.g. the decision for using *micro* services, standard api's, semantic models, centralised communication...)
3. What this resulted in: (e.g. the general proejct layout, using HTTP/JSON:API/SPARQL, Docker (Compose), the categories of repositories...)

Related commit: [2079fcef1724540b7ee95dc5e084ff423f2b7370@github.com/Denperidge-Redpencil/du-project](https://github.com/Denperidge-Redpencil/du-project/commit/2079fcef1724540b7ee95dc5e084ff423f2b7370)


## 01: How and why (pt3)
Information about possible/future expansions for semantic.works. A nice watch, but nothing that should be in the permanent documentation.

Related commit: N/A

## 02: A shared foundation (pt1)
The beginning of this video is a quick overview of Docker Compose, which is not imported.

Afterwards, however, the information about services as well as how to add it into the project is given and imported.

Related commit: [c57348f8c09ec2062f951f915da85d1408f05050@github.com/Denperidge-Redpencil/du-project](https://github.com/Denperidge-Redpencil/du-project/commit/c57348f8c09ec2062f951f915da85d1408f05050)

## 02: A shared foundation (pt2)
This video explains linked data and writing turtle. While eventually a written introduction in the semantic.works docucmentation should be made or linked, it is out of the scope for the current import.

Related commit: N/A

## 02: A shared foundation (pt3)
This video explains Ember.js, and has the same problem as pt2.

However, it should be noted that in the beginning Aad opens this one by saying "The hamster" and that made me very happy.

Related commit: N/A

## 03: Docker for real (pt1-3)
This video series explains Docker, and has the same problem as A shared foundation pt2-3 

## 04: Templates and conventions
FILLED with information! Templates was expanded, the applications category added, and a bunch of naming conventions written down, including of the docker-compose files!

Related commit: [fee309f545f22f01866c5532e8762799cd1b284d@github.com/Denperidge-Redpencil/du-project](https://github.com/Denperidge-Redpencil/du-project/commit/fee309f545f22f01866c5532e8762799cd1b284d)


## 05: Common microservices (pt1-2)
This part highlights the importance of identifier, dispatcher and virtuoso. This has been put into a quickstart how-tos/creating-applications. Some information (like the info about accept headers) and extra configuration options for dispatcher have not been imported, as to keep the how-to to the point.

Related commit: [5a637878d108af7e761e2b50d4ed701312093975@github.com/Denperidge-Redpencil/du-project](https://github.com/Denperidge-Redpencil/du-project/commit/5a637878d108af7e761e2b50d4ed701312093975)

## 05: Common microservices (pt3)
This part mostly talks about implementing mu-cl-resources. It has been added in the form of linking to the documentation.

Related commit: [5a637878d108af7e761e2b50d4ed701312093975@github.com/Denperidge-Redpencil/du-project](https://github.com/Denperidge-Redpencil/du-project/commit/5a637878d108af7e761e2b50d4ed701312093975)

## 05: Common microservices (pt4)
This is about the file service, login services... and documentation generation. I sent a message to check the up-to-dateness of the documentation generator, but aside from that, nothing was imported due to a grander amount of specificity.

Related commit: N/A

## 06: Intro virtuoso and sparql + 07: SPARQLing
These are tutorials for using virtuoso and SPARQL, and thus are out of scope for the current import.

Related commit: N/A

## Extra notes
We should make and/or link (a) tutorial(s) for the following things:
- Docker & Docker Compose
- Linked data & SPARQL
- Ember.js
- Accept headers

## That's it for now!
Now, onto the next step: polishing, refactoring, and deprecating!
