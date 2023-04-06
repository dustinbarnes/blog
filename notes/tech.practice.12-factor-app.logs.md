---
id: 12fa-11-logs
title: Logs
desc: ''
updated: 1680549346026
created: 1680549178330
---
> Treat logs as event streams

- Logs provide insight into a running app's behaviour.
- Logs are streams of aggregated, time-ordered events collected from output streams of Processes and Backing services.
    - Raw form is typically text.
    - We can mutate them into more structured data formats.
    - No fixed beginning or end.
- Apps don't concern themselves with the routing or storage of their output.
    - They write to stdout.
    - Local execution during development allows developers to see the logs via their terminal.
    - In deployed environments we can capture and retain this stream.
        
- The event stream is handled by the deployment environment, enabling:
    - Finding historical events.
    - Graphing trends.
    - Active alerting.
