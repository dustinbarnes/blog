---
id: 12fa-07-port-binding
title: Port Binding
desc: ''
updated: 1680889902594
created: 1680548217331
---
> Export services via port binding.


- Web apps are sometimes executed inside webserver containers.
- Services export their native protocol by binding a port and listening for traffic on it.
    - API Gateway
- This enables one app to be a [[tech.practice.12-factor-app.backing-services]]  for another by referencing it in the consuming app's [[tech.practice.12-factor-app.configuration]].

