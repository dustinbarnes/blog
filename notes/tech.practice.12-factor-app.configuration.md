---
id: 12fa-03-configuration
title: Configuration
desc: ''
updated: 1680545783650
created: 1680545634002
---
> Store configuration in the environment.


- Configuration is anything that is likely to very between deploys
- Strict separation of configuration from code; no configuration stored in constants.
  - Good litmus test: could we open source this today without compromising credentials?
- Use of files not checked into version control alongside code is best practice.
  - .env/dotenv
