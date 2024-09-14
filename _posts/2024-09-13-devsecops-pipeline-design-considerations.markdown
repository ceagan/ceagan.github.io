---
layout: post
title: "DevSecOps Pipeline Design Considerations"
date: 2024-09-13
categories: DevSecOps
---

After years of writing DevSecOps Pipeline jobs, I have come to rely on a few key elements that I use when building new pipelines or improving existing ones. The main goals for all of these is to optimize the jobs for performance so that the cycle time is reduced, giving developers the feedback they need as soon as possible.

1. Work offline
2. Test failures fail the build
3. Static Code Analysis quality gate failures do not fail branch builds
4. Gather all environment information automatically
5. Pipelines are consistent over time

# Work Offline

Build pipelines should be as fast as possible. Any type of input/output will slow down the build process and in the worst case, can cause builds to fail due to external issues. While reads and writes to disk and console output are worth considering, the most latency comes from network traffic.

The most common cause of network traffic is transfer of build dependencies from package managers. It is important to cache dependencies so that software can be built when the upstream package manager's site is unavailable. Dependencies should be provided directly to the build system without any network communication for optimum performance. When this option is not possible, load dependencies from a service on the local network that maintains a local cache.

When relying on network services for build steps, consider whether the results of those steps are critical to the pipeline. It may be worth allowing the pipeline to continue regardless of the results of those steps. If the results of the step do not affect the main pipeline, the step can be executed in parallel to the main pipeline and any results reported independently without holding up the main pipeline.

# Tests

Tests are critical validation points that provide assurance that software is operating as expected and should never be ignored. Ensure that tests are run with every build and fail the build when a test fails. If tests are inconsistent or failing, resist the temptation to allow pipelines to continue with broken tests, and ensure that developers are not pressured to disable tests just to make the pipeline work.

# Static Code Analysis

Code containing issues related to cognitive complexity, security, and maintainability requires attention to detail by developers throughout the software development lifecycle. Optimally, issues in this area should be identified and resolved before the code is committed to the repository. If the code does reach the repository, then it can be caught and addressed in a pull request. Configure pull request pipelines to run static code analysis and fail builds that introduce **new** issues.

Builds of branches may run static code analysis as well, but the pipeline should continue regardless of the issues found during these scans. No new code is being introduced without going through a pull request first, so any new issues found by static code analysis in these pipelines is due to new rules introduced by the static code analyzer since the code was last scanned. These issues need to be addressed, but failing the build because of them would result in blocking deployment of unrelated new code. The fact is that the released code already has these issues, so any new code unrelated to them will not make the release worse and can only make it better. This means that static code analysis can be skipped or parallelized in branch builds to make them faster.

# Gather Environment Information Automatically

Under no circumstances should the pipeline be parameterized where entering parameters is a manual process. Pipeline execution should be triggered by changes in the repository and the pipeline should get all the information it needs from the repository. It may perform lookups in other systems, like a secrets manager, based on references in the source repository, but the repository should be as independent as possible.

This configuration ensures that the source code fully directs the pipeline, which leads into the next item.

# Consistent Executions Over Time

Insofar as is possible, pipelines executed on one day should produce the same result as the same pipeline with no change to the source code on another day. Properly following the previous items listed here will help accomplish this, but there are other cases that require developers to adjust their workflows to ensure that pipelines are consistent.

When specifying version dependencies, package managers allow for loose versions where a minor version change is still considered acceptable and compatible with the application. To improve consistency of builds, I strongly recommend that developers specify explicit versions for applications, and use loose version specifiers for libraries. This allows applications to specify newer versions while maintaining compatibility with library dependencies. It is important for developers to control the versions of their direct dependencies more explicitly to ensure that all developers use the same versions at all times and that the pipelines do too. Lock files only address this for the pipelines. As soon as a developer installs a new package, the lock file is updated and all other developers must update their installed dependencies to match when the pull updates that include an updated lock file.

# Summary

The primary goal of a pipeline is to automate the process of taking source code and deploying it so that it may be distributed or accessed. In the course of doing this, a pipeline may be implemented to perform verification and validation of the source code to avoid letting defects into the deployment artifacts. However, any automated verification and validation that delays deployment excessively increases the cycle time for developers who need to access the deployment for manual verification and validation. Faster pipelines reduce cycle times and improve overall development team output.
