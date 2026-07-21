---
layout: post
title: "Application Monitoring"
date: 2025-06-26
categories: Operations Monitoring
---

I've noticed that Application Monitoring vendors and salespeople love to show off all their pie charts and graphs of metrics that they can automatically generate from an application and its deployment infrastructure. They claim this information is perfect for determining whether your application is working properly.

It's not a lie, but it isn't the whole truth either. Consider an application that is running steadily with no user complaints and using only 20% of the available memory on a node. The monitoring software will tell you this is great! I would argue that the node is over-allocated, and you are spending too much money on it. If your memory usage is 90%, the monitoring software will say this is terrible! I would argue that this node is right-sized.

So what metrics should we actually be looking at? The most important metrics to monitor for a software application are the ones that measure performance against purpose:

- **Latency**: The time taken to process a request — a direct measure of the application's responsiveness.
- **Throughput**: The number of requests or transactions processed per unit of time — a measure of the system's capacity.
- **Error Rate**: The frequency of errors or failed requests — a measure of the application's reliability and stability.

These three metrics together give a comprehensive view of performance, capacity, and reliability. They are the ones that truly matter for user experience and system health. Use other metrics (CPU, memory, disk, network, etc.) primarily to identify cost-saving opportunities. Make adjustments and then check how those changes affect the three critical metrics above.

Don't fall for the marketing trap. Properly gathering Latency and Throughput metrics for *your* application requires deliberate thought about what actually matters to your application and your users.
