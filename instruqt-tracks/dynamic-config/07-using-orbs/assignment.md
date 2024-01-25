---
slug: using-orbs
id: vxcg852dendf
type: challenge
title: Simplifying the configuration with the Go orb
teaser: Learn how to optimize your CircleCI workflows by using the Go orb to replace
  manual Docker image definitions and cache restoration steps.
notes:
- type: text
  contents: In this challenge, you'll learn how to implement and effectively use CircleCI
    orbs.
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

Orbs are reusable snippets of code that help to automate repeated processes, speed up project setup, and make it easy to integrate with third-party tools. They are a key component of CircleCI's configuration process, and understanding how to use them can greatly simplify and streamline your CI/CD workflows.

In this lab, you will learn how to utilize the go orb in CircleCI. Specifically, you will replace the manual definition of the Docker image for each of your jobs with an executor called `default` included in the orb. Furthermore, you will use the `mod-download-cached` command to replace some manual steps.

You can find the documentation for this orb [here](https://circleci.com/developer/orbs/orb/circleci/go).
This documentation will also include examples of how you might use the orb.

CircleCI has thousands of orbs available to browse on the orb registry.

Task 1: Import the go orb
==============
1. Import the go orb at the top of your configuration file, underneath the `version` tag, using the `orbs` key.
1. Use the `circleci/go@1.9.0` orb string to import the go orb.
1. The import statement should look like:
  ```
  orbs:
    go: circleci/go@1.9.0
  ```

Task 2: Use an executor from the go orb
==============
1. Replace the manual definition of the Docker image for each of your jobs with the `default` [executor](https://circleci.com/developer/orbs/orb/circleci/go#executors-default) from the go orb.
1. Use the `executor` key in your job configuration to specify the executor.
1. Set the `name` key to `go/default` to refer to the `default` executor.
1. Set the `tag` parameter to `1.21`.
1. The executor configuration should look like:
  ```
  executor:
    name: go/default
    tag: '1.21'
  ```
1. Each component of the orb and the corresponding parameters can be found in the orbs [documentation](https://circleci.com/developer/orbs/orb/circleci/go).


Task 3: Use a command from the go orb
==============
1. Replace the manual steps in the `install-dependencies` job with the `mod-download-cached` [command](https://circleci.com/developer/orbs/orb/circleci/go#commands-mod-download-cached) from the go orb.
   - The `mod-download-cached` command handles installing the go dependencies and caching them.
1. Remove the `restore_cache`, `run`, and `save_cache` steps from the `install-dependencies` job.
1. The `install-dependencies` job should now look like:
  ```
  install-dependencies:
    steps:
      - go/mod-download-cached
  ```

Task 4: Wrap up
==============
1. Note that some orbs also contain fully packaged jobs that can be directly inserted into a workflow.
1. Re-usable executors, commands, and jobs can also be used independently within your configuration using the `commands`, `executors`, and `jobs` keys.
1. These concepts help to simplify and reduce redundancy in your configuration.
1. Consider exploring these topics further in a separate training that covers creating your own orbs for public and private use.
