---
id: service-level-indicators
title: Service-Level Indicators
desc: ''
updated: 1680754043342
created: 1680753596765
---

Service Level Indicators, or SLIs, are ***quantitative*** measures of an aspect of the delivered service.

## SLI types

Base SLOs on metrics that matter to the particular workload:

- Request/response workloads involve clients making requests and awaiting responses.
    - Availability -- either in terms of time or request yield, are we online and reachable?
    - Latency -- how long does it take to return a response?
    - Quality -- what percentage of traffic is gracefully degrading due to overload or partial outages?
- Data processing take records as input, mutate them and emit them somewhere else.
    - Freshness -- how recent the data of the output is in relation to its inputs.
    - Correctness -- the proportion of data generating correct output.
    - Coverage -- the proportion of valid data processed successfully.
    - Throughput -- the proportion of time where the processing rate is faster than a threshold.
- Storage systems accept data for later retrieval.
    - Durability -- the proportion of written records which can be successfully read.
    - Throughput -- the proportion of records that can be written or read within a given time unit.
    - Latency -- the time taken for a written record to be read.

Be judicious in selection: ensure you're targeting things that users value, as there's a human cost to each metric monitored. Systems generally fit into a classification:

- User-facing systems, e.g. search engines, care about availability, latency and throughput.
- Storage systems emphasise latency, availability and durability.
- Big data systems care about throughput and end-to-end latency.
- All systems care about correctness, though this is outside of the SRE purview and usually lies with the product team.

Collection is usually server-side, either using time series data or periodic log analysis. Don't forget the client-side though: richer applications are prone to slowdowns and bugs that won't be detectable from the server-side, and these still hurt user experience.

Use distributions over averages for aggregation to catch long tail. Percentiles (50th, 99th and 99.9th, for instance) will show the average case whilst still allowing us to identify pathological cases.

Standardising indicators allows shared understanding without having to always reason about the meaning of a metric:

- Intervals (averaged over 1 minute)
- Regions (all tasks in a cluster)
- Measurement frequency (every 10 seconds)
- Which transactions are included (HTTP PUTs from tool X)
- Acquisition (server-side, client-side)
- Data-access latency (time to first byte, time to last byte)

Measurement strategies

- Application-level metrics
- Logs processing
- Frontend infrastructure metrics
- Synthetic clients/data
- Client-side instrumentation

Verification

- Can we measure the SLIs we've created where we want to measure them, given this infrastructure?
- Do the SLIs adequately capture the user journey and its failure modes?
- Are there any exceptions or edge cases to consider?
- Do the SLIs capture multiple journeys with differing requirements?
