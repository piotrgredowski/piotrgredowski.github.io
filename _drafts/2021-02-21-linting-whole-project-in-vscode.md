---
title: Linting whole project in VSCode
date: 2021-02-21 17:26:08 +0100
categories: [Blogging]
tags: [vscode, vscode extensions, vscode tips, software development, programming]
---

## TLDR

```json
// tasks.json
{
  // See https://go.microsoft.com/fwlink/?LinkId=733558
  // for the documentation about the tasks.json format
  "version": "2.0.0",
  "tasks": [
    {
      "label": "flake8",
      "type": "shell",
      "command": "flake8 .",
      "presentation": {
        "echo": true,
        "reveal": "never",
        "focus": false,
        "panel": "shared",
        "showReuseMessage": false,
        "clear": true
      },
      "problemMatcher": {
        "owner": "flake8",
        "source": "flake8",
        "base": "flake8",
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

```json
  "triggerTaskOnSave.tasks": {
    "flake8": ["**/*.*py"]
  }
```

Sources:

https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.triggertaskonsave
https://allisonthackston.com/articles/vscode-tasks-problemmatcher.html
