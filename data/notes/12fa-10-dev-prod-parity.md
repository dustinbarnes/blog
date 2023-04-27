> Keep development, staging, and production as similar as possible.

- Gaps between development and production manifest in three areas:
    - **The time gap:** A developer may work on code that takes days, weeks, or even months to go into production.
    - **The personnel gap:** Developers write code, ops engineers deploy it.
    - **The tools gap:** Developers may be using a stack like Nginx, SQLite, and OS X, while the production deploy uses Apache, MySQL, and Linux.

- Apps are designed for Continuous Delivery by shrinking these gaps.
    - Make the time gap small: a developer may write code and have it deployed hours or even just minutes later.
    - Make the personnel gap small: developers who wrote code are closely involved in deploying it and watching its behavior in production.
    - Make the tools gap small: keep development and production as similar as possible.

Gaps === risk
