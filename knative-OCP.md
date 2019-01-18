# Knative on an OpenShift cluster
Developer Preview 0.3.0
------

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Knative on an OpenShift cluster](#knative-on-an-openshift-cluster)
	- [Prerequisites](#prerequisites)
		- [Supported platform versions](#supported-platform-versions)
	- [Installing Knative on an OpenShift cluster using the script provided (recommended)](#installing-knative-on-an-openshift-cluster-using-the-script-provided-recommended)
	- [Manual installation steps](#manual-installation-steps)
	- [Post-installation tasks](#post-installation-tasks)
		- [Allowing external access to Knative services using a configured hostname.](#allowing-external-access-to-knative-services-using-a-configured-hostname)
	- [Additional resources](#additional-resources)

<!-- /TOC -->

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

2. Set the following SSH properties using your OpenShift cluster credentials.

   `export KUBE_SSH_USER=<username>`   
   `export KUBE_SSH_KEY=<path-to-private-ssh-key>`   

2. Navigate to the newly cloned repository and run the `install.sh` script.

   `$ ./etc/scripts/install.sh`  

>**NOTE** The installation script takes around 20-30 minutes to complete, depending on your system.

3. Once the script starts, you will see the following warning and prompt.

   `WARNING: This script will blindly attempt to install OLM, istio, and knative on your OpenShift cluster, so if   any are already there, hijinks may ensue.`

   `If your cluster isn't minishift, ensure $KUBE_SSH_KEY and $KUBE_SSH_USER are set`   

   `Pass -q to disable this warning`   

   `Enter to continue or Ctrl-C to exit:`   

   Press Enter to continue.
   
## Manual installation steps

If you wish to install Knative on an OpenShift cluster manually (without using the script), you will need to complete the following steps which are currently included in `install.sh`.

1. Enable admission webhooks
2. Install OLM
3. Install Istio
4. Install Knative components (Knative Build, Knative Serving, Knative Eventing and Operator groups).
5. Skip tag resolution for internal registry.

   `oc -n knative-serving get cm config-controller -oyaml | sed "s/\(^ *registriesSkippingTagResolving.*$\)/\1,docker-registry.default.svc:5000/" | oc apply -f -`   
   
6. Add image streams to build images.
7. Add required Istio permissions.

   `oc adm policy add-scc-to-user privileged -z default`   
   `oc adm policy add-scc-to-user anyuid -z default`   

## Post-installation tasks

### Allowing external access to Knative services using a configured hostname.

Your Knative services will not be accessible from outside of the OpenShift cluster by default, however you can enable access by configuring a hostname. This method uses wildcard routes to automatically assign a hostname to Knative services.

1. Find the default routing subdomain of your OpenShift cluster. For more information, see [Additional resources](#additional-resources).
2. Open the config map for your cluster to edit the Knative serving configuration.

   `oc edit configmap config-domain -n knative-serving`   

3. Replace the default "example.com" with your default routing subdomain.
4. Configure your router to allow wildcard domains.

   `oc set env dc/router ROUTER_ALLOW_WILDCARD_ROUTES=true -n default`   

5. Create a route that forwards wildcard traffic to the `knative-ingressgateway`.
   > **NOTE:** This route requires one host per namespace that is running Knative services. The format for the host of the route is `wildcard.<namespace>.<default-routing-subdomain>`.

  To do this, you must create a wildcard yaml file (in our example this is named `wildcard-routes.yml`) with the following contents.

    `apiVersion: route.openshift.io/v1
    kind: Route
      metadata:
      name: knative-ingressgateway-wildcard
      namespace: <namespace-running-knative-services>
    spec:
      host: wildcard.<namespace>.<default-routing-subdomain>
      port:
        targetPort: http2
      to:
        kind: Service
        name: knative-ingressgateway
        weight: 100
        wildcardPolicy: Subdomain`   

  Apply the yaml file.

     `oc apply -f wildcard-routes.yml`   

6. Clone the demo repository.

   `git clone https://github.com/openshift-cloud-functions/demos.git`

7. Deploy the demo application.

   `oc apply -f ~/demos/knative-kubecon/serving/010-service.yaml`   

8. View the domain that was assigned to the application. You can now access the demo application using this domain.

   `oc get service.serving.knative.dev/dumpy`   

## Additional resources

* For more information about default routing subdomains, see [OpenShift documentation](https://docs.openshift.com/enterprise/3.0/install_config/install/deploy_router.html#customizing-the-default-routing-subdomain).
