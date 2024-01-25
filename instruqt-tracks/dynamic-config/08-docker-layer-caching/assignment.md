---
slug: docker-layer-caching
id: cyzfzcj1eyqe
type: challenge
title: Docker Layer Caching
teaser: Speed up and optimise your docker builds by making use of Docker Layer Caching
notes:
- type: text
  contents: In this challenge, you'll be tasked with building a docker image, and
    utilising remote docker and docker layer caching to speed up the process
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

Docker images are a fundamental part of containerized applications. Building these images efficiently is crucial for a quick and smooth CI/CD process. Docker Layer Caching can significantly speed up this process by caching layers of your Docker images, preventing the need for them to be rebuilt in subsequent runs.

In this lab, you will learn how to build a Docker image from the Dockerfile included in the repository. You will create a new job called `docker-build` using a Docker executor and utilize Docker Layer Caching (DLC) to speed up the process. This job will be included in your `build-test-deploy` workflow.

Task 1: Create the docker-build job
==============
1. Create a new job named `docker-build` in your configuration file.
1. Use the default go executor for this job, similar to your previous jobs.

Task 2: Set up remote Docker
==============
> **Note:**Due to security risks, Docker-in-Docker builds are not allowed. To work around this, we spin up a build environment using the `setup_remote_docker` step. This in-built CircleCI step starts a remote environment that can be used to build Docker images.
> **Note:** If you require root access to a machine or direct access to the docker daemon, you may prefer to use our "machine" executor, which provides the ability to run a job in a Linux VM, complete with root access and with docker pre-installed.
1. Add the [`setup_remote_docker`](https://circleci.com/docs/docker-layer-caching/#remote-docker-environment) step to your `docker-build` job to spin up a remote environment for building Docker images.
1. Do not specify the `docker_layer_caching` parameter in this step just yet.

Task 3: Include the job in the workflow
==============
1. Include the `docker-build` job in your `build-test-deploy` workflow to ensure it runs along with the other jobs.
   - Since this doesn't rely on anything else, it doesn't need a `requires` tag.

Task 4: Build the Docker image
==============
> **Note:** Once you've included the `setup_remote_docker` step, you can build docker images just like anywhere else
1. Add the `docker build .` commandto your `docker-build` job to build the image.
1. Push your config and have a look at the run in CircleCI

Task 5: Implement Docker Layer Caching
==============
1. Enable [Docker Layer Caching](https://circleci.com/docs/docker-layer-caching/) (DLC) on your Docker build to speed up the process.
1. Pass the `docker_layer_caching` parameter into the `setup_remote_docker` step before running the `docker build` command.
1. Push this again and see if you can notice a difference in the job

Note: For more detailed information and examples on implementing Docker Layer Caching, refer to the provided documentation.
