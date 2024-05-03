---
layout: post
title: "Using Git Tags for Environments"
date: 2024-05-02
categories: DevSecOps
---

When constructing a DevSecOps pipeline, it is important to consider the traceability from an environment back to the code that is deployed in that environment. When a bug is reported against one of the environments and developers need to locate the exact spot in the code repository that needs fixing, developers need to be able to navigate to the code behind that environment with confidence. Considering that there are likely multiple deployment environments, each containing multiple deployable microservices, this can get confusing very quickly.

# The GitOps Approach

GitOps has emerged as a powerful solution to address this challenge because it directly maps deployment of deployable modules to environments through a separate repository. However, there are times when the technology stack doesn't mesh well with GitOps. With [Serverless Framework][serverless-framework] for example, it is possible to have separate steps for package and deployment, but when using [Serverless Compose][serverless-compose], build-time parameters contain environment-specific information, making it difficult to separate packaging from deployment. In scenarios like this one where a GitOps solution doesn't fit well, the following approaches may be good alternatives.

# Using Git Branches

One approach is to map Git branches to deployment environments using a standardized naming convention (e.g., dev, test, stage, prod). A build pipeline can be set up to deploy code from that branch to a specific environment. As long as the pipeline is triggered automatically whenever the code changes, and the pipeline build is successful, then the code and the environment are synchronized.

This approach is good because it provides a clear mapping between the Git branches and the environments. It is possible to follow a standard promotion path through the environments until production is reached. Unfortunately, it is also very easy for the branches to diverge because it is easy for developers to apply fixes to an environment branch directly, complicating future merges since they cannot be fast-forwarded anymore.

# Using Git Tags

The issues described in the previous section can be avoided by switching from branches to tags. Developers cannot add commits directly to tags and tags can be changed to point to a different commit on a completely different branch without the need to merge. Tags simply point directly to a commit and label it, giving that commit meaning. Git doesn't expect tags to change, so changing them does require the use of the `--force` option. This is because tags are usually used as long-lived labels and are not changed after being created. However, the use of tags described here is still in the spirit of labelling commits that are not intended to accept changes, so the fact that they change to point to new commits occasionally is not a problem.

# Example

Consider a set of tags in a repository pointing to commits where the tags are labeled dev, test, stage, and prod. A DevSecOps pipeline can be constructed to deploy the code from the dev tag to the dev environment whenever the dev tag is updated to point to a new commit. Updating the tag then triggers a deployment to that environment. To help the pipeline tool differentiate these deployment tags from other tags in the repository, a shared prefix may be used (e.g., deploy/dev, deploy/test, deploy/stage, deploy/prod).

Updating the dev deployment then requires the following two Git commands. These take the current commit checked out and update the deploy/dev tag to point to that commit, triggering a deployment to the dev environment.

{% highlight bash %}
git tag --force deploy/dev
git push --force origin deploy/dev
{% endhighlight %}

Since tags simply point to commits, the developers are free to construct their branches however they like. Some may choose to use a main branch with pull requests, while others may opt for git-flow. Regardless of how the branches are configured, the tags will point to the code that is deployed in the named environments.

It is useful to consider a promotion path that incorporates the creation of a release branch, onto which hot-fixes may be applied to maintain a release while the non-prod environments are between releases. In the promotion path of dev > test > stage > prod, the creation of a branch between test and stage enables the testing of a hot-fix in stage before promoting to prod. It is not necessary to create a branch unless a hot-fix is needed, but a tag should be created to label the release and make it easier to create a hot-fix branch.

Promoting code from dev to test requires the following Git commands. This will update the deploy/test tag to point to the same commit as deploy/dev, triggering a deployment to the test environment

{% highlight bash %}
git tag --force deploy/test deploy/dev
git push --force origin deploy/test
{% endhighlight %}

# Conclusion

DevSecOps Pipeline Engineers should ensure that there is clear traceability from each deployment environment back to the source code behind that deployment environment. GitOps is a powerful solution to provide this traceability, but if a GitOps deployment doesn't work for your situation, try using Git tags. It is a quick and easy method that leaves the option to implement GitOps available in the future.

[serverless-framework]: https://www.serverless.com/framework/docs
[serverless-compose]: https://www.serverless.com/framework/docs-guides-compose
