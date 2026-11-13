# Workload Target Requirements Supplemental Material

The following content supplements the [Device Requirements](https://docs.margo.org/specification/margo-devices/device-requirements) documentation in the core specification with requirements clarifications specific to workload targets supporting the Helm deployment technology.

Workload targets supporting the Helm deployment technology MUST provide the following additional components:

- Ability to deploy Helm charts
- Kubernetes orchestration platform

## Helm deployment approaches

Workloads can be deployed as Helm charts using either Helm [version 3](https://helm.sh/docs/v3/topics/charts) or [version 4](https://helm.sh/docs/topics/charts) using Chart APIVersion v2 only.

This specification does not dictate how workload targets deploy workloads packaged as Helm charts. A device vendor may choose a deployment approach that either interacts with the Kubernetes API (such as `helm install`) or an approach that renders the Helm templates and applies the templates (such as `helm template` or Kustomization). In order to facilitate this choice, certain Helm functions are not supported. See the [Special Constraints](../../README.md#special-constraints) for more details.

### Probationary support for `.Capabilities.APIVersions.Has` function

**Feedback Wanted:** We are looking for feedback on supporting this function. If you are using a deployment method that does not support passing the list of Kubernetes APIs out of band, please let us know.

If a device vendor chooses a deployment approach that does not interact with the Kubernetes API, the list of Kubernetes APIs available in the cluster MUST be provided out-of-band. This is supported by a variety of tooling such as the Helm GO SDK, `helm template` command, Kustomization, and ArgoCD.
