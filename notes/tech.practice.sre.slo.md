---
id: service-level-objectives
title: Service-Level Objectives
desc: ''
updated: 1680754211870
created: 1680754070854
---
Service Level Objectives specify a target level of reliability for a service by applying SLIs.

SLOs provide a means of evaluating user happiness with a service's performance. Objectives should be flexible: high opportunity costs should drive loosening of an SLO:

- Above the threshold, almost all users are happy with the service.
- Below the threshold, users are likely to complain or stop using the service.

Relationships to SLAs

SLAs are contractual obligations, and usually incorporate a set of penalities for failure to comply. These should be based upon, but more pessimistic than, your SLOs.

Defining happiness

User happiness:

- drives service use;
- drives revenue growth;
- reduces strain on customer service; and
- generates recommendations.

Don't aim for 100%

100% availability is an unrealistic target:

- There's a non-zero chance that critical infrastructure components will fail simultaneously.
- Your customers won't achieve beyond around 99.99% availability.
- You can never ship improvements to the service: the number one source of outages is change.
- An SLO of 100% forces you to only ever behave in a reactive fashion.

Expression

SLOs are expressed in the form:

SLI < target
lower bound <= SLI <= upper bound

Where curves matter, specify multiple SLOs:

    90% of all RPC calls will complete in 1ms.
    99% of all RPC calls will complete in 10ms.
    99.9% of all RPC calls will complete in 100ms.

Choosing SLOs isn't a purely technical exercise, though SREs can add value to the conversation. Work for both the product development and SRE teams should be based on them. Rules of thumb:

    Past success isn't an indicator of future success. Don't base targets on current performance; things can worsen as load increases in ways which are not yet clear.
    Keep it simple; we need to be able to reason about changes to these indicators.
    Avoid absolutes (always available, infinitely scalable).
    Focus: have as few SLOs as possible: if you can't prioritise work based on an SLO it isn't valuable.
    Perfection can wait, and there's value in having these values now. Start loose and refine over time.
    Have a safety margin. Keeping a tighter SLO internally than the one advertised externally allows room for change without disappointing users.
    Don't over-achieve: it may be necessary to cause synthetic outages in highly available services to ensure users don't come to depend on them exceeding their SLOs.

Service Level Agreements

SLOs should form the basis of informed Service Level Agreements with our customers. Our SLOs should be more optimistic than SLAs agreed with customers.
Setting SLOs

Remember the well-known compliance statement -- allow room for performance to slip, and tighten the SLO later:

    Past performance is not an indicator of future reliability.

    Choose an SLI specification.
    Define an SLI implementation.
    Identify coverage gaps, based on user journeys
        Do the SLIs adequately cover user journeys and their failure modes?
        Are there exceptions or edge cases to cover?
        Do the SLIs capture multiple journeys with differing needs?
    Define aspirational SLOs based on business needs.

Some failure modes should be considered out of scope, especially where they're covered by existing testing methods (integration testing, unit testing, etc.).

risk = probability * impact
risk = time to failure * service downtime

Put another way, risk can be measured in terms of three factors:

    Time to detection.
    Time to resolution.
    Percentage of impacted users.

Document SLOs both in your monitoring system and documentation. Include:

    Why the threshold is where it is.
    Why the SLIs are appropriate.
    Monitoring data deliberately excluded from the SLI.

SLOs require iterative refinement to get right. Consider labelling them as:

    In development first, while narrowing down indicators.
    In testing when they're considered likely correct.
    Actively paging when you're confident that false positives have been eliminated.

Recognise that they're a moving target by specifying versioning from the start. Consider storing this metadata alongside the configuration for the monitoring tool, e.g. in an SCM repository.