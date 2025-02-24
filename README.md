# Azure Pipeline SPS Tasks

General extensions for Azure Pipelines for internal SPS Commerce custom tasks. Tasks are created as a logical group inside an extension. This repository is set up with templates to deploy one extension at a time, each containing one or more related custom tasks.

### Current version

- Agent version to target: 3.218.1
- Node version to target: Node 16 and Node 1878

## Tasks

Available task documentation in this repository:
| Task  | Marketplace | Description | Status | 
|---|---|---|---|
| [Artifact Cache](extensions/artifact-cache) | [Marketplace](https://marketplace.visualstudio.com/items?itemName=SPSCommerce.ArtifactCaching) | Cache artifacts such as Maven, NPM or NuGet into an AWS S3 Bucket between builds.  | [![Build Status](https://dev.azure.com/spscommerce/azp-infrastructure/_apis/build/status/extensions/extension-artifact-cache?branchName=main)](https://dev.azure.com/spscommerce/azp-infrastructure/_build/latest?definitionId=684&branchName=main)  |
| [Artifact Version](extensions/artifact-version) | [Marketplace](https://marketplace.visualstudio.com/items?itemName=SPSCommerce.ArtifactVersioning) | Tasks for determining version numbers on artifacts and builds. Include [semantic-release](https://semantic-release.gitbook.io/semantic-release/) plugin.  | [![Build Status](https://dev.azure.com/spscommerce/azp-infrastructure/_apis/build/status/extensions/extension-artifact-version?branchName=main)](https://dev.azure.com/spscommerce/azp-infrastructure/_build/latest?definitionId=806&branchName=main)  |
| [BDP Core](extensions/bdp-core) | [Marketplace](https://marketplace.visualstudio.com/items?itemName=SPSCommerce.BDPCore) | BDP Core API Integration Tasks  | [![Build Status](https://dev.azure.com/spscommerce/azp-infrastructure/_apis/build/status/extensions/extension-bdp-core?branchName=main)](https://dev.azure.com/spscommerce/azp-infrastructure/_build/latest?definitionId=762&branchName=main)  |
| [DevCenter](extensions/dev-center) | [Marketplace](https://marketplace.visualstudio.com/items?itemName=SPSCommerce.DevCenter) | DevCenter and K8 deployment tasks  | [![Build Status](https://dev.azure.com/spscommerce/azp-infrastructure/_apis/build/status/extensions/extension-dev-center?branchName=main)](https://dev.azure.com/spscommerce/azp-infrastructure/_build/latest?definitionId=699&branchName=main)  |
| [Private Packages](extensions/private-packages) | [Marketplace](https://marketplace.visualstudio.com/items?itemName=SPSCommerce.PrivatePackages) | Private Registry, Index and Module config setup with authorization for internal SPS Commerce locations (Jfrog, AzP, GitHub)  | [![Build Status](https://dev.azure.com/spscommerce/azp-infrastructure/_apis/build/status/extensions/extension-private-packages?branchName=main)](https://dev.azure.com/spscommerce/azp-infrastructure/_build/latest?definitionId=850&branchName=main)  |
| [SPS Workflow](extensions/workflow) | [Marketplace](https://marketplace.visualstudio.com/items?itemName=SPSCommerce.Workflow) | Custom workflow tasks for managing and orchestrating build and deploy (including change request automation).  | [![Build Status](https://dev.azure.com/spscommerce/azp-infrastructure/_apis/build/status/extensions/extension-workflow?branchName=main)](https://dev.azure.com/spscommerce/azp-infrastructure/_build/latest?definitionId=914&branchName=main)  |
| [Browser](extensions/browser) | [Marketplace](https://marketplace.visualstudio.com/items?itemName=SPSCommerce.Browser) | Custom browser task for extension creation.  | [![Build Status](https://dev.azure.com/spscommerce/azp-infrastructure/_apis/build/status/extensions/extension-browser?branchName=main)](https://dev.azure.com/spscommerce/azp-infrastructure/_build/latest?definitionId=1030&branchName=main)  |
| [API](extensions/api) | [Marketplace](https://marketplace.visualstudio.com/items?itemName=SPSCommerce.API) | Custom task for API Design first.  | [![Build Status](https://dev.azure.com/spscommerce/azp-infrastructure/_apis/build/status/extensions/extension-api?branchName=main)](https://dev.azure.com/spscommerce/azp-infrastructure/_build/latest?definitionId=1395&branchName=main)  |


## Managing Extensions

All deployed extensions are available on the Azure DevOps Marketplace as private extensions that are shared with the `spscommerce` Azure DevOps organization. The extensions and the private sharing can be managed via this portal: 
https://marketplace.visualstudio.com/manage/ 
- User: azure-pipelines-admin@spscommerce.com
- Password: PMP OR SPSSREPROD Secret For "Azure DevOps Tokens"

Extensions can be viewed privately when logged into the marketplace. This allows viewing of the associated information for that extension including documentation, build badges and repository. See an [example here](https://marketplace.visualstudio.com/items?itemName=SPSCommerce.ArtifactCaching).

Extensions within the `spscommerce` Azure DevOps organization can be managed and reviewed here:
https://dev.azure.com/spscommerce/_settings/extensions

After publishing a new extension from this repository there is a one-time click to install the extension. All updates to that extension after that are automatically available.

## Writing a Task

Tasks should be fully self-contained within their extension, meaning there is no repository-wide `package.json` or typescript implementation. Each implementation is at the task level allowing full control and flexibility. That being said, each task should be written using the same version of node, npm and typescript (defined in the [Dockerfile](Dockerfile)). 

Extension versions are auto-incremented with every deploy, _but task versions are not._ You'll need to manually increment the semantic version of your task after making an update for it to deploy an update to that task (even though the extension version is automatic). This is updated within the `task.json` in each task folder.

#### Local Development Node Version

As many of the tests do not make use of the [Mock test Node Handler](https://github.com/microsoft/azure-pipelines-task-lib/blob/master/node/docs/nodeVersioning.md#mock-test-node-handler), it is worthwhile to use the currently targeted node version [listed above](#node-versions) to run tests. To facilitate this it is highly recommended developers use [nvm](https://github.com/nvm-sh/nvm) as an [`.nvmrc`](https://github.com/nvm-sh/nvm#nvmrc) has be provided [here][nvmrc] with the currently targeted version of node selected.

To take advantage of the `.nvmrc` run `nvm use` in the root directory of this repository:

```console
$ nvm use
Found '/Users/jdmoldenhauer/src/SPSCommerce/azure-pipelines-extensions/.nvmrc' with version <v10.24.1>
Now using node v10.24.1 (npm v6.14.12)
```

##### Automated Detection and Loading of `.nvmrc`

nvm [provides direction](https://github.com/nvm-sh/nvm#deeper-shell-integration) for how to call `nvm use` automatically in a directory with an `.nvmrc` file. It amounts ot adding a function to the `.*rc` file of the flavor of shell you prefer.

```console
$ cd azure-pipelines-extensions
Found '/Users/jdmoldenhauer/src/SPSCommerce/azure-pipelines-extensions/.nvmrc' with version <v10.24.1>
Now using node v10.24.1 (npm v6.14.12)

$ cd ..
Reverting to nvm default version
Now using node v14.17.0 (npm v8.5.2)
```

Users of zsh, and oh-my-zsh can opt into this functionality by activating the `nvm` plugin and adding `export NVM_AUTOLOAD=1` to their `.zprofile`.

## Secrets

Secrets were initially stored in AWS using SSM Parameter Store. On each request the secret is pulled for every task execution that requires it. Ongoing `Rate Exceeded` issues continue to occur in a single account like spsc/spscommerce. All secrets are pushed to SSM path `/vsts/` via an [automated pipeline](https://dev.azure.com/spscommerce/azp-infrastructure/_release?_a=releases&view=mine&definitionId=7) that additionally copies them to both spsc/spscommerce accounts for usage.

SSM Parameter Store was used for secrets in the past.
new secrets should be created in the existing Secret resource in AWS Secrets Manager deployed to the AZP agents Product Account. 
As updates are made, effort to move existing secrets from SSM to Secrets Manager is highly encouraged to help reduce `rate throttling` concerns (for ourselves and to be a good citizen for others). 
Since AWS Secrets Manager allows for cross-account communication, this additionally enables secrets to be used multi-account really easily when referenced by full ARN.
Additionally, multiple copies of the secrets are not necessary and this secret is the only one used by all agents:
`arn:aws:secretsmanager:us-east-1:229613081102:secret:4c03b50f-db44-49af-a97e-0e732ce51182/prod/azure-pipelines-tasks`

For local development, assuming the SRE role in the PROD Team Account(`645896758999`) or in AZP agents product account(`229613081102`) will work for accessing the secret:
`arn:aws:iam::645896758999:role/admin/SRERole`. The Secret value is only filled within the PROD account since secrets would be the
same across all environments with third party access. However, this pattern should be reviewed in the future, cross environment access for obtaining secrets should be avoided.

## Local Testing

Requires node and Typescript versions found in Dockerfile[.Dockerfile]

Some tasks have tests, others less so. YMMV.
Once in a directory for a task, build and test:

```
tsc

# assume role in spssreprod account to run the test
npm run test
```

## Linting

ESLint is the standard that we use at SPS for linting JavaScript and TypeScript files.

### Run ESLint on file save:
1. Install the ESLint extension from the VS Code marketplace.
2. Open the VS Code settings (File > Preferences > Settings).
3. Search for "eslint.autoFixOnSave" in the settings search bar.
4. Check the box to enable ESLint to automatically fix issues on file save.
5. Save the settings file.

For more information on using ESLint with VS Code, refer to the official documentation:
https://eslint.org/docs/user-guide/integrations#visual-studio-code


<!-- Links -->

[agent]: https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops&tabs=browser
[node-versioning]: https://github.com/microsoft/azure-pipelines-task-lib/blob/master/node/docs/nodeVersioning.md
[node-environment]: https://github.com/microsoft/azure-pipelines-task-lib/blob/master/node/docs/nodeEnvironment.md
[nvmrc]: .nvmrc
