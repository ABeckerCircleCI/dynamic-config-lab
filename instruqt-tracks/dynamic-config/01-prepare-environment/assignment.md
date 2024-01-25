---
slug: prepare-environment
id: fnsxokhjqba9
type: challenge
title: Preparing Your Environment
teaser: We'll begin with setting up the environment you'll need to complete the hands-on
  exercises in this course.
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

In this challenge, you will be setting up your environment to work with both GitHub and CircleCI in order for us to begin building our first pipeline.

Task 1: Authenticate pre-installed GitHub CLI
==============

1. Open the lab instance terminal by clicking on “>_ Shell” in the upper left-hand corner of the screen and run `gh auth login`.

1. Follow the prompts: Select `GitHub.com` as the account and `HTTPS` as the protocol.

1. If you are asked if you wish to authenticate with your GitHub credentials, type Y and hit Enter.

1. When asked, select the option to 'Paste an authentication token' - This will be a Personal Access Token (PAT). You can generate a PAT at https://github.com/settings/tokens (choose `Tokens (classic)`) and then click `Generate new token (classic)`. Ensure it has `Repo`, `workflow` and `read:org permissions`.

1. Paste your `PAT` into the terminal and hit `Enter`. Reminder: CMD+V or CTRL+V will most likely not work in this terminal instance. Right-click (or CTRL-click on a Mac) to paste into the terminal. Successful validation will display a confirmation message.

Task 2: Create CircleCI personal API token and set up pre-installed CircleCI CLI
==============

1. In the terminal, type circleci setup. When prompted, input your CircleCI PAT. You can generate this by navigating to [`app.circleci.com`](https://app.circleci.com/) And clicking `> User Settings (the very bottom selection on the lefthand frame)  > Personal API Tokens > Create New Token`. If you're having trouble locating the Personal API Tokens page under User Settings, try this link: [`app.circleci.com/settings/user/tokens`](https://app.circleci.com/settings/user/tokens)

1. If asked if you would like to enable telemetry, you may select Yes or No, either is fine.

1. Paste your CircleCI token into the terminal and hit `Enter`. A confirmation message will appear if successful. Keep your CircleCI Personal API Token in your clipboard, or save it to a document for future use - you'll need to use it again soon.

1. You will see a prompt specifying CircleCI Host https://circleci.com – hit Enter to confirm this. You should see `"Setup complete."`

1. Click on the `Code Editor` tab and click the CircleCI icon on the sidebar

1. Select the `Log In` button if you do not see an entry box for your personal access token

1. Paste your CircleCI Personal API Token into the box and click `Connect to CircleCI` **Note: While the Terminal tab tends to not accept CMD+V or CTRL+V, this window tends to not accept Right-click or Command-click input, but will accept CMD+V or CTRL+V for paste.**

Task 3: Set up the CircleCI project
==============

1. Move back to the terminal by clicking on the `">_ Shell" tab`. Verify the existence of the .circleci directory by typing `ls -la`.

1. You should be in the `project` directory. If so, you should see a directory named `.circleci` within your current working directory. If you do not see this, but do see a directory named `project`, input `cd project` and try `ls -la` again now that you are in the project directory.

1. Create a private GitHub repository with the current directory's content using `gh repo create --private --source=. --push`

1. On https://app.circleci.com/ go to Projects, find your newly created project, and click `Set up Project`.

1. In the setup options, select the `Fastest` option, choose your project and branch (`main`), and click `Set up Project` again to trigger your first build. Note, this initial build will fail, as we haven't built out our config yet.
