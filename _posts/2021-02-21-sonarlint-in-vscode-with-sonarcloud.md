---
title: SonarCloud integration with VSCode
date: 2021-02-21 17:26:08 +0100
categories: [Blogging]
tags: [vscode, vscode extension, sonarlint, sonarcloud, software development, programming]
---

## Intro

We are using in our company SonarCloud to perform static analysis on our projects.
Sonar analysis is part of our CI pipeline and it gives great tips on how to improve quality of our code - no doubts.

But my goal was to not have to wait for analysis done and to go to SonarCloud website which will show me results of Sonar analysis.

I just wanted to see all of problems in my code editor, VSCode. Below setup will allow you to see on every opened file results of SonarLint analysis.

## Prerequisites

- VSCode
- Access to subscription of SonarCloud
  - Organization key (look at *How to get organization key from SonarCloud?*)
  - Project(s) key (look at *How to get project key from SonarCloud?*)
  - Token (look at *How to generate token at SonarCloud?*)
- [SonarLint extension for VSCode](https://marketplace.visualstudio.com/items?itemName=SonarSource.sonarlint-vscode)
  - Java Runtime (JRE) 8 or 11

### How to get organization key from SonarCloud?

1. You need to go to [https://sonarcloud.io/projects](https://sonarcloud.io/projects) and login.
2. In top right corner - click on your avatar and look for **My Organizations**. Click on organization of your choice.
3. Now you will see all of your organization's projects. You can get organization key from url which look like below:

   `https://sonarcloud.io/organizations/<organization_key>/projects`

   Or in the top right corner, under your avatar you should see organization key also:

   ![Screenshot with organization key](/assets/img/sonarlint/organization_key.png)

### How to get project key from SonarCloud?

1. Go to [https://sonarcloud.io/projects](https://sonarcloud.io/projects). Find your project - copy project key (bold name visible as headline of each project).

### How to generate token at SonarCloud?

1. Go to [https://sonarcloud.io/account/security/](https://sonarcloud.io/account/security/)
2. Name your new token - let it be e.g. `vscode`.
3. Click generate.
4. **Save it somewhere!**

## Setup

When you have everything installed and have all of keys/tokens - we need to set configuration of VSCode.

1. Open settings of VSCode with `CTRL + ,`.
2. Choose `User` as a scope of where you want to edit settings.

   This is important if you want to share this setting between many projects.

3. Type `sonarlint.connectedMode`.
4. And click on `Edit in settings.json`.

   ![Screenshot with connectedMode setting](/assets/img/sonarlint/setting-connectedmode.png)

5. Configure it like below:

   ```json
    "sonarlint.connectedMode.connections.sonarcloud": [
      {
        "organizationKey": "<organization_key>",
        "token": "<token>"
      }
    ]
   ```

6. Now open one of your projects analysed by SonarCloud in VSCode
7. Go to settings (`CTRL + E`).
8. Choose `Workspace` as a scope of where you want to edit settings.

   Because we want to configure for every project different project ID.

9. Type `sonarlint.connectedMode.project`.
10. And click on `Edit in settings.json`.
11. Configure it like below:

    ```json
        "sonarlint.connectedMode.project": "<project_id>"
    ```

    > If not only you in your team are using VSCode as code editor - it may be useful to keep `.vscode/settings.json` file in your repository. Thanks to this you will share your settings with your colleagues via repository.
    >
    > To keep file in repository (but exclude others from `.vscode` directory), add to `.gitignore` following lines
    >
    > ```none
    > # Exclude VSCode files
    > /.vscode/*
    > ## But include these
    > !/.vscode/launch.json
    > ```

12. You can repeat steps from 6 to 11 for other projects.
13. To confirm that SonarLint is working - open some file with code and open command pallette
    (`CTRL + SHIFT + P`). Then type `SonarLint: Show SonarLint Output` and confirm.

    If the output of SonarLint will look like below - analysis is working correctly (number of found issues can differ :)):

    ```log
    [Info  - 17:27:42.581] Started security hotspot handler on port <port>
    [Info  - 17:27:43.116] Using storage for server '<default>' (last update 2/21/21 5:25 PM)
    [Info  - 17:27:44.147] Analyzing file 'file:///<filename>'...
    [Info  - 17:28:46.374] Found 0 issue(s)
    ```

## How it's working

And from now - if SonarLint will find any problems with your opened file, they will be visible in `PROBLEMS` tab in VSCode.
SonarLint will use exactly the same settings as are used in SonarCloud analysis.

![Lint example](/assets/img/sonarlint/lint-example.png)

You can even look at rule definition by hovering over underlined line, and clicking on `Quick fix`. Then click on `Open description...`.

And you will see how rule is defined in SonarLint.

![Description of rule](/assets/img/sonarlint/full-rule-explanation.png)

## Summary

With above setup - you will be able to see only results for selected file.
In future post I'll try to write how to lint whole project.

Happy coding!
