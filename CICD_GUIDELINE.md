# CI/CD Guideline

## `1` Document Information

### `1.A` Scope

#### `1.A.1`
This document provides a baseline for a software repository's Continuous Integration and Continuous Deployment (CI/CD) pipeline: its platform, workflows, integration, version formatting, environments, pipelines, release workflow, deploy job, deployment model, cross-repository triggers and documentation.

#### `1.A.2`
Documentation following this guideline should also inherit the [Language Guideline](./LANGUAGE_GUIDELINE.md).

#### `1.A.3`
The rules in this guideline apply to any platform. Examples are written for GitHub Actions.

### `1.B` Definitions

#### `1.B.1`
Terms in this guideline have the following meanings.

##### `1.B.1(a)`
Definitions:

| Term | Definition |
| --- | --- |
| Pipeline | The workflows that take a repository's code from a commit to a running release. |
| Workflow | A file that defines jobs and the events that start them. |
| Job | A set of steps in a workflow that runs on one runner. |
| Runner | A machine that runs jobs. |
| Hosted runner | A runner the platform provides, started fresh for each job. |
| Self-hosted runner | A runner on a machine outside the platform. |
| Protected branch | A branch that only takes changes through a merge rule. For example, `main`. |
| Protected tag | A tag that only takes pushes a rule allows. For example, a tag ruleset limited to maintainers. |
| Manual run | A run started on request, through the platform's interface or API, rather than by a push, tag or schedule. |
| Version tag | A tag that names a version. |
| Label | The part of a version after its dash, naming a target. For example, `dev` in `1.2.3-dev.2`. |
| Counter | The number after a label, allowing more builds of one version. For example, `2` in `1.2.3-dev.2`. |
| Target | A place a release is installed, named by its label, or `prod` without one. |
| Release | The published files for a version tag, held by the platform. |
| Promoted build | A release whose files come from a tested build instead of a new one. |
| Deploy | Installing a release on its target. |
| Immediate pipeline | A deploy started by a version tag push. |
| Scheduled pipeline | A deploy started by a schedule. |
| Deploy root | The directory on a host that holds its release directories and `current` link. |
| Active release | The release that `current` points at. |

## `2` Guidelines

### `2.A` Platform

#### `2.A.1`
Workflows must go in the folder the platform reads. For example, `.github/workflows/`.

