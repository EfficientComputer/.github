# Efficient Computer Engineering Organization

Welcome to the Efficient Computer engineering organization! This repository contains shared workflows, templates, and community health files used across all our repositories.

## 🔄 Shared Workflows

### Slack PR Notifications

A reusable workflow that posts Slack notifications when PRs are requested for review from specific teams.

**Location**: `.github/workflows/slack-pr-notifications.yml`

**Features**:
- Posts notification when team review is requested
- Adds reaction emojis based on review state (✅ approved, 🔧 changes requested, 💬 commented)  
- Posts thread replies with reviewer name and review state
- Strikes through message when PR is closed/merged
- Supports multiple teams with different channels

**Usage in any repository**:

```yaml
# .github/workflows/pr-notifications.yml
name: PR Notifications
on:
  pull_request:
    types: [review_requested, closed]
  pull_request_review:
    types: [submitted]

permissions:
  contents: read
  issues: write
  pull-requests: write

jobs:
  slack-notifications:
    uses: Efficient-Computer/.github/.github/workflows/slack-pr-notifications.yml@main
    with:
      team_slug: "architecture"  # Change to your team slug
    secrets:
      SLACK_BOT_TOKEN: ${{ secrets.SLACK_PR_NOTIFIER_BOT_TOKEN }}
      SLACK_CHANNEL: ${{ secrets.SLACK_PR_NOTIFIER_CHANNEL }}
```

**Required secrets** (set these in your repository or organization):
- `SLACK_PR_NOTIFIER_BOT_TOKEN`: Slack bot token with chat:write permissions
- `SLACK_PR_NOTIFIER_CHANNEL`: Slack channel ID (e.g., "C1234567890")

**Supported teams**: Configure `team_slug` input to match your GitHub team slug (e.g., "architecture", "compiler", "applications")
