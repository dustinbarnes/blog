---
id: 12fa-08-concurrency
title: Concurrency
desc: ''
updated: 1680890967400
created: 1680548436398
---
> Scale out via the process model

- Historically we scaled via OS processes
    - Large virtual machines like JVM favour allocating one large uberprocess and facilitating concurrency via threads.
- [[tech.practice.12-factor-app.processes]] are a first-class citizen.
    - HTTP requests via a web process, long-running background tasks via worker processes.
- Individual [[tech.practice.12-factor-app.processes]] can do their own multiplexing using threads/green threads.
    - Share-nothing, horizontally partitionable process model simplifies scaling out.
- Never daemonise or write PID files.
- Rely on the OS process manager (e.g. systemd), cloud platform, etc.
    - The process manager should take care of output streams, crashed processes and operator-initiated events like restarts/terminations.
