# Knative on an OpenShift cluster
Developer Preview 0.3.0
------

> **IMPORTANT:** The functionality introduced by Knative on an OpenShift cluster is developer preview only. Red Hat support is not provided, and this release should not be used in a production environment.

> **NOTE ON INSTALLATION:** All of the required components of Knative on an OpenShift cluster, including software dependencies and configurations, can be installed by using the script provided by Red Hat in the OpenShift Cloud Functions `knative-operators` repository. The steps for manual installation are also included in this document to provide information about the steps completed by running the script, however the script installation method is recommended.

## Prerequisites

> **IMPORTANT:** You will need cluster administrator privileges to install and use Knative on an OpenShift cluster.

### Supported platform versions

| Platform        | Supported versions           |
| ------------- |:-------------:|
| OpenShift      | 3.11 or newer |

> **IMPORTANT:** Docker and Kubernetes are also required to install and use Knative on an OpenShift cluster, however these components are outside the scope of this documentation.
