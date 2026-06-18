---
date: 2019-07-03T22:24:47-05:00
title: "Using Github Actions and the Anka Build Cloud"
linkTitle: "Github Actions"
weight: 3
description: >
  Instructions on how to use Github Actions with the Anka Build Cloud
aliases:
  - /ci-plugins-and-integrations/controller-+-registry/github-actions
---

The [Anka Actions](https://github.com/veertuinc/anka-actions) monorepo provides GitHub Actions for orchestrating ephemeral, self-hosted runners on Anka Build Cloud. Each action lives in its own subfolder and is referenced by path. You can also find them on the [GitHub Marketplace](https://github.com/marketplace/actions/anka-actions).

| Action | Path | Description |
| --- | --- | --- |
| Anka Actions - Up | `veertuinc/anka-actions/up@v2.0.0` | Spins up a new Anka VM instance and registers a self-hosted runner. |
| Anka Actions - Down | `veertuinc/anka-actions/down@v2.0.0` | Tears down the Anka VM instance and removes the runner. |

## VM Template & Tag Requirements

1. Install the GitHub runner inside of your VM. We recommend using [our installation script](https://github.com/veertuinc/getting-started/blob/master/GITHUB_ACTIONS/install.sh).

2. Create a GitHub [personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) and add it as a repository secret. The token **owner** must be a repository **Admin** — permissions on the token cannot exceed the account's role.

   - **Classic PAT:** `repo` scope
   - **Fine-grained PAT:** resource owner = org/user that owns the repo; **Administration: Read and write** on the target repo

## Usage

Reference each action by its folder path within the [anka-actions](https://github.com/veertuinc/anka-actions) repository:

```yaml
jobs:
  action-up:
    runs-on: ubuntu-latest
    steps:
      - uses: veertuinc/anka-actions/up@v2.0.0
        id: action-up
        with:
          gh-pat: ${{ secrets.SERVICE_USER_PAT }}
          template-id: '9690461a-02b5-412d-8778-dab4167743db'
          controller-url: 'https://controller.mysite.com'
    outputs:
      action-id: ${{ steps.action-up.outputs.action-id }}

  inside_vm_job:
    needs: action-up
    runs-on: [ self-hosted, "${{ needs.action-up.outputs.action-id }}" ]
    steps:
      - name: Inside VM Job
        run: echo "running on runner inside of VM (${{ needs.action-up.outputs.action-id }})"

  action_down:
    if: always()
    needs: [ action-up, inside_vm_job ]
    runs-on: ubuntu-latest
    steps:
      - uses: veertuinc/anka-actions/down@v2.0.0
        with:
          action-id: ${{ needs.action-up.outputs.action-id }}
          gh-pat: ${{ secrets.SERVICE_USER_PAT }}
          controller-url: 'https://controller.mysite.com'
```

See the repository READMEs for the full list of inputs, outputs, and advanced configuration (certificate auth, root token auth, GitHub Enterprise Server, etc.):

- [Anka Actions - Up](https://github.com/veertuinc/anka-actions/blob/edge/up/README.md)
- [Anka Actions - Down](https://github.com/veertuinc/anka-actions/blob/edge/down/README.md)

---

## Release Notes

Release notes for the Anka GitHub Actions can be found on the [anka-actions releases page](https://github.com/veertuinc/anka-actions/releases).
