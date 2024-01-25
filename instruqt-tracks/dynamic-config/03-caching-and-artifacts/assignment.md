---
slug: caching-and-artifacts
id: 1qzelp1x5jrw
type: challenge
title: Caching, Test Results, and Artifact Storage
teaser: Take your CircleCI pipeline to the next level by incorporating essential features
  like caching, test result storage, and artifact storage.
notes:
- type: text
  contents: In this challenge, you'll be tasked with improving an existing CircleCI
    pipeline by incorporating three key features - caching, test results storage,
    and artifact storage. This challenge will help you understand the benefits of
    these features and how to correctly implement them in a CircleCI pipeline. By
    the end of this challenge, you'll have a more robust and efficient pipeline.
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

Your next task is to improve the existing CircleCI pipeline by incorporating caching, storing test results, and artifact storage.

Task 1: Caching
==============

1. In the `install-dependencies` job, add the [`restore_cache`](https://circleci.com/docs/configuration-reference/#restorecache) step before `go mod download`. This should contain a 'keys:' array followed by a new line with "- go-mod-{{ arch }}-{{ checksum "go.sum" }}"

2. Use the [`save_cache`](https://circleci.com/docs/configuration-reference/#savecache) step after `go mod download`. The save_cache step should have a new line underneath it containing 'key: go-mod-{{ arch }}-{{ checksum "go.sum" }}' and another line after that specifying 'paths:' with a value of '- /home/circleci/go/pkg/mod'.

3. Use a combination of the architecture and a checksum of `go.sum` file as the cache key.

4. Use multiple keys in `restore_cache` to ensure a [partial](https://circleci.com/docs/caching/#restoring-cache) cache restore if an exact match isn't found.

> **Note:** users often prefix cache keys with a number, such as v1-dependencies. This is a convention to easily facilitate starting from a clean cache, by changing the key to a new number e.g v2-dependencies.

Refer to the [CircleCI documentation](https://circleci.com/docs/caching/#using-keys-and-templates) for more information on cache keys.

Task 2: Test Results
==============
1. After running the test command in the `test` job, add a [`store_test_results`](https://circleci.com/docs/configuration-reference/#storetestresults) step to store the `report.xml` file. This will allow you to analyze test execution over time in the CircleCI UI, as well as providing the necessary data to split tests across multiple machines (we'll set this up later).

> **Note:** This project is already using a test runner that supports the [JUnit XML](https://github.com/testmoapp/junitxml) format that CircleCI requires here. For your own projects, you'll need to configure your own test runner to export test results in this format.

Task 3: Artifact Storage
==============
1. Add a [`store_artifacts`](https://circleci.com/docs/configuration-reference/#storeartifacts) step in the `test` job to store the `report.xml` file as an artifact. This can be helpful for debugging and record keeping.

Task 4: Observe your changes
==============
1. Push your latest config to your repository.

2. Check the CircleCI UI to verify that a cache is populated and test results are available.

3. Re-run your build. The `install-dependencies` job should now be faster due to caching.
