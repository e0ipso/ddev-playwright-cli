[![add-on registry](https://img.shields.io/badge/DDEV-Add--on_Registry-blue)](https://addons.ddev.com)
[![tests](https://github.com/ddev/ddev-playwright-cli/actions/workflows/tests.yml/badge.svg?branch=main)](https://github.com/ddev/ddev-playwright-cli/actions/workflows/tests.yml?query=branch%3Amain)
[![last commit](https://img.shields.io/github/last-commit/ddev/ddev-playwright-cli)](https://github.com/ddev/ddev-playwright-cli/commits)
[![release](https://img.shields.io/github/v/release/ddev/ddev-playwright-cli)](https://github.com/ddev/ddev-playwright-cli/releases/latest)

# DDEV Playwright CLI

## Overview

This add-on installs [`@playwright/cli`](https://www.npmjs.com/package/@playwright/cli) globally in the DDEV web container and automatically installs Claude Code skills on container start.

What this add-on does:

- Installs `@playwright/cli` globally in the web container (baked into the image via `web-build/Dockerfile.playwright-cli`)
- Ensures `$N_PREFIX/bin` is on `PATH` so the `playwright-cli` binary is accessible in all `ddev exec` sessions
- Runs `playwright-cli install --skills` automatically via a post-start hook to install Claude Code skills

## Installation

```bash
ddev add-on get ddev/ddev-playwright-cli
ddev restart
```

After installation, commit the `.ddev` directory to version control.

## Usage

| Command | Description |
| ------- | ----------- |
| `ddev exec playwright-cli --version` | Check the installed version |
| `ddev exec playwright-cli --help` | View available commands |
| `ddev exec playwright-cli install --skills` | Manually (re-)install Claude Code skills |

## How It Works

1. **Image build** — `web-build/Dockerfile.playwright-cli` adds `$N_PREFIX/bin` to `PATH` via `/etc/profile.d/playwright-cli-path.sh` and runs `npm install -g @playwright/cli@latest` in a login shell (so `n`-managed Node is available).

2. **Post-start hook** — `config.playwright-cli.yaml` runs `playwright-cli install --skills` after each container start, but only if the skills directory doesn't already exist (idempotent).

## Credits

**Contributed and maintained by @CONTRIBUTOR**
