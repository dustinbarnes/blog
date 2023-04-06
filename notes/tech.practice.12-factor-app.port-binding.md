---
id: 12fa-07-port-binding
title: Port Binding
desc: ''
updated: 1680548405053
created: 1680548217331
---
> Export services via port binding.


- Web apps are sometimes executed inside webserver containers.
- Services export their native protocol by binding a port and listening for traffic on it.
    - API Gateway
- This enables one app to be a backing service for another by referencing it in the consuming app's Configuration.

