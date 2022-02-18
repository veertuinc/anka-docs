---
---

Microsoft's [Azure Pipelines](https://azure.microsoft.com/en-us/services/devops/pipelines/) provide a useful CI/CD solution within Azure DevOps.

"Azure Pipelines automatically builds and tests code projects to make them available to others. It works with just about any language or project type. Azure Pipelines combines continuous integration (CI) and continuous delivery (CD) to test and build your code and ship it to any target."

Currently, we provide a solution for starting Anka VMs directly on a host and then executing the Anka CLI. This solution does not require the Anka Build Cloud Controller, but does require the Anka Build Cloud Registry.

## Node / Anka CLI Requirements

1. The Anka CLI has a valid registry with the templates/tags you wish to use added (`anka registry add...` + `anka registry list-repos`)
2. Install and register one [Self-hosted agent](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops&tabs=browser#install) per VM you wish to run

## VM Template & Tag Requirements

1. There are no current requirements

## Customize your pipeline YAML

Example pipeline .yml files can be found in our [Azure DevOps Pipeline Examples Repo](https://github.com/veertuinc/azure-devops-pipeline-examples).

[Read more about Pipeline YAML](https://docs.microsoft.com/en-us/azure/devops/pipelines/customize-pipeline?view=azure-devops#understand-the-azure-pipelinesyml-file)
