---
id: 12fa-09-disposability
title: Disposability
desc: ''
updated: 1680548917243
created: 1680548666714
---
> Maximize robustness with fast startup and graceful shutdown

- Processes are dispoable.
    - Elastic scaling
    - Rapid deployment of code or Configuration
- Minimise startup time.
    - Ready in second(s) from cold
    - Easier to relocate
    - Easier to use spot instances
- Processes shut down gracefully
    - Stop listening on the service port.
    - Let current transactions complete (draining)
- Processes should be robust against sudden death in case of hardware failure:
    - Queueing backend
    - Return jobs to the queue on failure.


