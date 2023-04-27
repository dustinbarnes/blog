The on-call effort is about minimising detection and resolution time. The postmortem process should raise and follow up on action items identified as causes of incidents, reducing time to next failure.

Put in place tools

- Playbooks define a linear set of actions to take in response to types of incident. Aim for one entry per alert.
- Runbooks extend playbooks by containing conditional steps. They may be executed automatically, optionally relying on external human decision making.

Create training roadmaps

It's important to prepare engineers for their on-call shifts, so define an onboarding process. For example:

- Administering production jobs
- Interpreting debug data
- Draining traffic from a cluster
- Rolling back a bad release
- Blocking/rate-limiting unwanted traffic
- Adding capacity: scaling up/out
- Using monitoring systems (alerting, dashboards, logs)
- Describing architecture, key components and dependencies

- Aim to build muscle memory. Running regular "wheel of misfortune" sessions may help teams gain understanding of common failure scenarios.

Review guidelines for process:

- Reading handoff from the previous shift at shift start.
- Minimise impact first, then address underlying cause.
- Escalate when...
- Send handoff to the next shift at shift end.

Review pager load

Alerts should be immediately actionable. Thoroughly test new pages before making them live.

Inputs

- Production:
    - Existing bugs
    - Newly introduced bugs
    - Speed of new bug identification
    - Speed of bug mitigation and removal
- Alerting:
    - Alerting thresholds for paging alerts
    - Introduction of new paging alerts
    - SLO alignment with upstream services' SLOs
- Human processes:
    - Rigour of fixes and bug follow-up
    - Quality of data collected about paging alerts
    - Attention paid to pager load trends
    - Human-initiated production changes
    - Quality of data collected on operational issues, between shifts in handoff
    - Vigilance given to operational overload

Resolutions

- Address bugs as a priority: consider rolling back upon investigation of a bug, fixing it and then rolling forward rather than leaving known-bad releases in production. This reduces impact to the error budget. Roll out features behind flags so that they can easily be disabled when problems arise.
- Consider automating changes currently made by humans, allowing for greater validation and testing.
- Reduce identification delay by changes to alerting and the operational playbook. Practice emergency response, limit the size of releases and log changes. Know when to ask for help.

Scheduling

On-call shifts should be kept short to minimise impact on mental health. Scheduling should take into account personal circumstances in order to be fair, accommodating different social commitments and allowing for swapping of shifts in the short-term. Document the process. Tools like PagerDuty may help.

Time spent on-call should be compensated, either through time off in lieu or monetary reward. Pay attention to local labour laws.