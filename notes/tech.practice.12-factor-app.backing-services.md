---
id: 12fa-04-backing-services
title: Backing Services
desc: ''
updated: 1680547576599
created: 1680546326478
---
> Treat backing services as attached resources

- Backing service is defined as any service the app consumes over the network in normal operation.
    - Datastores (databases, blob storage)
    - Messaging/queueing
    - Mail (SMTP, IMAP)
    - Caching
- No distinction between local and third-party services.
    - They're both just attached resources, accessed via a URL or some other locator using credentials stored in Configuration.
- Misbehaving resources can be replaced without changes to code.

