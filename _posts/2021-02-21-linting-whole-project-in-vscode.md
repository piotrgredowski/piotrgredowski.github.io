---
title: Linting whole project in VSCode with flake8
date: 2021-02-21 19:56:59 +0100
categories: [Blogging]
tags: [vscode, vscode extensions, vscode tips, flake8, python, software development, programming]
comments_id: 1
---

## Intro

When your pipeline fails because your linter reported some problems in your code -
you have to go to file on your computer, find problem and fix it.

Or you can run your linter locally, click on file name - go to faulty line and fix it.

> You won't have such problem if you have configured precommit hook which runs all
> your scripts for code analysis before commit is created.
> But that's a topic for another blog post...

Another example of situation in which linting whole project in VSCode will be
helpful is when you want to add code linting to the project.
Then you will have XXX issues to solve. You'll have to look into terminal and go
line by line through problems... It's boring.

In such cases - it's very good to have all problems listed in `PROBLEMS` tab in VSCode.
Then you can just iterate through them with `Go to Next Problem...` (button `ALT + F8`).
Cursor will be positioned at line and column of where problem exists. And you can
easily fix it. Then press `ALT + F8` and fix next problem...

## Linters in VSCode

VSCode has support for all linters I use (`tslint`, `pylint`, `flake8`).
But it runs them only against files which are currently opened.

**This post will focus on running `flake8` on whole project.**

## Task for linting whole project with flake8

```jsonc
// tasks.json
{
  // See https://go.microsoft.com/fwlink/?LinkId=733558
  // for the documentation about the tasks.json format
  "version": "2.0.0",
  "tasks": [
    {
      "label": "flake8-whole-project",
      "type": "shell",
      "command": "flake8 .",
      "presentation": {
        "echo": true,
        "reveal": "never",
        "focus": false,
        "panel": "shared",
        "showReuseMessage": false,
        "clear": true,
        "revealProblems": "never"
      },
      "problemMatcher": {
        "owner": "flake8",
        "source": "flake8-whole-project",
        "fileLocation": ["relative", "${workspaceFolder}"],
        "pattern": {
          "regexp": "^(.+):(\\d+):(\\d+): ((\\w)\\d+) (.+)$",
          "file": 1,
          "line": 2,
          "column": 3,
          "code": 4,
          "severity": 5,
          "message": 6
        }
      }
    }
  ]
}

```

### What does the definition of task mean?

```jsonc
// tasks.json
{
  // See https://go.microsoft.com/fwlink/?LinkId=733558
  // for the documentation about the tasks.json format
  "version": "2.0.0",
  "tasks": [
    {
      // That the name of task
      "label": "flake8-whole-project",
      // We are going to execute shell command in this task
      "type": "shell",
      // Actual command to execute
      "command": "flake8 .",
      // How command output will be presented - in overall, we want it to be silent
      "presentation": {
        "echo": true,
        "reveal": "never",
        "focus": false,
        "panel": "shared",
        "showReuseMessage": false,
        "clear": true,
        "revealProblems": "never"
      },
      // Definition of problem matcher - which will translate problems reported in
      // command line by flake8 to entries in PROBLEMS tab
      "problemMatcher": {
        // Name of owner which will handle found problems
        "owner": "flake8",
        // Name of problem matcher which will show next to problem in PROBLEMS tab
        "source": "flake8-whole-project",
        // How paths to files with found problems, should be interpreted by problem matcher
        "fileLocation": ["relative", "${workspaceFolder}"],
        "pattern": {
          // Regular expression used to match found problems
          // For details, see https://regex101.com/r/JXYDAE/1
          "regexp": "^(.+):(\\d+):(\\d+): ((\\w)\\d+) (.+)$",
          "file": 1,
          "line": 2,
          "column": 3,
          "code": 4,
          "severity": 5,
          "message": 6
        }
      }
    }
  ]
}
```

### Run task on file save

1. To run task whenever `.py` file is saved, we need to install
[_Trigger Task on Save_](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.triggertaskonsave)
VSCode extension.
2. Then go to settings (`CTRL + ,`), choose `Workspace` as scope of settings (to use task only in current project).
3. Find `triggerTaskOnSave.tasks` and click on `Edit in settings.json`.
4. Fill it like below:

  ```json
    "triggerTaskOnSave.tasks": {
      "flake8-whole-project": ["**/*.*py"]
    }
  ```

And from this moment, whenever you will save your `.py` file - task `flake8-whole-project`  will be executed.

### One disadvantage: duplicates with additional problem matcher

For now, if you run task and then open file in which one of problems was found -
there are two references to problem in PROBLEMS tab. One is from above task, one from VSCode's
`flake8` execution.

![Duplicated in VSCode's PROBLEMS tab](/assets/img/linting-whole-project-in-vscode/linter-task-duplicates.png)

~~And I don't know how to get rid of duplicates.~~

EDIT: To get rid of this problem - you can just disable `flake8` in VSCode in your workspace.
If you have doubled problems in `Problems tab` for `flake8` - just disable it like below:

![Disable flake8 in VSCode](/assets/img/linting-whole-project-in-vscode/disable-flake8.png)

You can do the same thing for other linters in VSCode.

## Summary

Now you can iterate through all problems found by `flake8` in whole project with
`ALT + F8` and fix them!

### Additional tasks

* `mypy`

  ```json
      {
        "label": "mypy-whole-project",
        "type": "shell",
        "command": "source .venv/bin/activate; mypy . --show-column-numbers --show-error-codes --ignore-missing-imports",
        "presentation": {
          "echo": true,
          "reveal": "never",
          "focus": false,
          "panel": "shared",
          "showReuseMessage": false,
          "clear": true,
          "revealProblems": "never"
        },
        "problemMatcher": {
          "owner": "mypy",
          "source": "mypy-whole-project",
          "fileLocation": ["relative", "${workspaceFolder}"],
          "pattern": {
            "regexp": "^([\\w.]+):(\\d+):(\\d+): (\\w+): (.+)  \\[([\\w-]+)\\]$",
            "file": 1,
            "line": 2,
            "column": 3,
            "code": 6,
            "severity": 4,
            "message": 5
          }
        }
      }
  ```

## Links

[https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.triggertaskonsave](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.triggertaskonsave)
[https://allisonthackston.com/articles/vscode-tasks-problemmatcher.html](https://allisonthackston.com/articles/vscode-tasks-problemmatcher.html)

