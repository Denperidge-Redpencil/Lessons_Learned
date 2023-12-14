# 07_Current_Implementation

Navigation:
- [..](../)
- [06_SIDEQUEST_AnyArgs](06_SIDEQUEST_AnyArgs.md)

## Info
- Date: Current 
- Repo (1): https://github.com/mu-semtech/repo-harvester
- Repo (2): https://github.com/denperidge-redpencil/divio-docs-parser
- Repo (3): https://github.com/Denperidge-Redpencil/semantic.works
- Repo (4): https://github.com/Denperidge-Redpencil/app-mu-info-rework/
- Deployment: https://dev.info.mu.semte.ch/

Repo-Collector & app-mu-info-archive got reworked and split up into a few projects which I hold dear.
This implementation has actually already been described.

Please see the document at [Denperidge-Redpencil/du-project@project/discussions/documentation-structure](https://github.com/Denperidge-Redpencil/du-project/blob/master/docs/discussions/documentation-structure.md#implementation).

## Things it did well:
Everything marked in the table!
- Versioned & parsed documentation.
- Increased accessibility & responsiveness.
- Dogfooding!

## Things it lacked:
- Minimising & optimising. As mentioned by [@madnificent](https://github.com/madnificent/), there's a decent amount of abstractions.
  These could - and perhaps should - be minimised down the road.
- More testing & stability. It feels too finicky at times.
