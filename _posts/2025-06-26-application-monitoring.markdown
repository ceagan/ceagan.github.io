---
layout: post
title: "Application Monitoring"
date: 2025-06-26
categories: Operations Monitoring
---

I've noticed that Application Monitoring vendors and salespeople want to show off all their pie charts and graphs of metrics that they can automatically generate from an application and deployment infrastructure. They claim that this information is perfect for seeing whether your application is working properly or not.

It's not a lie, but it isn't the whole truth either. Consider an application that is steadily working with no user complaints and using 20% of the available memory on a node. The monitoring software will tell you this is great! I would argue that the node is overallocated and you are spending too much money for it. If your memory usage is 90%, the the monitoring software will say this is terrible! I would argue that this node is right-sized.

So what metrics should we be looking at? The most important metrics to monitor for a software application are the ones that measure performance against purpose.

- *Latency:* Measures the time taken to process a request, indicating the application's responsiveness.
- *Throughput:* Tracks the number of requests or transactions processed per unit of time, reflecting the system's capacity.
- *Error Rate:* Captures the frequency of errors or failed requests, providing insight into the application's reliability and stability.

These three metrics together give a comprehensive view of performance, capacity, and reliability, which are critical for ensuring a good user experience and system health. Use other metrics to identify cost savings opportunities. Make adjustments and see how they impact the three critical metrics above.

Don't fall for the marketing trap. Proper gathering of Latency and Throughput metrics for your application require thinking about what matters to your application and your users.
