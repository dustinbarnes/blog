> Execute the app as one or more stateless processes


- Apps are executed in the execution environment as one or more processes.
- Processes are stateless and share nothing.
    - Persistence takes place in stateful [[tech.practice.12-factor-app.backing-services]].
- Execution environment memory and filesystem resources are brief, single-transaction caches.
    - Never assume anything is cached in memory and available for future transactions.
    - Relocation will mean starting from a cold cache.
    

No sticky sessions!
