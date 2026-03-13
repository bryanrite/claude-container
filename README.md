# claude-container

A generic Docker container for running Claude Code in isolated, per-project environments. Supports mounting additional directories for shared libraries or sibling repos.

## Install

Clone the repo and symlink the binary into your PATH:

```bash
git clone https://github.com/bryanrite/claude-container
cd claude-container
sudo ln -sf "$PWD/claude-container" /usr/local/bin/claude-container
```

## Usage

From any project directory:

```bash
claude-container
```

This will:

1. Build the `claude-container` Docker image if it doesn't already exist.
2. Create a `.claude/` directory in the current project for config and credentials
3. Mount the current directory as `/workspace` and start Claude Code

All arguments are passed through to Claude:

```bash
claude-container --resume
```

## Volume Conventions

| Mount | Container Path |
|---|---|
| Current directory | `/workspace` |
| `.claude/config/` | `/root/.claude` |
| `.claude/claude.json` | `/root/.claude.json` |

## Additional Volumes

To mount additional directories (e.g. shared libraries or sibling repos), create a `.claude/volumes` file:

```
# one mount per line — source:destination
# relative paths are resolved from the project root
../../some_lib:/refs/some_lib
../shared_config:/refs/shared_config
```

Lines starting with `#` and blank lines are ignored.

## Gitignore/Dockerignore

Add `.claude/` to your project's `.gitignore` and `.dockerignore` — it contains local config and credentials that shouldn't be committed:

```bash
echo '.claude/' >> .gitignore
echo '.claude/' >> .dockerignore
```

## Upgrading Claude

To upgrade claude, rebuild the container, it will install the latest claude code.

```bash
docker rmi claude-container
```

The image will be rebuilt automatically on next run. All settings, conversation history, and volume mounts are stored in the project's `.claude/` directory and are preserved across rebuilds.
