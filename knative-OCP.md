# Knative on an OpenShift cluster
Preview
------

> **IMPORTANT:** The functionality introduced by Knative on an OpenShift cluster is preview only. Red Hat support is not provided, and this release should not be used in a production environment.

## Prerequisites

> **IMPORTANT:** You will need cluster administrator privileges to install and use Knative on an OpenShift cluster.

### Supported platform versions

| Platform        | Supported versions           |
| ------------- |:-------------:|
| OpenShift      | 4.0		|

## Installing Knative on an OpenShift cluster using the script provided

1. Login to the cluster using your admin credentials.

   `oc login <admin-credentials>`
   
2. Clone the `knative-operators` repository.

   `git clone https://github.com/openshift-cloud-functions/knative-operators`   
   `cd knative-operators/`   
   `git fetch --tags`   
   `git checkout openshift-v0.2.0`   

3. Set the following SSH properties using your OpenShift cluster credentials.

   `export KUBE_SSH_USER=<username>`   
   `export KUBE_SSH_KEY=<path-to-private-ssh-key>`   

4. Navigate to the newly cloned repository and run the `install.sh` script.

   `./etc/scripts/install.sh`  

>**NOTE** The installation script takes around 20-30 minutes to complete, depending on your system.

5. Once the script starts, you will see the following warning and prompt.

   `WARNING: This script will blindly attempt to install OLM, istio, and knative on your OpenShift cluster, so if   any are already there, hijinks may ensue.`

   `If your cluster isn't minishift, ensure $KUBE_SSH_KEY and $KUBE_SSH_USER are set`   

   `Pass -q to disable this warning`   

   `Enter to continue or Ctrl-C to exit:`   

   Press Enter to continue.
