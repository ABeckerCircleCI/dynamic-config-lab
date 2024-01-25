---
slug: build-and-test
id: u18ifz1qshws
type: challenge
title: build-and-test
teaser: |
  In this challenge, you will learn to create a CircleCI configuration for a Go project from scratch.
notes:
- type: text
  contents: |
    This challenge includes setting up jobs to install dependencies and run tests, and creating a workflow that defines the order of these jobs. You will use a CircleCI's pre-built Go Docker image and learn to use the checkout and run steps. By the end of this challenge, you will have a working CircleCI configuration that can build, test, and deploy a Go project.
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

Jobs are the core component of a CircleCI pipeline. They consists of the commands that CircleCI will run on your behalf, and the execution environment in which to run them.<p>
**Note: hints are included to help clarify proper YAML syntax. You may also check your YAML at any time by clicking the `Check` button; checks will be run against your YAML, and you will receive a message letting you know if anything needs to be corrected. Typically it's simply a missing or superfluous space, hyphen, or colon.** <p>
**Should you wish to move on to the next challenge, but find you are unable to pass all of the checks, you may press the `Skip` button to move on to the next challenge. Your YAML will be updated to align with having passed the first challenge. It is recommended that you copy your existing YAML to an application that will preserve your spacing/indentations, so you can compare what you wrote to what the `Skip` button updates your YAML to.**

Task 1: Install Dependencies Job
==============


1. Click on the `Code Editor` tab, if you are not already in it.

2. Click on the `Explorer` icon in the sidebar, and find the `config.yml` file in the `.circleci` directory. Click it to load it into the Code Editor window.

3. Define a [job](https://circleci.com/docs/configuration-reference/#jobs) named `install-dependencies` using the Docker image `cimg/go:1.21`.
<details>
  <summary>If you're stuck, click here to reveal a hint</summary>
   Hint: <b>install-dependencies:</b> will be subordinate to <b>jobs:</b> and <b>docker:</b> will be subordinate to <b>install-dependencies</b>. <b>- image: cimg/go:1.21:</b> will be subordinate to <b>docker:></b>
</details>

4. Add a [`checkout`](https://circleci.com/docs/configuration-reference/#checkout) step to pull your code onto the CircleCI environment.

5. Add a [`run`](https://circleci.com/docs/configuration-reference/#run) step with the command `go mod download` to download the Go dependencies.
<details>
  <summary>If you're stuck, click here to reveal a hint</summary>
   Hint: <b>step:</b> will be subordinate to <b>jobs:</b> - in the same column as/aligned with <b>docker:</b>, <b>- checkout</b> and <b>- run: go mod download</b> will be subordinate to <b>steps:</b> - in the same column as/aligned with <b>- image: cimg/go:1.21</b>. Remember that lines beginning with a hyphen need the hyphen to be subordinate to the line it's defining above.
</details>

6. Add a [`persist_to_workspace`](https://circleci.com/docs/workspaces/) step to save the dependencies and your project for later jobs. Set the `root` to `~/` and `paths` to `project` and `go/pkg/mod`.

> **Note:** CircleCI provides its own docker images under the `cimg` namespace. These images provide faster spin up times, thanks to a common base of layers that are frequently cached across our cloud executors. You can however use any public or private docker image for your jobs.

Task 2: Test Job
==============

1. Define a `test` job using the Docker image `cimg/go:1.21`. Your formatting should align with that of the `install-dependencies` job.

1. Attach the workspace from the `install-dependencies` job using the `attach_workspace` step and set the `at` parameter to `~/`.

1. Add a run step with the command `gotestsum --junitfile report.xml` to execute your tests and generate a junit XML report.
<details>
  <summary>If you're stuck, click here to reveal a hint</summary>
   Hint: The line containing the run step should look like this: - run: gotestsum --junitfile report.xml
</details>

Task 3: build-test-deploy Workflow
==============

1. Define a [workflow](https://circleci.com/docs/workflows/) named `build-test-deploy`.

1. Add the `install-dependencies` and `test` jobs to this workflow.

1. Specify that `test` [requires](https://circleci.com/docs/workflows/#sequential-job-execution) `install-dependencies` to run successfully before it can start.
<details>
  <summary>If you're stuck, click here to reveal a hint</summary>
   Hint: <b>requires:</b> should be subordinate to <b>- test:</b> and <b>- install dependencies</b> should be subordinate to <b>requires:</b>.
</details>

Task 4: Observe your first workflow
==============

1. Push your code to GitHub and observe your first build on CircleCI.
1. Click Check to confirm that your YAML is valid and completes the requirements.
