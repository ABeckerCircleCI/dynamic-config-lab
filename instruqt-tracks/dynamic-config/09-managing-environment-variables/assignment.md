---
slug: managing-environment-variables
id: jr5mfqqr8jam
type: challenge
title: Managing environment variables
teaser: Learn how to manage and utilize environment variables.
notes:
- type: text
  contents: |
    You will also explore how to define environment variables at different levels of your configuration, including at the job, executor, and step levels. Furthermore, you will learn how to add a variable via a context and understand how CircleCI masks the contents of a variable from a context to prevent accidental secret leakage.
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

In this lab, you will explore managing environment variables in CircleCI, including adding a variable via a context. You will also learn how to implement branch filtering so that the deploy job only runs on the main branch, and how to include an approval job to require manual approval before it runs.

Environment variables are a key component of CI/CD workflows. They allow you to customize the behavior of your jobs, steps, and commands based on the current environment. In CircleCI, you can define environment variables at different levels of your configuration, including at the job, executor, and step levels.

Additionally, implementing branch filtering and approval jobs can give you more control over when and how your jobs run.

Built-in environment variables
===============

Before we begin writing more config, take a moment to explore the "Preparing environment variables" step within the CircleCI UI. This can be found in the steps of one of your previous jobs. Here, you will find a list of [built-in environment variables](https://circleci.com/docs/variables/#built-in-environment-variables) that CircleCI automatically includes in your runtime. These variables can provide valuable information about your build environment such as the build number, git revision, branch name, and more.

Task 1: Create the deploy job
==============
1. Create a new job named `deploy` in your configuration file.
1. Add this job to your workflow using the `workflows` key.
1. Use the `go` executor for the `deploy` job.

Task 2: Define an environment variable
==============
1. Use the [`environment` key](https://circleci.com/docs/set-environment-variable/#set-an-environment-variable-in-a-job) at the job level to define an environment variable called `MY_FIRST_VARIABLE`.
  - You could configure this variable to include any text you want, but for the purposes of this lab, let's use: 'MY_FIRST_VARIABLE: hello world!'
1. Create a [`run`](https://circleci.com/docs/configuration-reference/#run) step within the `deploy` job to echo this environment variable to stdout.
1. Set the `name` key to `Print my first variable`.
1. Use the `command` key to run the `echo $MY_FIRST_VARIABLE` command.

Task 3: Define an environment variable via a context
==============
1. Create a [context](https://circleci.com/docs/contexts/#create-and-use-a-context) in the CircleCI web application called `my-lab-context`.
1. Add an environment variable called `HELLO_LAB` to the `my-lab-context` context.
1. Add a run: step to the `deploy` job to print the `HELLO_LAB` variable.
1. Set the `name` key to `Print $HELLO_LAB`.
1. Use the `command` key to run the `echo $HELLO_LAB` command.
1. Push your config and inspect the build in CircleCI.
  - Have a look at the `Print $HELLO_LAB` step.
  - You'll see that the variable is [masked](https://circleci.com/docs/contexts/#secrets-masking) by CircleCI to prevent leaking.

Task 4: Specify the context in the workflow
==============
Now, we'll pecify the `my-lab-context` [context](https://circleci.com/docs/contexts/#create-and-use-a-context) within the `deploy` job in your workflow.
1. Use the `context` key within the `deploy` job configuration, setting it to `my-lab-context`.

Your config should look something like:
```
workflows:
  build-test-deploy:
    jobs:
      # ... your jobs
      deploy:
        # requires etc
        context:
          - my-lab-context
```

Wrapping up
================
In this challenge, you'll have explored how to manage environment variables at different levels of configuration.

Although we've looked at environment variables within the context of deployment, deployment itself is a complex topic, often involving concerns such as behind the firewall deployment, configuration policies and OIDC. These topics are beyond the scope of today's lab, but be sure to raise any concerns or interests with your Customer Engineer to provoke further discussion on the matter.
