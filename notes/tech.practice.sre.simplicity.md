---
id: sre-seeks-simplicity
title: SRE Seeks Simplicity
desc: ''
updated: 1680754458798
created: 1680754354068
---
Simplicity is efficiency.

John Gall:

> A complex system that works is invariably found to have evolved from a simple system that worked. A complex system, designed from scratch never works and cannot be patched to make it work. You have to start over, beginning with a simple working system.

Measures of complexity

- Cylomatic complexity measures the number of paths (in the form of branches and loops) through software source code.
- Training time for new hires.
- How much explanation time is required for a comprehensive, high-level overview.
- How much administrative diversity is enabled through configuration.
- The diversity between deployed environments.
- Age of the system, per Hyrum's Law.

Rules of thumb

- Deal with complexity upfront (e.g. API design, typing and documentation for forward and backward compatibility).
- Consider the full lifecycle of rewrite projects, including developing against a moving target, a full migration plan and additional costs incurred during migration.
