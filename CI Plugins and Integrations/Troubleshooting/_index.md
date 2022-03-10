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

These checklists are to give you an idea of what logs and where to look for indications of exactly what's wrong should a problem arise.

#### Gitlab Runner

1. Find the job's console log inside of your Gitlab instance and gather the VM and node information from it.
1. If you have access to the gitlab-runner logs, collect those for the time period of the failure. Logs in the runner are labelled with `job=#`, matching the job ID.
1. Collect the logs in the Anka Build Cloud Controller from the time period of the failure (`Controller > Logs > Service: Controller`)

Our gitlab runner is based on the core and official gitlab runner release, so the [official troubleshooting guide](https://docs.gitlab.com/runner/faq/) is also good to review.

#### Jenkins

1. When you first kick off a job in Jenkins, you can find the "node" or "computer" Jenkins ties the VM our plugin spins up to at the top of the Console Output. If you go to the node page, you'll be able to access the logs for Jenkins connecting to the Anka VM. This is useful to check first, but the log is only available until the VM is terminated and Jenkins deletes the node.
1. Gain a understanding of what specifically is failing from the Console Output in the Jenkins job.
1. Collect the logs for the Anka Build Cloud Controller from the time period of the failure (`Controller > Logs > Service: Controller`).
1. Find the Jenkins System Log and look at the time of the failure for any errors related to the Anka plugin or Jenkins in general.
1. Collect the logs in the Anka Build Node from the time period of the failure (`Controller > Logs > Service: {Specific Node Name}`).

#### Buildkite

#### Github Actions

#### Azure Devops Pipelines

---

## Troubleshooting Guides