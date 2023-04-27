> Maximize robustness with fast startup and graceful shutdown

- [[tech.practice.12-factor-app.processes]] are dispoable.
    - Elastic scaling
    - Rapid deployment of code or [[tech.practice.12-factor-app.configuration]]
- Minimise startup time.
    - Ready in second(s) from cold
    - Easier to relocate
    - Easier to use spot instances
- [[tech.practice.12-factor-app.processes]] shut down gracefully
    - Stop listening on the service [[port|tech.practice.12-factor-app.port-binding]]
    - Let current transactions complete (draining)
- [[tech.practice.12-factor-app.processes]] should be robust against sudden death in case of hardware failure:
    - Queueing backend
    - Return jobs to the queue on failure.


