# DCM CI Modernization

This repository describes a GitHub Actions model for DCM CI. Each approved
target host runs one self-hosted GitHub Actions runner. The runner starts the
CI execution container locally for a workflow run.

## Add a CI runner to a host

### Prerequisites

- An approved Linux target host.
- A container runtime on the host.
- Network access from the host to GitHub.
- Repository administrator access to create a short-lived runner registration
  token.
- The approved runner image and CI execution image.

### Register the runner

1. In this repository, open **Settings → Actions → Runners**.
2. Select **New self-hosted runner** and choose Linux.
3. Follow GitHub's displayed registration instructions on the target host.
   Treat the registration token as a short-lived secret. Do not add it to Git,
   images, or workflow files.
4. Run the GitHub runner as a container. Configure it to restart after a host
   reboot.
5. Add the labels required by the workflows. For example: `self-hosted`,
   `linux`, `dcm-ci`, and either a target label or `ci-pool` for a shared CI
   host.

### Verify

1. Return to **Settings → Actions → Runners**.
2. Confirm that the runner status is **Idle**.
3. Run the manual CI workflow with a safe test option.
4. Confirm that the workflow selects the expected labels and the execution
   container starts on the selected host.

## Container model

The host runs the GitHub Actions runner container continuously. A workflow run
starts a separate, short-lived CI execution container. That container contains
the required automation tools and can run the existing Jenkins-based flow
during the migration.

## Safety

Use these runners only for trusted manual or scheduled workflows. Do not run
code from public pull requests or forks on a runner that can access a deployment
host, containers, or local secrets.

For the full design, see
[DCM CI Modernization Enhancement](DCM_CI_MODERNIZATION_ENHANCEMENT.md).
