---
title: Setup
---

FIXME: Setup instructions live in this document. Please specify the tools and
the data sets the Learner needs to have installed.
<!--
## Data Sets

FIXME: place any data you want learners to use in `episodes/data` and then use
       a relative link ( [data zip file](data/lesson-data.zip) ) to provide a
       link to it, replacing the example.com link.
Download the [data zip file](https://example.com/FIXME) and unzip it to your Desktop
-->
## Software Setup

We use VS Code for general coding. Make sure to install VS Code for your operating system following the instructions below:

- [Windows](https://code.visualstudio.com/docs/setup/windows)
- [macOS](https://code.visualstudio.com/docs/setup/mac)
- [Linux](https://code.visualstudio.com/docs/setup/linux)


::::::::::::::::: discussion

### Windows users

If you are a Windows user, you may also need WSL, the Windows Subsystem for Linux. WSL will allow you to use the Terminal from within VS Code and to execute UNIX style commands.

Follow the [official instructions here.](https://learn.microsoft.com/en-us/windows/wsl/install)

After installing WSL, you will also need to install [GitBash](https://gitforwindows.org/).

After installing GitBash, change the default Terminal executable on VS Code:

1. Open VS Code.
2. From the `View` menu select `Command Palette --> Terminal: Select Default Profile`.
3. Select `GitBash` as the default profile.

✅ You're all set to use VS Code with GitBash on Windows. 

::::::::::::::::::::::::::::


## Environment management with conda

For certain workshops, you may need to install [conda](https://docs.conda.io/en/latest/), an open-source software and package management system. This is usually necessary when working with Python code.

Our preferred flavour of conda is [miniforge](https://github.com/conda-forge/miniforge), but you could also use [miniconda](https://www.anaconda.com/docs/getting-started/miniconda/main).

[Click here and follow the miniforge installation instructions for your OS.](https://conda-forge.org/download/)


<!--
READ HERE FOR OS-SPECIFIC INSTRUCTIONS

Setup for different systems can be presented in dropdown menus via a `spoiler`
tag. They will join to this discussion block, so you can give a general overview
of the software used in this lesson here and fill out the individual operating
systems (and potentially add more, e.g. online setup) in the solutions blocks.
-->

<!--
:::::::::::::::: spoiler

### Windows

Use PuTTY

::::::::::::::::::::::::

:::::::::::::::: spoiler

### MacOS

Use Terminal.app

::::::::::::::::::::::::


:::::::::::::::: spoiler

### Linux

Use Terminal

::::::::::::::::::::::::
-->

