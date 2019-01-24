# Knative on a Minishift cluster
Preview
------

> **IMPORTANT:** The functionality introduced by  is preview only. Red Hat support is not provided, and this release should not be used in a production environment.

> **NOTE ON INSTALLATION:** All of the required components of Knative on a Minishift cluster, including software dependencies and configurations, can be installed by using the script provided by Red Hat in the OpenShift Cloud Functions `knative-operators` repository. The steps for manual installation are also included in this document to provide information about the steps completed by running the script, however the script installation method is recommended.

## Prerequisites

> **IMPORTANT:** You will need cluster administrator privileges to install and use Knative on a Minishift cluster.

### Supported platform versions

| Platform        | Supported versions           |
| ------------- |:-------------:|
| Minishift      | 1.25.0 |

### Resource requirements

The Minishift cluster must have at least 10GB of memory allocated to run Knative correctly.

If you are running Minishift on your local machine, ensure that virtualization is enabled, as this installation requires the use of virtual machines (KVM).


## Installing Knative on a Minishift cluster using install-on-minishift.sh

> **IMPORTANT:** `install-on-minishift.sh` is destructive. It will destroy any existing Knative profile and create a new one with known-to-work configuration.

1. Clone the `knative-operators` repository.

   `git clone https://github.com/openshift-cloud-functions/knative-operators`   
   `cd knative-operators/`   
	 `git fetch --tags`   
	 `git checkout openshift-v0.2.0`

2. Confirm that your Minishift instance has been stopped.

	`minishift stop`

3. Navigate to the newly cloned repository and run the install-on-minishift.sh script.

	`./etc/scripts/install-on-minishift.sh`

