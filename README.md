# discord-notifier

Composite GitHub Action that sends a rich Discord embed for the latest commit on push. Works on `ubuntu-latest`, `windows-latest`, and `macos-latest` runners.

## What it sends

An embed with:

- commit message (truncated to 500 chars)
- author name, linked to their GitHub profile
- branch name, linked to the branch
- short SHA, linked to the commit
- files changed count
- lines added / removed
- list of up to `max-files` changed files, each linked, with a note if more were truncated

## Usage

```yaml
name: Discord Commit Notification

on:
  push:
    branches:
      - master

jobs:
  notify-discord:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: filispeen/discord-commit-notify@v1
        with:
          webhook-url: ${{ secrets.DISCORD_WEBHOOK_URL }}
```

`fetch-depth: 0` is required — the action diffs against the previous commit, and a shallow checkout won't have that history.

## Example

<img width="654" height="414" alt="image" src="https://github.com/user-attachments/assets/4bf583d3-c5c3-4d41-be96-bd1b9a423a25" />

## Inputs

| Name | Required | Default | Description |
|---|---|---|---|
| `webhook-url` | yes | - | Discord webhook URL |
| `color` | no | `5814783` | Embed color, decimal integer |
| `max-files` | no | `10` | Max number of changed files listed in the embed |

## Setup

1. In your Discord server: **Server Settings → Integrations → Webhooks → New Webhook**. Copy the URL.
2. In your GitHub repo: **Settings → Secrets and variables → Actions → New repository secret**. Name it `DISCORD_WEBHOOK_URL`, paste the webhook URL.
3. Add the workflow above at `.github/workflows/notify.yml` in your repo.

## License

MIT
