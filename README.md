# Posit CLA Assistant

This repo contains Posit PBC's standardized GitHub Actions workflow for our repositories that require all contributors to sign CLAs.

To enforce CLA signing for your posit-dev repo, follow these steps:

1. File a ticket with CloudOps requesting the `CLA_ASSISTANT_PAT` organizational secret be granted to your repo.
2. Add a file in your repo at `.github/workflows/cla.yml` with these contents:

```yaml
name: "CLA Signature Assistant"
on:
  issue_comment:
    types: [created]
  pull_request_target:
    types: [opened, closed, synchronize]

permissions:
  actions: write
  contents: read
  pull-requests: write
  statuses: write

jobs:
  RequireCLA:
    uses: posit-dev/cla-assistant/.github/workflows/posit-cla.yml@v1
    secrets:
      CLA_ASSISTANT_PAT: ${{ secrets.CLA_ASSISTANT_PAT }}
```

## How it works

This repostiory really just contains an invocation of [CLA Assistant Lite](https://github.com/contributor-assistant/github-action). It's designed to let us change CLA Assistant Lite settings in one place (instead of having to touch all of our CLA-enforcing repos).

Our copy of CLA Assistant Lite is at [posit-dev/cla-assistant-github-action](https://github.com/posit-dev/cla-assistant-github-action), which is a direct fork of the original with no changes; we keep a forked copy solely to ensure the deployment stays under our control (i.e. cannot be changed or deleted without our intervention).

The "database" for CLA Assistant Lite is at [posit-dev/cla-assistant-signatures](https://github.com/posit-dev/cla-assistant-signatures) (a private repo).
