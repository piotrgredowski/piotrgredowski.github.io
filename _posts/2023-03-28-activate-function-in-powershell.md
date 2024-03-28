---
title: Activating Python virtual environment in PowerShell
date: 2024-03-28 07:26:08 +0100
categories: [Blogging]
tags: [powershell, activate, python, virtualenv, virtual environment, function, shell]
comments_id: 3
---

When I'm working on Windows in the repository where I build executables, I'm usually using two types of virtual environments for my Python development.

One is called `.venvdev` and it's located in the root of my project. It's used for development purposes.
Another is called `.venv`, it's located in the same directory as `.venvdev` and it's used for production purposes. Whenever I need to build for example an executable with `pyinstaller` I'm using this environment. And `pyinstaller` is bundling all of the installed libraries from the environment to the executable.

## Why?

I don't want to activate `.venv` environment when I'm developing - because it doesn't contain linters and formatters.
And I don't want to activate `.venvdev` when I'm building an executable - because I don't want to bundle all of the development libraries to the executable.

## What?

I've created a function in my PowerShell profile which is activating the environment. It's super simple. But I can also pass the path to the virtual environment as an argument.

So by default - it's activating `.venvdev` environment. But when I'm passing `.venv` as an argument - it's activating `.venv` environment.

```powershell
activate # will activate .venvdev if it exists, otherwise .venv
activate ../.my_venv # will activate ../.my_venv if it exists, otherwise .venv
```

## How?

```powershell
function activate {
    param (
        [string]$venv_path = ".venvdev"
    )

    if (-not (Test-Path $venv_path)) {
        $venv_path = ".venv"
    }
    $activate_path = Join-Path $venv_path "Scripts\activate"
    if (-not (Test-Path $activate_path)) {
        Write-Host "No venv found"
        return
    }
    & $activate_path
}
```
