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

## Installing Knative on an OpenShift cluster using the script provided (recommended)

1. Clone the `knative-operators` repository.

   `$ git clone git@github.com:openshift-cloud-functions/knative-operators.git`  

2. Set the following SSH properties.

   `export KUBE_SSH_USER=ec2-user`   
   `export KUBE_SSH_KEY=~/.ssh/ocp-workshop.pem`   

2. Navigate to the newly cloned repository and run the `install.sh` script.

   `$ ./etc/scripts/install.sh`  

>**NOTE** The installation script takes around 20-30 minutes to complete, depending on your system.

3. Once the script starts, you will see the following warning and prompt.

   `WARNING: This script will blindly attempt to install OLM, istio, and knative on your OpenShift cluster, so if   any are already there, hijinks may ensue.`
   
   `If your cluster isn't minishift, ensure $KUBE_SSH_KEY and $KUBE_SSH_USER are set`   
   
   `Pass -q to disable this warning`   
   
   `Enter to continue or Ctrl-C to exit:`   
   
   Press Enter to continue.

## Post-installation tasks

### Creating routes

Your Knative services will not be accessible from outside of the OpenShift cluster by default, however you can enable access by creating routes.

1. Expose the `knative-ingressgateway`.

   `oc -n istio-system expose svc/knative-ingressgateway`   

2. Check that the route has been created and note the hostname.

   `oc get route -n istio-system knative-ingressgateway`   

3. Deploy your source code and access it through the route.

   `curl -v -H "Host: hello-go.default.example.com" http://knative-ingressgateway-istio-system.apps.<yourcity-guid>.openshiftworkshop.com:32380`   
   