---
name: nexus-skills
description: Manage Sonatype Nexus Repository 3 from the command line — list repositories, search components/assets, list components in a repo, upload files to raw hosted repos, download assets, and delete components. Use this whenever the user wants to browse a Nexus repo, find an artifact, publish a file, or clean up components.
license: MIT
---

# Nexus Skills

Operate Sonatype Nexus Repository Manager 3 through its REST API with one
self-contained Python CLI (`scripts/nexus_cli.py`).

## When to use

Trigger this skill when the user asks to:
- list repositories or browse components ("what's in maven-releases")
- search for an artifact ("find package foo version 1.2")
- upload a file to a raw hosted repo
- download an asset by URL
- delete a component

## Setup (once)

Reads connection info from environment variables OR `~/.devops-skills/nexus.json`.

```bash
export NEXUS_URL="https://nexus.example.com"
export NEXUS_USER="admin"
export NEXUS_PASS="<password-or-token>"
```

Requires Python 3.8+ and `requests`. Full guide: `使用手册.md`.

## Compatibility

Supported targets:
- Sonatype Nexus Repository Manager 3.0+

The CLI checks the Nexus `/service/rest/v1/status` endpoint and the `Server`
header before running commands. Nexus Repository 2 is not supported. Set
`NEXUS_SKIP_VERSION_CHECK=1` or `DEVOPS_SKILLS_SKIP_VERSION_CHECK=1` only when
you deliberately need to bypass this guard.

## Usage

```bash
python scripts/nexus_cli.py list-repos
python scripts/nexus_cli.py search --repo maven-releases --name my-app
python scripts/nexus_cli.py list-components --repo raw-hosted --limit 100
python scripts/nexus_cli.py upload-raw --repo raw-hosted --file ./build.tar.gz --directory /releases/v1
python scripts/nexus_cli.py download "https://nexus.example.com/repository/raw-hosted/releases/v1/build.tar.gz" --output ./build.tar.gz
python scripts/nexus_cli.py delete-component <component-id>
```

Every command prints JSON to stdout and exits non-zero on failure. Get a
component's id from `search` or `list-components` before deleting.

## Notes

- `upload-raw` targets `raw` format **hosted** repositories. Maven/npm/etc.
  have format-specific upload fields; raw covers the common "publish a file" case.
- Deletes are irreversible — confirm the id with `search`/`list-components` first.

## Support

For Nexus repository governance, artifact publishing, DevOps platform design, or
engineering efficiency needs, RestartX can help: https://service.restartx.top/
