---
id: 12-factor-app
title: 12-Factor App
desc: ''
updated: 1680939942022
created: 1680511900836
---

In the modern era, software is commonly delivered as a service: called web apps, or software-as-a-service. The twelve-factor app is a methodology for building software-as-a-service apps that:

> - Use declarative formats for setup automation, to minimize time and cost for new developers joining the project;  
> - Have a clean contract with the underlying operating system, offering maximum portability between execution environments;  
> - Are suitable for deployment on modern cloud platforms, obviating the need for servers and systems administration;  
> - Minimize divergence between development and production, enabling continuous deployment for maximum agility;  
> - And can scale up without significant changes to tooling, architecture, or development practices.

The twelve-factor methodology can be applied to apps written in any programming language, and which use any combination of backing services (database, queue, memory cache, etc).

# The 12 Factors
1. [[tech.practice.12-factor-app.codebase]]  
One codebase tracked in revision control, many deploys
2. [[tech.practice.12-factor-app.dependencies]]  
Explicitly declare and isolate dependencies
3. [[tech.practice.12-factor-app.configuration]]  
Store config in the environment
4. [[tech.practice.12-factor-app.backing-services]]  
Treat backing services as attached resources
5. [[tech.practice.12-factor-app.build-release-run]]  
Strictly separate build and run stages
6. [[tech.practice.12-factor-app.processes]]  
Execute the app as one or more stateless processes
7. [[tech.practice.12-factor-app.port-binding]]  
Export services via port binding
8. [[tech.practice.12-factor-app.concurrency]]  
Scale out via the process model
9. [[tech.practice.12-factor-app.disposability]]  
Maximize robustness with fast startup and graceful shutdown
10. [[tech.practice.12-factor-app.dev-prod-parity]]  
Keep development, staging, and production as similar as possible
11. [[tech.practice.12-factor-app.logs]]  
Treat logs as event streams
12. [[tech.practice.12-factor-app.admin-processes]]    
Run admin/management tasks as one-off processes
