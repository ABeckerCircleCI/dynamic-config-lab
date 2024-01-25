---
slug: branch-filtering-and-approvals
id: vmyclmtxajsb
type: challenge
title: Branch Filtering & Manual Approval
teaser: In this challenge, you will learn how to implement branch filtering and approval
  jobs in CircleCI.
notes:
- type: text
  contents: In this challenge, you'll delve into branch filtering and approval jobs.
tabs:
- title: Code Editor
  type: service
  hostname: circleci-dev
  port: 8443
- title: Shell
  type: terminal
  hostname: circleci-dev
difficulty: basic
timelimit: 1500
---

In this challenge, you will learn how to implement branch filtering and approval jobs in CircleCI. You will modify your workflow to ensure that the deploy job only runs on the main branch. Additionally, you will add an approval step to require manual approval before the deploy job runs.

Branch filtering and approval jobs are key features in CircleCI that give you more control over your CI/CD workflows. Branch filtering allows you to specify which branches should trigger a workflow, making it a useful tool for managing different environments like development, staging, and production. Approval jobs, on the other hand, allow you to pause a workflow until manual approval is given, providing an extra layer of control before deploying changes.

Task 1: Implement branch filtering using logic statements
==============
1. Use a [`when`](https://circleci.com/docs/using-branch-filters/#branch-filtering-for-job-steps) step with a `condition` key to implement branch filtering.
1. Set the [`equal`](https://circleci.com/docs/configuration-reference/#logic-statements) key to `[ main, << pipeline.git.branch >> ]` to check if the current branch is the main branch.
1. Add the steps that should run on the main branch inside the `steps` key.

> **Note:** We won't be doing a real deploy here, so a simple echo statement will do.

Here is an example of how to implement this:
```
  - when:
      condition:
        equal: [ main, << pipeline.git.branch >> ]
      steps:
        - run: echo "Run your deployment logic here"
```

> **Note:** Additional [logic statements](https://circleci.com/docs/configuration-reference/#logic-statements) such as `and`, `or`, and `not` are available.

Task 2: Implement branch filtering using job filters
==============
1. Modify your workflow configuration to ensure that the `deploy` job only runs when the main branch is pushed.
1. Use the [`filters`](https://circleci.com/docs/configuration-reference/#filters) key for the `deploy` job.
1. Set the [`branches`](https://circleci.com/docs/configuration-reference/#branches) key to `only: main` to specify that the job should only run on the main branch.

Your config should look something like this.
```
  - deploy:
      filters:
        branches:
          only: main # regex can be used here!
```
> **Note:** Along with `branches`, `tags` is also available to only run jobs on matching tags.

Try pushing this configuration to your repository. Can you see how the workflow looks different this time?

Hint: you might need to switch to a different branch! Run `git checkout -b not-main` if so!

Task 3: Hold a workflow for manual approval
==============
1. Add an [approval](https://circleci.com/docs/workflows/#holding-a-workflow-for-a-manual-approval) job to your workflow configuration, this job does not need to exist in the `jobs:` section.
1. Call this job `hold-for-approval`.
1. Set the [`type`](https://circleci.com/docs/configuration-reference/#type) key to `approval`.
1. Update the `requires` key for the `hold-for-approval` job to include the `test-split-by-filename` job.
1. Update the `requires` key for the `deploy` job to include the `hold-for-approval` job.

Your config should look something like:
```
  - hold-for-approval:
      type: approval
      requires:
        - test-split-by-filename
```
> **Note:** Although they're sometimes called approval _jobs_, they do not provision any compute and do not burn any credits while on hold.

Push your project once again and you'll find you now need to click the approval job in order for the workflow to complete.

> **Note:** The API can also be used to approve these holds.
