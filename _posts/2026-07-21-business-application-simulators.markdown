---
layout: post
title: "Business Application Simulators"
date: 2026-07-21
categories: Development
---

Large Language Models (LLMs) are impressive, but they get off-track, distracted, and give up on tasks without constant guidance. Tools like Claude Code address many of the shortcomings of today's models by applying Agile principles to focus the work and organize tasks among multiple parallel LLM conversations. As the agents simulate the human roles of Project Manager, Quality Assurance Engineer, Architect, and Developer, we can expect software built this way to be as good as what humans have produced using the same methodology.

However, we should not lose sight of the fact that we are training our LLMs to build software in a mirror image of our best ideas about how to organize humans to build. There may be new and different methodologies that are not possible to perform with humans alone in a reasonable amount of time. What could these be?

Our current application development approach incorporates testing to validate that the underlying code does what the specification and users expect it to do, but our test harnesses are often only a small sampling of the scenarios the applications are expected to handle in production. They are based on what engineers think will happen, and when something unexpected happens in production, regression tests are created to make sure it doesn't happen again. Still, there is only so much that can be done.

What if it were possible to more completely simulate the domain in which the software would be used? Could we optimize the test harnesses to test the most important areas? Would we even need a separate test harness at all?

Imagine starting a business and the first piece of software you build is a simulator for the business domain. Include as much detail as possible to model the business and evaluate outcomes. For example, a cupcake business might simulate capital expenditures required for a store, ingredient supply-chain management, product options, power usage, employee retention, and more. Artificial intelligence can help create these simulators in record time. Another AI can then review and tweak the simulator to optimize for customer happiness and profitability.

![Diagram of a cupcake business simulator](/assets/images/cupcake-business-simulator.jpg)

*Example: a cupcake business simulator before any production apps are built*

The actors within the simulation may end up being humans, software applications, or robots in the real world. The software applications created to fill business roles become the applications needed to run the business, targeted specifically to its needs. Human employees may be needed at first, but eventually robots will be able to fill all the roles identified by the simulation.

The existence of the simulator provides a training ground for humans and robots alike. Before soldiers are sent to war, they receive extensive training in both virtual and mock environments. Creating these environments is a necessity to ensure soldiers perform their best in critical situations. Simulations are generally used to train and prepare for the most critical scenarios. AI will lower the cost of creating these simulated environments and enable them to be applied more widely.

There are many more ways AI will be used, and the only sure thing is that they will surprise us. We are currently building models of the systems we understand, modeling the LLMs as humans and coordinating them the way we coordinate humans. There is likely a more efficient way to let the LLMs more directly address the problems we have without having to prescribe these interactions, and we may get there sooner than we think.
