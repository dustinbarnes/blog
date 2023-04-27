> Treat logs as event streams

- Logs provide insight into a running app's behaviour.
- Logs are streams of aggregated, time-ordered events collected from output streams of [[tech.practice.12-factor-app.processes]] and [[tech.practice.12-factor-app.backing-services]].
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
