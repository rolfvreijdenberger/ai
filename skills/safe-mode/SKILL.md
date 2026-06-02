---
name: safe-mode
description: toggle safe mode to prevent destructive commands and provide basic production safety guardrails. Use `safe mode on` to activate, or `safe mode off` to deactivate.
license: WTFPL
metadata:
  author: Rolf Vreijdenberger
---

# usage
Invoked via `safe mode`, `safe-mode` or similar patterns.
or via `/safe-mode` as a command.
Invoke this skill with an optional parameter:

```text
safe mode on
safe mode off
```

Arguments after `safe mode` are provided as user input to this skill.

- `on` or no parameter: activate safe mode and follow the rules below.
- `off`: deactivate safe mode; the restrictions below no longer apply, while normal user instructions, tool policies, and platform safety requirements still apply.
- Any other parameter: keep or activate safe mode and ask the user to clarify whether they meant `on` or `off`.

## scope and duration

When enabled, safe mode remains active for this conversation/session until `safe mode off` is invoked.

If uncertain whether safe mode is currently on or off, behave as if safe mode is on.

## safe mode: prevent destructive and data loss commands and protect the system 

### default action

If uncertain whether a command or action can change, destroy, overwrite, remove, expose, or make data unavailable, do not run it. Ask the user if possible; if asking is not possible, refuse or choose a read-only alternative.

### DO
- only execute commands that do not destroy data and that do not remove files
- ask for user confirmation when in interactive mode when in doubt if a command is safe to execute
- perform safe commands only
- make sure that security properties of confidentiality, integrity and availability of the system, processes, data and files are always protected
- prefer read-only inspection commands, dry-runs, status checks, and commands that only create clearly requested new files
- before editing or overwriting an existing file, create a backup or ask for explicit confirmation

### DO NOT / NEVER
- do not execute commands that remove files
- do not execute commands that remove directories
- do not execute commands that destroy data
- do not execute commands in non-interactive mode when in doubt if a command is safe to execute
- do not execute scripts that are dangerous
- do not change settings, files or metadata in such a way that security risks will arise.
- do not overwrite, truncate, reset, clean, force-update, kill, stop, uninstall, migrate, or otherwise mutate persistent state unless the action is explicitly requested and clearly non-destructive or backed up

### allowed examples while safe mode is on

- Read-only file inspection:
  - `ls`
  - `find`
  - `rg`
  - reading files
- Status checks:
  - `git status`
  - `docker ps`
  - service status commands
- Dry-runs and validation:
  - commands with `--dry-run`
  - syntax checks
  - linters
- Non-destructive tests:
  - tests that do not mutate persistent state
- New file creation:
  - `touch README.md`
  - writing a new file when explicitly requested and not overwriting existing data

### blocked examples while safe mode is on

- Deletion/removal command with and without arguments:
  - `rm`
  - `rmdir`
  - `del`
  - `git clean`
- Overwrite/truncation:
  - shell redirection over existing files
  - `truncate`
  - destructive formatter writes without backup
- Force/reset operations:
  - `git reset --hard`
  - force pushes
  - forced overwrites
- Permissions/ownership:
  - `chmod`
  - `chown`
  - ACL changes
  - credential or permission changes
- Package/system mutation:
  - installs
  - uninstalls
  - upgrades
  - service stops/restarts
  - process kills
- Data stores/infrastructure:
  - database writes/migrations
  - destructive Docker/Kubernetes/cloud commands

## safe mode off: safe-mode restrictions removed

When invoked as:

```text
safe mode off
```

Deactivate safe mode. In this mode, the extra safe-mode restrictions are removed when commands are explicitly requested by the user, subject to normal user instructions, tool policies, and platform safety requirements.
