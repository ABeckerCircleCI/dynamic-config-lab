---
slug: compute
id: caw8pdvqwkjd
type: challenge
title: Compute Architectures on CircleCI
teaser: Dive into the world of resource classes, compute architectures, and secondary
  containers in CircleCI.
notes:
- type: text
  contents: |
    Learn how to optimize your workflows by customizing your resource classes, experimenting with different compute architectures like Docker and MacOS, and adding secondary containers to your jobs.
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

CircleCI offers a variety of resource classes and compute architectures, including Docker, Linux VMs, MacOS, Windows, and ARM, as well as providing the ability for you to run jobs on your own infrastructure. These options allow you to tailor your CI/CD workflows to your specific needs and optimize your resource usage. In this lab, we will focus on Docker and MacOS VMs.

Task 1: Customizing the Resource Class
==============
1. Customize the [`resource_class`](https://circleci.com/docs/resource-class-overview/) for the `install-dependencies` job.
1. Set the `resource_class` to `small` to provision a smaller container for this job.
1. Push your changes to your repository. Once the `install-dependencies` job runs with the newly designated resource class, you'll be able to observe the changes in CPU and RAM usage for the job on the "resources" tab in the CircleCI UI.

Task 2: Installing Dependencies on MacOS
==============
1. Specify a MacOS [executor](https://circleci.com/docs/using-macos/) in the job configuration for installing dependencies on MacOS.
1. Use the `macos` key and set the `xcode` version to `14.2`.
1. Set the `resource_class` to `macos.x86.medium.gen2`.
1. Replace `# TODO` with the appropriate commands to install dependencies.
1. Add the `install-dependencies-osx` job to your workflow.
1. Push the configuration to see the job live in the CircleCI UI.
1. Compare the `install-dependencies-osx` job with the Docker-based jobs.

Task 3: Using Secondary Containers with Docker Executor
==============

Returning to our docker based jobs, let's explore how to use [secondary containers](https://circleci.com/docs/using-docker/#using-multiple-docker-images) with the [Docker executor](https://circleci.com/docs/using-docker/) in CircleCI. Secondary containers are additional services that are required for your job to run. For example, you might need a database like PostgreSQL or a cache like Redis.

Even though our project doesn't require a secondary container, for the sake of this lab, let's assume we need a PostgreSQL database for running some tests.

1. Add a second `image:` key to the list of docker images defined under the `docker` key for your `test` job.
1. Add the `cimg/postgres:15.1` image as a [secondary container](https://circleci.com/docs/using-docker/#using-multiple-docker-images).
1. Observe your changes - can you see how multiple containers are now spun up as part of your `test` job?


```
test:
  docker:
    - image: cimg/go:1.21 # your steps will always run in the first container
    - image: cimg/postgres:15 # subsequent images will be spun up as secondary containers
```

> **Note:** All the containers within a single job will share the same resources. Explore the usage of [secondary containers](https://circleci.com/docs/using-docker/#using-multiple-docker-images) with the [Docker executor](https://circleci.com/docs/using-docker/) in CircleCI. Secondary containers provide additional services required for a job, such as databases or caches.

