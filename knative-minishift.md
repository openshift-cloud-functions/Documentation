# Knative on a Minishift cluster
Developer Preview
------

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Knative on a Minishift cluster](#knative-on-a-minishift-cluster)
	- [Prerequisites](#prerequisites)
		- [Supported platform versions](#supported-platform-versions)
		- [Resource requirements](#resource-requirements)
		- [Installing Minishift](#installing-minishift)
			- [Installing dependencies for a new Minishift cluster](#installing-dependencies-for-a-new-minishift-cluster)
	- [Installing Knative on a Minishift cluster using install-on-minishift.sh](#installing-knative-on-a-minishift-cluster-using-install-on-minishiftsh)
	- [Installing Knative on a Minishift cluster manually](#installing-knative-on-a-minishift-cluster-manually)
		- [Configuring Minishift for Knative](#configuring-minishift-for-knative)
		- [Installing Operator Lifecycle Manager (OLM)](#installing-operator-lifecycle-manager-olm)
		- [Installing Istio add-on for Minishift](#installing-istio-add-on-for-minishift)
		- [Installing Knative on Minishift](#installing-knative-on-minishift)
			- [Installing Knative build](#installing-knative-build)
			- [Installing Knative serving](#installing-knative-serving)
			- [Installing Knative eventing](#installing-knative-eventing)
	- [Additional resources](#additional-resources)

<!-- /TOC -->

> **IMPORTANT:** The functionality introduced by  is developer preview only. Red Hat support is not provided, and this release should not be used in a production environment.

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

### Installing Minishift

See the Minishift documentation in [Additional resources](#additional-resources) for more information on installing a new Minishift instance.

#### Installing dependencies for a new Minishift cluster

> **IMPORTANT:** Docker and Kubernetes are also required to install and use Knative on a Minishift cluster, however these components are outside the scope of this documentation.

## Installing Knative on a Minishift cluster using install-on-minishift.sh

1. Start Minishift.

   `minishift start`  

2. Clone the OLM repository.

   `git clone git@github.com:operator-framework/operator-lifecycle-manager.git`  

3. Navigate to the newly cloned repository and run the `install-on-minishift.sh` script.

   `./etc/scripts/install-on-minishift.sh`  

## Installing Knative on a Minishift cluster manually

1. Start Minishift.

   `minishift start`  

2. Clone the OLM repository.

	    `git clone git@github.com:operator-framework/operator-lifecycle-manager.git`  

3. Set the required environment variables.

	 	 `eval $(minishift oc-env)`  
	 	 `eval $(minishift docker-env)`  

4. Login as administrator.

	 	`oc login -u system:admin`  

### Configuring Minishift for Knative

You will need to set the correct configuration before starting Minishift. You can do this by using the commands

   `minishift profile set knative`  
	 `minishift config set openshift-version v3.11.0`  
	 `minishift config set memory 10GB`  
	 `minishift config set cpus 4`  
	 `minishift config set disk-size 50g`  
	 `minishift config set image-caching true`  
	 `minishift addons enable admin-user`  
	 `minishift addons enable anyuid`  

### Installing Operator Lifecycle Manager (OLM)

Navigate to the `operator-lifecycle-manager` directory and install the latest OLM release using the following command.

   `oc create -f deploy/okd/manifests/latest/`  

>**NOTE** When starting Minishift to install OCF, or completing other tasks,  leave this terminal open and start a new one.

### Installing Istio add-on for Minishift
[See Istio instructions here](https://github.com/minishift/minishift-addons/tree/master/add-ons/istio#istio-add-on)

### Installing Knative on Minishift

Install the `knative-operators` catalog source.

   `oc apply -f https://raw.githubusercontent.com/openshift-cloud-functions/knative-operators/master/knative-operators.catalogsource.yaml`  

#### Installing Knative build

1. Create the `knative-build` project and namespace.

   `oc new-project knative-build`  
   `oc create ns knative-build`   

2. Ensure that you are in the `knative-build` namespace.

  `oc project knative-build`  

3. Create and apply a new Subscription.

   `cat <<EOF | oc apply -f -`   
    `apiVersion: operators.coreos.com/v1alpha1`   
    `kind: Subscription`   
    `metadata:`   
      `name: knative-build-subscription`   
      `generateName: knative-build-`   
      `namespace: knative-build`   
    `spec:`   
      `source: knative-operators`   
      `name: knative-build`   
      `startingCSV: knative-build.v0.2.0`   
      `channel: alpha`   
   `EOF`   

#### Installing Knative serving

1. Create the `knative-serving` project and namespace.

   `oc new-project knative-serving`  
   `oc create ns knative-serving`   

2. Ensure that you are in the `knative-serving` namespace.

    `oc project knative-serving`  

3. Create and apply a new Subscription.

    `cat <<EOF | oc apply -f -`   
    ` apiVersion: operators.coreos.com/v1alpha1`   
    `kind: Subscription`   
    `metadata:`   
    `name: knative-serving-subscription`   
      `generateName: knative-serving-`   
      `namespace: knative-serving`   
    `spec:`   
      `source: knative-operators`   
      `name: knative-serving`   
      `startingCSV: knative-serving.v0.2.2`   
      `channel: alpha`   
      `EOF`   

#### Installing Knative eventing

1. Create the `knative-eventing` project and namespace.

    `oc new-project knative-eventing`  
    `oc create ns knative-eventing`   

2. Ensure that you are in the `knative-eventing` namespace.

    `oc project knative-eventing`  

3. Create and apply a new Subscription.

    `cat <<EOF | oc apply -f -`   
    `apiVersion: operators.coreos.com/v1alpha1`   
    `kind: Subscription`   
    `metadata:`   
      `name: knative-eventing-subscription`   
      `generateName: knative-eventing-`   
      `namespace: knative-eventing`   
    `spec:`   
      `source: knative-operators`   
      `name: knative-eventing`   
      `startingCSV: knative-eventing.v0.2.1`   
      `channel: alpha`   
   `EOF`   

## Additional resources
* For more information about installing Minishift, see the [Minishift documentation](https://docs.okd.io/latest/minishift/getting-started/installing.html).
* For more information about installing libvirt and qemu-kvm, see the documentation [here](https://docs.okd.io/latest/minishift/getting-started/setting-up-virtualization-environment.html).
