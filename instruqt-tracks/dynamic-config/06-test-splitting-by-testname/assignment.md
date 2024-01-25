---
slug: test-splitting-by-testname
id: 9mwozab0wjah
type: challenge
title: Advanced Test Splitting by Test Names
teaser: Dive deep into CircleCI's test splitting feature and learn how to optimize
  your test suite by splitting tests based on test names.
notes:
- type: text
  contents: |
    In this challenge, you will further optimize your CircleCI pipeline by implementing an advanced level of test splitting - splitting by test names. This strategy can be more efficient than splitting by file names, especially when test files have uneven numbers of tests or tests that take varying amounts of time to run.
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

Your next task is to further optimize your CircleCI pipeline by implementing test splitting based on test names. This strategy can be more efficient than splitting by file names, especially when test files have uneven numbers of tests or tests that take varying amounts of time to run. However, this approach requires your test runner to be able to accept a list of specific test names to run, which may not be possible with all test runners.

Task 1: Create a new job
==============
1. Create a new job named `test-split-by-testname`.
1. Use the same Docker image as the `test` job (`cimg/go:1.21`).
1. Configure the job to run in [parallel](https://circleci.com/docs/parallelism-faster-jobs/#specify-a-jobs-parallelism-level) across two containers.

Task 2: Attach workspace
==============
1. Add a step to attach the [workspace](https://circleci.com/docs/workspaces/) at `~/` in the `test-split-by-testname` job configuration.

Task 3: Extract Test Names
==============
1. Add a `run:` step to the `test-split-by-testname` job, where you will extract a list of a test names from your Go files.
1. Try using the `get-test-names.sh` script in the shell tab via `./get-test-names.sh`. You'll see that this script returns all the names of individual tests in the project.

> **Note:** This script uses `go test -list` to retrieve test names. This may be equally as easy or more challenging in your own project depending on the capabilities of your testing toolchain.

> **Note:** The `get-test-names.sh` script conveniently removes trailing metadata from the `go test -list` output to provide raw test names only.

Task 4: Split Tests
==============
1. Pipe the output of `./get-test-names.sh` into the `circleci tests split` command in the new step of the `test-split-by-testname` job created in Task 3.
1. Store the result of the split operation in a variable named `TESTS_TO_RUN`.

The command should look like:
  ```
  TESTS_TO_RUN=$(./get-test-names.sh | circleci tests split)
  ```

Task 5: Split By Timings
==============
We'll now modify the `circleci tests split` command to split the tests based on timing data from our XML output.

1. Update the usage of `circleci tests split` to include the `--split-by timings` flag to split the tests based on timing data instead of alphabetically.
1. Also add the `--timings-type testname` flag to match the provided strings to test names in the timing data.

> **Note:** Even test runners that export results in the JUnit XML format do not always include the timing data by default. Take a look at a `results.xml` artifact from one of your previous builds. Can you spot the timing data?

> **Note:** The first time you use these flags you will likely see the below error message:
>
> `Error reading historical timing data: file does not exist
> Requested weighting by historical based timing, but they are not present. Falling back to weighting by name.`
>
> This should disappear on subsequent runs.

Task 6: Run the Tests
==============

1. Add a variable called `LIST_OF_TESTS` to your run step, declared like so: `LIST_OF_TESTS=$(echo $TESTS_TO_RUN | tr ' ' '|')`. This simply provides us with the list of test names delimited by the `|` character, which will be used to construct a regular expression which will be provided to the test runner.
1. Add `gotestsum` to your step in a similar fashion to your previous test jobs.
1. Modify your `gotestsum` command to specify only a certain subset of tests by using the `-run` flag, providing it with the string `"^($LIST_OF_TESTS)$"`.

The final command should look like:
```
LIST_OF_TESTS=$(echo $TESTS_TO_RUN | tr ' ' '|')
gotestsum --junitfile report.xml -run "^($LIST_OF_TESTS)$"
```

> **Note:** The fact that we match tests via a regular expression provided to the test runner is a nuance of the tooling used in this particular project. Please refer to your own tooling to understand how to invoke only a certain set of test names.

Task 7: Store test results & add to the workflow
==============
1. Add steps to store the [test results](https://circleci.com/docs/configuration-reference/#storetestresults) and the `report.xml` file as an artifact in the `test-split-by-testname` job configuration.
1. Include the `test-split-by-testname` job in the `build-test-deploy` workflow.
1. Set the `test-split-by-testname` job to require the completion of the `install-dependencies` job before running.

Task 8: Observe results
==============
1. Push the latest configuration to your repository.
1. Rerun the build multiple times to observe the improved runtime of the `test-split-by-testname` job compared to previous jobs.
1. Note that splitting by timings can also be used when splitting by filenames, as long as the JUnit XML results contain the `file` attribute and timing data.
