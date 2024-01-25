---
slug: test-splitting
id: cxgq2emsleay
type: challenge
title: Implementing Basic Test Splitting
teaser: In this challenge, you will leverage the power of CircleCI's test splitting
  feature to optimize your pipeline.
notes:
- type: text
  contents: |
    Test splitting allows you to distribute your tests across multiple containers, reducing the overall time it takes to run your tests. By the end of this challenge, you'll have a more efficient pipeline with reduced test runtime, making your development process faster and more efficient.
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

Your next task is to leverage the power of CircleCI's test splitting feature to reduce the run time of your tests. You will create a new job, `test-split-by-filename`, that runs tests in parallel, splitting them by file names.

Task 1: Create a new job
==============
1. Create a new [job](https://circleci.com/docs/jobs-steps/#jobs-overview) named `test-split-by-filename`.
1. Use the same Docker image as the `test` job (`cimg/go:1.21`).

Task 2: Set parallelism and attach workspace.
==============
1. Configure the `test-split-by-filename` job to run in [parallel](https://circleci.com/docs/parallelism-faster-jobs/) across multiple containers by setting the `parallelism` key to `2` in the job configuration.
1. Add a step to attach the [workspace](https://circleci.com/docs/workflows/) at `~/` in the `test-split-by-filename` job configuration.

Task 3: Split tests
==============
1. Add an empty `run:` step to your new job which we'll use to run our tests, split by filename, as detailed below.
1. Use the [`circleci tests glob`](https://circleci.com/docs/use-the-circleci-cli-to-split-tests/#glob-test-files) command within this step to find all Go test files (`*_test.go`) within this step.
1. Pipe the result into [`circleci tests split`](https://circleci.com/docs/use-the-circleci-cli-to-split-tests/#split-tests) and store the output in a variable named `TESTS`.
1. Run the `gotestsum` command as before, this time providing the `$TESTS` variable as a final argument.

The final command should look like:
  ```
  TESTS=$(circleci tests glob *_test.go | circleci tests split)
  gotestsum --junitfile report.xml $TESTS
  ```
> **Note:** The CLI knows which particular machine it is running on via the environment variables `CIRCLE_NODE_INDEX` and `CIRCLE_NODE_TOTAL`.

Task 4: Store test results and artifacts
==============
1. Add steps to store the [test results](https://circleci.com/docs/configuration-reference/#storetestresults) and the `report.xml` file as an artifact in the `test-split-by-filename` job configuration.

Task 5: Add to the workflow
==============
1. Include the `test-split-by-filename` job in the `build-test-deploy` workflow.
1. Set the `test-split-by-filename` job to require the completion of the `install-dependencies` job before running.

Task 6: Observe the results
==============
1. Push your changes to the repository.
1. Compare the runtime of the `test` and `test-split-by-filename` jobs in the pipeline.
1. Observe the `CIRCLE_NODE_INDEX` and `CIRCLE_NODE_TOTAL` environment variables in the "Preparing environment variables" step in the CircleCI UI.
