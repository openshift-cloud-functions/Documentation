# Knative on an OpenShift 4.0 cluster
------

> **IMPORTANT:** The functionality introduced by Knative on an OpenShift cluster is preview only. Red Hat support is not provided, and this release should not be used in a production environment.

## Prerequisites

> **IMPORTANT:** You will need cluster administrator privileges to install and use Knative on an OpenShift cluster.

### Supported platform versions

> **NOTE:** This Knative on OpenShift preview is only available by using the OpenShift 4.0 developer preview. You will require a Red Hat Developers login to try this. Visit [try.openshift.com](https://try.openshift.com/) for getting started information.

| Platform        | Supported versions           |
| ------------- |:-------------:|
| OpenShift      | [4.0 Developer Preview](https://try.openshift.com/)		|

## Installing Knative on an OpenShift cluster using the script provided

1. Login to the cluster using your admin credentials.

   `oc login <admin-credentials>`
   
2. Clone the `knative-operators` repository.

   `git clone https://github.com/openshift-cloud-functions/knative-operators`   
   `cd knative-operators/`   
   `git fetch --tags`   
   `git checkout openshift-v0.3.0`   


3. Navigate to the newly cloned repository and run the `install.sh` script.

   `./etc/scripts/install.sh`  

>**NOTE** The installation script takes around 20-30 minutes to complete, depending on your system.

4. Once the script starts, you will see the following warning and prompt.

   `WARNING: This script will attempt to install Istio, Knative, and OLM in your Kubernetes/OpenShift cluster.`
    
    `If targeting OpenShift, a recent version of 'oc' should be available in your PATH. Otherwise, 'kubectl' will be used.`

    `If using OpenShift 3.11 and your cluster isn't minishift, ensure \$KUBE_SSH_KEY and \$KUBE_SSH_USER are set`

    `Pass -q to disable this prompt`
 
    `Enter to continue or Ctrl-C to exit:`

5. Press Enter to continue.


## Post-installation tasks

### Allowing external access to Knative services for OCP 4.0

#### Updating Knative's config-domain ConfigMap and replacing "example.com" with your domain's suffix.

>**NOTE**: This will not be needed once the new operator capabilities are enabled to set the value automatically.

1. Find out your cluster's domain suffix <YOUR CLUSTER SUFFIX> with the following command:

   `oc get ingresses.config.openshift.io cluster -o jsonpath="{.spec.domain}"`
   
2. Edit Knative's config-domain ConfigMap and replace "example.com" with <YOUR CLUSTER SUFFIX> by using the following command:

   `oc edit configmap config-domain -n knative-serving`
   
   
#### Creating an OpenShift Route pointing to the istio-ingressgateway for each of your Knative Service. 

1. Create a yaml file, "my-route.yaml" with the following content:  
 * Replace <YOUR KNATIVE SERVICE NAME> with your knative service name; 
 * Replace <YOUR NAMESPACE> with your service's namespace;
 * Replace <YOUR CLUSTER SUFFIX> with the value output by the `oc get` command in the previous step.

         apiVersion: v1
          kind: Route
          metadata:
            name: <YOUR KNATIVE SERVICE NAME>
            namespace: istio-system
          spec:
            host: <YOUR KNATIVE SERVICE NAME>.<YOUR NAMESPACE>.<YOUR CLUSTER SUFFIX>
            to:
              kind: Service
              name: istio-ingressgateway

2. Run the following yaml file command:

   `oc apply -f my-route.yaml` 
   
>**NOTE**: If you previously deployed your Knative Services, you will need to redeploy them for the changes to the domain ConfgMap to take effect.
