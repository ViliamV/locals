# Locals
Per-directory Bash aliases made easy :balloon:

## What it does
*Locals* hooks into builtin `cd` command and looks for aliases file (default `.local-aliases`).
When the file is present, it sources it (loads aliases).
When you `cd` into another directory, it uses `$OLDPWD` variable to check if previous directory had local aliases.
If yes, it unaliases them.

You can extend this functionality using [custom hook](#Customization).

## Requirements
- Bash >= 4.4.0

## Installation
1. Clone git repo
  ```sh
    git clone https://github.com/ViliamV/locals ~/.locals
  ```
2. Add this to your `.bashrc`
  ```sh
    source ~/.locals/locals
  ```
3. Enjoy :tada:

## Usage
1. To create local aliases you can either
  - use command `locals` which opens local aliases file in your `$EDITOR`, or
  - open local aliases file (default `.local-aliases`) in your editor of choice.

2. Inside the file use standard alias syntax e.g.
  ```sh
    alias up='docker-compose up'
  ```

3. If you did not use command `locals` call `locals-refresh` to refresh local aliases.

4. Now you can use alias `up` in this directory. When you `cd` into another directory, it is unaliased automatically.

## Customization
Locals reads these `ENV` variables.

| Variable         | Description                                                   | Default          |
| ---------------- | ------------------------------------------------------------- | ---------------- |
| `LOCALS_NAME`    | filename to look for                                          | `.local-aliases` |
| `LOCALS_VERBOSE` | print local aliases when entering the directory               | unset            |
| `LOCALS_DEBUG`   | print information about unaliasing when leaving the directory | unset            |
| `LOCALS_HOOK`    | command that is called after `cd`                             | unset            |

## Goals
- be easy to use and intuitive
- have no dependencies other than [Coreutils](https://www.gnu.org/software/coreutils/)
- cause no conflicts with Bash builtins
- be compatible with Bash >=4.4.0

## Non-Goals
- traverse directory tree and look for local aliases in parent directories
- be replacement for [GNU Make](https://www.gnu.org/software/make/)
- be an automation tool
