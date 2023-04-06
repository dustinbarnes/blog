---
id: 12fa-06-processes
title: Processes
desc: ''
updated: 1680548206070
created: 1680548065391
---
> Execute the app as one or more stateless processes


- Apps are executed in the execution environment as one or more processes.
- Processes are stateless and share nothing.
    - Persistence takes place in stateful Backing services.
- Execution environment memory and filesystem resources are brief, single-transaction caches.
    - Never assume anything is cached in memory and available for future transactions.
    - Relocation will mean starting from a cold cache.
    

No sticky sessions!
