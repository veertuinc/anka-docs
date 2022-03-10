---
title: "Troubleshooting"
weight: 99
hideSubPagesInLeftMenu: true
showBaseDirName: true
---
{{< hint error >}}
EOL and Deprecation list:
- Jenkins Plugin versions < 2.0 EOL
- Anka GitLab Runner versions < 1.0 EOL
{{< /hint >}}

**Once you have collected the logs, go through the below mentioned troubleshooting steps and then contact support.**

---

### Troubleshooting Checklists

#### Gitlab Runner

1. Find the job's console log inside of your Gitlab instance and gather the VM and node information from it.
1. If you have access to the gitlab-runner logs, collect those for the time period of the failure. Logs are labelled with `job=#`, matching the job ID.
1. Collect the logs from the Anka Build Cloud Controller from the time period of the failure.

#### Jenkins

1. Find the job's console log inside of your Gitlab instance and gather the VM and node information from it.
1. Collect the logs from the Anka Build Cloud Controller from the time period of the failure.

#### Buildkite

#### Github Actions

#### Azure Devops Pipelines

---

## Troubleshooting Guides