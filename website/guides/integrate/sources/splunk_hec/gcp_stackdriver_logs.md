---
last_modified_on: "2020-04-01"
$schema: "/.meta/.schemas/guides.json"
title: "Send logs from Splunk HEC to GCP Stackdriver"
description: "A simple guide to send logs from Splunk HEC to GCP Stackdriver in just a few minutes."
author_github: https://github.com/binarylogic
cover_label: "Splunk HEC to GCP Stackdriver Logs Integration"
tags: ["type: tutorial","domain: sources","domain: sinks","source: splunk_hec","sink: gcp_stackdriver_logs"]
hide_pagination: true
---

import ConfigExample from '@site/src/components/ConfigExample';
import InstallationCommand from '@site/src/components/InstallationCommand';
import Jump from '@site/src/components/Jump';
import ServiceDiagram from '@site/src/components/ServiceDiagram';
import Steps from '@site/src/components/Steps';

Logs are an _essential_ part of observing any
service; without them you are flying blind. But collecting and analyzing them
can be a real challenge -- especially at scale. Not only do you need to solve
the basic task of collecting your logs, but you must do it
in a reliable, performant, and robust manner. Nothing is more frustrating than
having your logs pipeline fall on it's face during an
outage, or even worse, disrupt more important services!

Fear not! In this guide we'll show you how to send send logs from [Splunk HEC][urls.splunk_hec] to [GCP Stackdriver][urls.gcp_stackdriver]
and build a logs pipeline that will be the backbone of
your observability strategy.

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the template located at:

     website/guides/integrate/sources/splunk_hec/gcp_stackdriver_logs.md.erb
-->

## Background

### What is Splunk HEC?

The [Splunk HTTP Event Collector (HEC)][urls.splunk_hec] is a fast and efficient way to send data to Splunk Enterprise and Splunk Cloud. Notably, HEC enables you to send data over HTTP (or HTTPS) directly to Splunk Enterprise or Splunk Cloud from your application.

### What is GCP Stackdriver Logs?

[Stackdriver][urls.gcp_stackdriver] is Google Cloud's embedded observability suite designed to monitor, troubleshoot, and improve cloud infrastructure, software, and application performance. Stackdriver enables you to efficiently build and run workloads, keeping applications performant and available.

## Strategy

### How This Guide Works

We'll be using [Vector][urls.vector_website] to accomplish this task. Vector
is a [popular][urls.vector_stars], lightweight, and
[ultra-fast][urls.vector_performance] utility for building observability
pipelines. It's written in [Rust][urls.rust], making it memory safe and
reliable. We'll be deploying Vector as a
[service][docs.strategies#service].

The [service deployment strategy][docs.strategies#service] treats Vector like a
separate service. It is desigend to receive data from an upstream source and
fan-out to one or more destinations.
For this guide, Vector will receive data from
Splunk HEC via Vector's
[`splunk_hec` source][docs.sources.splunk_hec].
The following diagram demonstrates how it works.

<ServiceDiagram
  platformName={null}
  sourceName={"splunk_hec"}
  sinkName={"gcp_stackdriver_logs"} />

### What We'll Accomplish

To be clear, here's everything we'll accomplish in this short guide:

<ol className="list--checks list--flush">
  <li>
    Accept log data just like the Splunk HTTP event collector.
    <ol>
      <li>Automatically parse incoming data into structured events.</li>
      <li>Optionally require authentication on all requests.</li>
    </ol>
  </li>
  <li>
    Send logs to GCP Stackdriver.
    <ol>
      <li>Leverage any of GCP's IAM strategies.</li>
      <li>Batch data to maximize throughput.</li>
      <li>Automatically retry failed requests, with backoff.</li>
      <li>Buffer your data in-memory or on-disk for performance and durability.</li>
    </ol>
  </li>
  <li className="list--li--arrow list--li--pink text--bold">All in just a few minutes!</li>
</ol>

## Tutorial

<Steps headingDepth={3}>
<ol>
<li>

### Install Vector

<InstallationCommand />

</li>
<li>

### Configure Vector

<ConfigExample
  format="toml"
  path="vector.toml"
  sourceName={"splunk_hec"}
  sinkName={"gcp_stackdriver_logs"} />

</li>
<li>

### Start Vector

```bash
vector --config vector.toml
```

That's it! Simple and to the point. Hit `ctrl+c` to exit.

</li>
</ol>
</Steps>

## Next Steps

Vector is _powerful_ utility and we're just scratching the surface in this
guide. Here are a few pages we recommend that demonstrate the power and
flexibility of Vector:

<Jump to="https://github.com/timberio/vector" leftIcon="github" target="_blank">
  <div className="title">Vector Github repo <span className="badge badge--primary"><i className="feather icon-star"></i> 4k</span></div>
  <div className="sub-title">Vector is free and open-source!</div>
</Jump>

<Jump to="/guides/getting-started/" leftIcon="book">
  <div className="title">Vector getting started series</div>
  <div className="sub-title">Go from zero to production in under 10 minutes!</div>
</Jump>

<Jump to="/docs/about/what-is-vector/" leftIcon="book">
  <div className="title">Vector documentation</div>
  <div className="sub-title">Thoughtful, detailed docs that respect your time.</div>
</Jump>


[docs.sources.splunk_hec]: /docs/reference/sources/splunk_hec/
[docs.strategies#service]: /docs/setup/deployment/strategies/#service
[urls.gcp_stackdriver]: https://cloud.google.com/products/operations
[urls.rust]: https://www.rust-lang.org/
[urls.splunk_hec]: http://dev.splunk.com/view/event-collector/SP-CAAAE6M
[urls.vector_performance]: https://vector.dev/#performance
[urls.vector_stars]: https://github.com/timberio/vector/stargazers
[urls.vector_website]: https://vector.dev