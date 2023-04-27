> Store configuration in the environment.


- Configuration is anything that is likely to very between deploys
- Strict separation of configuration from code; no configuration stored in constants.
  - Good litmus test: could we open source this today without compromising credentials?
- Use of files not checked into version control alongside code is best practice.
  - .env/dotenv