#### `2.A.2`
Jobs should run on hosted runners, ensuring code from a pull request never reaches a private host. A job that must reach a private host or network, or that needs access or hardware a hosted runner lacks, should run on a self-hosted runner inside that network, following [`2.A.3`](#2a3).

#### `2.A.3`
A self-hosted runner should only take jobs from workflows on protected branches, or on protected tags that only the accounts allowed to change a protected branch can push, ensuring a branch cannot change its own workflow to run on it. For example, a runner group limited to `deploy.yml`.

##### `2.A.3(a)`
Where the platform cannot limit a self-hosted runner to certain workflows, only trusted accounts should be able to start a job in its repository, ensuring no one outside them can send the runner a job. For example, approval for every run from an outside contributor.

#### `2.A.4`
Jobs should use a token that expires with the run and grants only what the job needs, not a personal access token. For example, the run's `github.token`.

##### `2.A.4(a)`
Where an outside service offers no token that expires with the run, the job should use the narrowest token the service offers, kept in the target's environment, ensuring a leaked token reaches as little as possible. For example, a Vercel token limited to one team.

#### `2.A.5`
Third-party pipeline steps, and repositories cloned during a run, should be pinned to a full commit hash, with the version in a comment, ensuring a moved tag cannot change what runs. For example, `actions/checkout@{commit_hash} # v4.3.1`.

#### `2.A.6`
Tools downloaded during a run should be pinned to a version and checked against a known checksum, ensuring a replaced file cannot run. For example, `sha256sum --check` with the checksum kept in the workflow.

#### `2.A.7`
A job should pass every setting, secret and event value to its scripts through environment variables, not in the script's text or arguments, ensuring a value can never change the command or show in a process list. For example, an `env:` block.

#### `2.A.8`
A secret that is a binary file should be stored base64-encoded, as a secret holds only text, and decoded into a temporary file only for the step that needs it. For example, an Android signing keystore.

#### `2.A.9`
A secret should hold one value, not structured data, ensuring the platform can hide it in logs. For example, separate secrets instead of one JSON secret.

### `2.B` Workflows

#### `2.B.1`
Every repository should have a `check` workflow. A repository that publishes versions should also have a `release` workflow, and one that installs onto a target should also have a `deploy` workflow, unless an agent on the target deploys, following [`2.F.6`](#2f6).

##### `2.B.1(a)`
Workflows:

| File | Trigger | Purpose |
| ------------- | -------------------------------------- | ------------------------------------------------------------------------------------------- |
| `check.yml` | Push to any branch, pull request | Check the sources. |
| `release.yml` | Called by `deploy`, manual run | Publish the release for a version tag. |
| `deploy.yml` | Version tag push, schedule, manual run | Run `release`, then install the release on its target. |

##### `2.B.1(b)`
A repository without a `deploy` workflow should give `release` the triggers `deploy` would have.

#### `2.B.2`
Another workflow should be added only for a job with its own trigger. For example, copying each published release to a file share.

#### `2.B.3`
Matching a workflow's display name to its file name is recommended, ensuring a failing check points to its file. For example, `name: check` in `check.yml`.

#### `2.B.4`
A workflow or job that a merge rule requires should keep its name, ensuring the rule does not wait on a check that never reports.

### `2.C` Integration

#### `2.C.1`
The `check` workflow should run on every branch push and pull request, and skip tag pushes, ensuring a release's tag does not start it.

##### `2.C.1(a)`
Example:

```yaml
on:
  push:
    branches: ["**"]
  pull_request:

permissions:
  contents: read
```

#### `2.C.2`
The `check` workflow should not publish a build of the app, ensuring every installed build comes from a release.

#### `2.C.3`
The `check` workflow should confirm, at a minimum:

- `2.C.3(a)`
Every source file parses or compiles.

- `2.C.3(b)`
Every configuration template is valid.

- `2.C.3(c)`
The build, lint and tests pass, where the repository has them.

- `2.C.3(d)`
Scripts the pipeline runs pass their own tests. For example, a deploy script.

#### `2.C.4`
A merge should require the `check` workflow to pass. For example, a branch rule that lists its jobs as required checks.

##### `2.C.4(a)`
Where the platform cannot require a check for a merge, the `release` workflow's check in [`2.G.2`](#2g2) is the gate, ensuring an unchecked commit is still never released.

#### `2.C.5`
The `check` workflow should not need secrets, ensuring code or a dependency under review never sees one. Where a process needs a setting to start, the workflow should use a placeholder value. For example, a dummy `NEXTAUTH_SECRET`.

##### `2.C.5(a)`
Where a check needs a private dependency, it should read it without a stored secret, ensuring the check holds none. For example, a package that grants the repository read access, read with the run's own token.

#### `2.C.6`
The `check` workflow should only have repository read permissions, following [`2.A.4`](#2a4). For example, `permissions: contents: read` in [`2.C.1(a)`](#2c1a).

#### `2.C.7`
Running a linter on shell scripts is recommended. For example, `shellcheck`.

### `2.D` Version Formatting

#### `2.D.1`
A version should come from a version tag pushed before the pipeline runs.

#### `2.D.2`
A version should never be worked out by the pipeline. For example, a scheduled run should not raise the patch number.

#### `2.D.3`
No file in the repository should hold the current version, ensuring it can never disagree with its tag.

#### `2.D.4`
Where a tool needs a version in a file, the file should hold a placeholder, and the build should set the real one without a commit. For example, `"version": "0.0.0-placeholder"` in `package.json`.

#### `2.D.5`
A repository should use one tag format for every version, ensuring tag filters and sorting see every release. [Semantic Versioning](https://semver.org/) is recommended.

#### `2.D.6`
A deploy target other than `prod` should have one label of its own, added to the version after a dash. For example, `1.2.3-dev`.

#### `2.D.7`
A labeled version may carry a counter after its label, separated by a dot, allowing more builds without raising the version. For example, `1.2.3-dev.2`.

#### `2.D.8`
A build should take its version from the version tag being deployed, whether the pipeline or a manual deploy runs it, and use it everywhere the app reports a version.

#### `2.D.9`
A release build with no version tag should fail, ensuring an untagged build is never published. A check or local build may use the placeholder version from [`2.D.4`](#2d4).

#### `2.D.10`
Where a platform also needs a build number that only rises, the pipeline may derive it from the version, ensuring a higher version always gets a higher number. For example, an Android `versionCode`.

### `2.E` Environments

#### `2.E.1`
Where a project has more than one target, a tag's label should select the target it names, and a tag without a label should target `prod`. No run-time input should select a target.

##### `2.E.1(a)`
Targets:

| Tag | Release type | Target |
| -------------- | ------------ | ------ |
| `1.3.0-dev.2` | Prerelease | `dev` |
| `1.3.0` | Release | `prod` |

#### `2.E.2`
Environment settings and secrets should live outside the repository and its releases, in a place the deploy or the app can read, ensuring a secret never reaches the repository or a release. For example, GitHub Environments.

##### `2.E.2(a)`
A device installed from an image should get its secrets when it is set up, not from the image. For example, a flashing step that writes them from the secret store.

#### `2.E.3`
Where environment settings and secrets live on the platform, the deploy should apply them to the host on every run, ensuring a change on the platform reaches the host with the next deploy.

#### `2.E.4`
Where the deploy applies settings to the host, it should replace them rather than add to them, ensuring the host never keeps a setting the platform no longer holds.

### `2.F` Pipelines

#### `2.F.1`
The `deploy` workflow should run the `release` workflow, then install what it released.

##### `2.F.1(a)`
Example:

```yaml
on:
  push:
    tags: ["[0-9]+.[0-9]+.[0-9]+*"]
  schedule:
    - cron: "0 9 * * *"
  workflow_dispatch:

jobs:
  release:
    uses: ./.github/workflows/release.yml

  deploy:
    needs: release
    concurrency:
      group: deploy-${{ needs.release.outputs.target }}
      cancel-in-progress: false
```

#### `2.F.2`
Deploys to one target should run one at a time, ensuring two releases never switch the same host at once.

#### `2.F.3`
The `deploy` workflow should run on a version tag push, for the immediate pipeline, or on a schedule, for the scheduled pipeline, and never on a branch push, ensuring every deploy is a tagged version. A manual run should start from the version tag it deploys. For example, a tag chosen as the run's ref.

#### `2.F.4`
A scheduled run should deploy a target named in the workflow, using the highest version tag for that target, when it is higher than the installed version, and do nothing otherwise, ensuring a scheduled run never rolls a target back. For example, the highest `-dev` tag for `dev`.

#### `2.F.5`
A release whose deploy fails should stay as it is, and its fix should ship under a new tag, ensuring a published version never changes: the next counter for a labeled version, or the next patch otherwise. For example, `1.3.0-dev.3` after a failed `1.3.0-dev.2`.

#### `2.F.6`
Where a runner should not reach the target, an agent on the target may deploy instead, following the rules for a deploy job other than [`2.H.1`](#2h1). It should install its target's highest published version when it is higher than the installed version, ensuring the target needs no inbound access.

##### `2.F.6(a)`
Reporting the agent's result to the platform is recommended, ensuring a failed deploy shows beside its release. For example, a deployment status.

#### `2.F.7`
A scheduled run or agent should not retry a version that failed on its target, ensuring a bad release is not installed again on every run. For example, until a manual retry or a higher version.

### `2.G` Release Workflow

#### `2.G.1`
The `release` workflow should have only the permissions it needs, following [`2.A.4`](#2a4). For example, `contents: write` and `actions: read`.

#### `2.G.2`
The `release` workflow should release only a commit whose `check` run passed. It should find that run by the commit, as the `check` workflow skips tags in [`2.C.1`](#2c1). For example, the commit's latest `check.yml` run, read through the API.

#### `2.G.3`
Where the commit's `check` run has not finished, the `release` workflow should wait for it, then fail if it does not pass, ensuring a tag is never left unreleased without notice.

#### `2.G.4`
Where the tag already has a published release, the `release` workflow should use it as it is, ensuring a rerun deploys the same release instead of failing.

#### `2.G.5`
The `release` workflow should publish a release only once all its files are attached, ensuring a published release is always complete. For example, create it as a draft, attach the files, then publish it.

#### `2.G.6`
The `release` workflow should output the version and its target, ensuring the deploy installs the version released, to the target it names.

##### `2.G.6(a)`
Example:

```bash
{
  echo "version=${version}"
  echo "target=${target}"
} >> "${GITHUB_OUTPUT}"
```

#### `2.G.7`
The `release` workflow should mark a release for a labeled tag as a prerelease, ensuring it is never shown as the latest release.

##### `2.G.7(a)`
Example:

```bash
prerelease_flag=$([ -n "${LABEL}" ] && echo --prerelease || true)
gh release create "${TAG}" ${PACKAGE} --draft --title "${TAG}" ${prerelease_flag}
gh release edit "${TAG}" --draft=false
```

#### `2.G.8`
Where the target installs a built package, the `release` workflow should build it and attach it to the release with its checksum, ensuring the deploy can confirm it installs the exact file published.

#### `2.G.9`
A pipeline may build once, test the build, then publish the tested files without rebuilding, ensuring the released files are the files tested. For example, a costly build that needs a hardware test before it is published. A promoted build should:

- `2.G.9(a)`
Take only files whose test passed for those exact files. For example, the release of `1.3.0-dev.2` promoted as `1.3.0` on the same commit.

- `2.G.9(b)`
Change only the files that record the version, setting its own tag's version.

- `2.G.9(c)`
Prove that nothing else differs in what the target runs, ensuring the promoted release runs the tested software. For example, a file-by-file comparison of each image's file systems.

### `2.H` Deploy Job

#### `2.H.1`
The deploy job should declare its target's environment, where the platform has them, ensuring the target's settings and secrets apply.

##### `2.H.1(a)`
Example:

```yaml
runs-on: [self-hosted, "${{ needs.release.outputs.target }}"]
environment: ${{ needs.release.outputs.target }}
```

#### `2.H.2`
The deploy job should install exactly what was released, either by building the released tag on the host or by downloading the release's package and checking it against its checksum, ensuring a target never runs a build that was not released.

#### `2.H.3`
Where a deploy installs onto a host, it should run as an account that owns the deploy root and has no root access other than fixed commands, never a script or file a push can change, ensuring a release's code never runs as root. For example, `sudo systemctl restart {service}`.

#### `2.H.4`
The deploy account should not be in a group that grants root, ensuring it gains no other root access. For example, the `docker` group.

### `2.J` Deployment Model

#### `2.J.1`
A release's database changes should keep working with the release live before it, ensuring a rollback never meets a schema it cannot read.

##### `2.J.1(a)`
Testing a release's database changes against the release live before it is recommended, ensuring a rollback is proven before it is needed. For example, applying the migrations, then running the previous release's tests.

#### `2.J.2`
A deploy should check the release's health once it is live, giving the app time to start. When the check fails, it should return the target to the release live before it and fail, ensuring a bad release never stays live. For example, pointing `current` back and restarting the service.

##### `2.J.2(a)`
Where a release reads its settings from a service when it starts, the deploy should confirm that service answers before it switches, ensuring the rollback can start the release before it.

##### `2.J.2(b)`
Where a target cannot return to an earlier release, the rollback does not apply. For example, a device that refuses a lower build number.

#### `2.J.3`
The health check should make a request that reaches what the release itself needs to serve, and change nothing, ensuring it proves the release works without side effects. For example, an API request that reads the app's own database. A service the app only calls should stay out of the check, as its outage would roll back a good release.

#### `2.J.4`
An alert for a failed release or deploy is recommended, ensuring a failure is noticed without watching the run.

### `2.K` Host Installs

#### `2.K.1`
This section applies where a release installs onto a host as files. A hosting platform or container runtime that keeps each release and switches between them should use its own releases and rollback instead. For example, switching back to the previous deployment.

#### `2.K.2`
A release should install into a new directory under a deploy root, named for its version and its run, ensuring a rerun never writes into the active release and the release live before it stays whole for a rollback.

#### `2.K.3`
A `current` symlink should point at the active release, ensuring the service always runs from one path and a rollback only moves the link.

##### `2.K.3(a)`
Example of [`2.K.2`](#2k2) and [`2.K.3`](#2k3):

```
{deploy_root}/
├── 1.2.0_41/
├── 1.3.0_57/
└── current -> 1.3.0_57
```

#### `2.K.4`
Data the app writes and keeps should live outside the release directories, reached from each release by a link or a setting, ensuring a new release starts with it and removing an old release never deletes it. For example, uploaded files.

#### `2.K.5`
Where a deploy removes old releases, it should never remove the active release or the release live before it, ensuring a rollback stays possible.

### `2.L` Cross-Repository Triggers

#### `2.L.1`
A release that starts a build in another repository should send that repository a named event carrying its version. For example, `repository_dispatch` with a `client_payload`.

#### `2.L.2`
Access to another repository should use a token limited to the repositories it needs, following [`2.A.4`](#2a4). For example, a GitHub App installation token from `actions/create-github-app-token`.

### `2.M` Documentation

#### `2.M.1`
Every repository with a `release` or `deploy` workflow should document its pipeline in `docs/deployment/CICD_DEPLOYMENT.md`, following the [Project Structure Standard](./PROJECT_STRUCTURE_STANDARD.md). These sections are recommended:

- `2.M.1(a)`
Overview: the workflows and what each one checks.

- `2.M.1(b)`
Targets: how a tag selects a target, and which pipeline each target uses.

- `2.M.1(c)`
Configuration: every setting and secret the pipeline reads, and where each lives.

- `2.M.1(d)`
Deployment Model: where a release lands, how the active release is selected, and where the data the app keeps lives.

- `2.M.1(e)`
Release Procedure: the tag format, how to start a release by tag or by a manual run, and what to check afterward, following the [Language Guideline Instructions section](./LANGUAGE_GUIDELINE.md#instructions).

- `2.M.1(f)`
Pipeline Operations: redeploying, rolling back, and applying a changed setting to the host, following the [Language Guideline Instructions section](./LANGUAGE_GUIDELINE.md#instructions). A deploy without the pipeline belongs in `MANUAL_DEPLOYMENT.md`.
