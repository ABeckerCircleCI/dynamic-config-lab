---
slug: schedules
id: wm0gr594vyhy
type: challenge
title: Setting scheduled runs for your pipelines
teaser: Utilise the scheduled pipelines feature to set up automatic runs for your
  pipelines
notes:
- type: text
  contents: |
    In this challenge, you will set up a schedule for your project that will trigger runs automatically at certain times. This can be very useful for running jobs that need to run regularly, such as nightly builds.
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

Your next task is to set up a [schedule](https://circleci.com/docs/scheduled-pipelines/) to run your pipeline at a certain time every day.
For this, create a schedule that runs your pipeline [daily](https://circleci.com/docs/set-a-nightly-scheduled-pipeline/) at midnight.

The scheduled pipeline interface can be found in the `Triggers` section of your project settings, and will consist of several options:

Trigger Name
----------------
This is the name your schedule will appear under, make sure this is an accurate description of your schedule.

Trigger Description
------------------------
This allows you to provide further details on the schedule, such as any rationale or specific purposes of the schedule.
This is optional and not required for this task.

Timing Options
------------------
These drop-downs and check boxes allow you to select when and how often the pipeline wil run.
For this task, set these settings so that your pipeline triggers once every day at midnight.

Branch or Tag Name
----------------------
This field allows you to select the branch or tag to use when running the pipeline. The run will use the `HEAD` of that branch.
For this task, you should make sure you use the branch you have been pushing to during this lab.

Pipeline Parameters
-----------------------
This field allows you to set parameters that will be passed into any pipelines triggered by this schedule.
This can be extremely useful for changing the behaviour of your runs during scheduled runs when compared to normal runs.
This is an optional setting, and is not required for this task.

Attribution
--------------
This setting allows you to select the user and permissions this job will run with.
If you have a particular permissions requirement, it can be useful to have your schedules run as your user, granting any runs access to whatever your user has access to.
For this task, leaving the setting on the `Scheduled Actor` is sufficient.
