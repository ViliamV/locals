[![forthebadge](https://forthebadge.com/images/badges/built-by-developers.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/check-it-out.svg)](https://forthebadge.com)

*Locals* is a lightweight manager for per-directory bash aliases and environment variables.

## Table of Contents
- [What it is](#what-it-is)
- [Requirements](#requirements)
- [Installation](#installation)
  - [One-line](#one-line)
  - [Manual](#manual)
- [Usage](#usage)
- [Configuration](#configuration)
- [List of commands](#list-of-commands)
- [Goals](#goals)
- [Non-Goals](#non-goals)
- [Disclaimer](#disclaimer)
- [Similar projects](#similar-projects)

## What it is
*Locals* hooks into builtin `cd` command and looks for `.locals` in the current directory.
When the file is present it is executed (loads aliases, env variables, etc).
When you `cd` into another directory, it uses `$OLDPWD` variable to check if the previous directory had `.locals`.
If yes, *Locals* will `unalias` aliases and `unset` exported variables automatically.
Any additional "cleanup" is done by executing file `.locals.leave` (if present in the previous directory).

You can add use [custom hook](#Configuration) to add more functionality.

## Requirements
- Bash ≥ 4.0.0
- Coreutils

(You probably have both :relieved:)

## Installation
### One-line
  ```sh
  curl https://raw.githubusercontent.com/ViliamV/locals/master/locals --output ~/locals && \
  echo "[[ -f ~/locals ]] && source ~/locals" >> ~/.bashrc
  ```
### Manual
- Download file using `curl`
  ```sh
    curl https://raw.githubusercontent.com/ViliamV/locals/master/locals --output ~/locals
  ```
- Or right-click on [this](https://raw.githubusercontent.com/ViliamV/locals/master/locals) and **Save Link as...** `locals` in your home directory.

- Add this to your `.bashrc`
  ```sh
    [[ -f ~/locals ]] && source ~/locals
  ```

Enjoy :tada:

## Usage
1. To create local aliases you can either
  - use command `locals edit` which opens local aliases file in your `$EDITOR`, or
  - open local aliases file (default `.locals`) in your editor of choice.

2. File is executed when you enter the directory so enter any shell commands.
  ```sh
    #!/bin/bash
    alias up='docker-compose up'
    export FOO=bar
    echo "I have local $FOO"
  ```

3. If you did not use command `locals edit` call `locals refresh` to refresh local aliases.

4. Now you can use alias `up` or variable `$FOO` in this directory.
When you `cd` into another directory, `up` is unaliased automatically and `$FOO` is unset.

## Configuration
Locals reads the following variables:

| Variable                | Description                                                   | Default         |
| ----------------------- | ------------------------------------------------------------- | --------------- |
| `LOCALS_FILENAME`       | locals file to source                                         | `.locals`       |
| `LOCALS_LEAVE_FILENAME` | file to source when leaving                                   | `.locals.leave` |
| `LOCALS_VERBOSE`        | print local aliases when entering the directory               | unset           |
| `LOCALS_DEBUG`          | print information about unaliasing when leaving the directory | unset           |
| `LOCALS_HOOK`           | command that is called after `cd`                             | unset           |

## List of commands
- `locals` print local aliases in current directory
- `locals print` print local aliases in current directory
- `locals edit` - open `$LOCALS_FILENAME` file in current directory using `$EDITOR`
- `locals edit-leave` - open `$LOCALS_LEAVE_FILENAME` file in current directory using `$EDITOR`
- `locals refresh` refresh local aliases in current directory

## Goals
- be easy to use and intuitive
- have no dependencies other than [Coreutils](https://www.gnu.org/software/coreutils/)
- cause no conflicts with Bash builtins
- be compatible with Bash ≥ 4.0.0

## Non-Goals
- traverse directory tree and look for .locals files in parent directories
- be replacement for [GNU Make](https://www.gnu.org/software/make/)
- do magic
- exceed 100 LOC

## Disclaimer
*Locals* **always** overwrites aliases and variables.
Also, when leaving it has no mechanism to replace them with original values.

*Locals* can be disabled by `unset cd`.

## Similar projects
- [Autoenv](https://github.com/inishchith/autoenv)
- [direnv](https://direnv.net/)
- [desk](https://github.com/jamesob/desk)
