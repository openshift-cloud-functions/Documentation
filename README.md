# OpenShift Cloud Functions (OCF)
Developer Preview 0.2.0
------

> **IMPORTANT:** The functionality introduced by OCF is developer preview only. Red Hat supported is not provided, and OCF should not be used in a production environment.

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [OpenShift Cloud Functions (OCF)](#openshift-cloud-functions-ocf)
	- [Prerequisites](#prerequisites)
		- [Supported platform versions](#supported-platform-versions)
		- [Installing dependencies](#installing-dependencies)
	- [Installing OCF on Minishift](#installing-ocf-on-minishift)
	- [Accessing the OCF console (user interface)](#accessing-the-ocf-console-user-interface)
	- [Installing Knative Operators on Minishift using OLM](#installing-knative-operators-on-minishift-using-olm)
		- [Knative build](#knative-build)
		- [Knative serving](#knative-serving)
		- [Knative eventing](#knative-eventing)

<!-- /TOC -->

## Prerequisites

> **IMPORTANT:** You will need cluster administrator privileges to install and use OCF.

### Supported platform versions

| Platform        | Supported versions           |
| ------------- |:-------------:|
| Minishift      | 1.25.0 or newer |

### Installing dependencies

You must install the following dependencies before installing OCF on Minishift.

- [Minishift](https://docs.okd.io/latest/minishift/getting-started/installing.html)
- [Operator Lifecycle Manager (OLM)](https://github.com/operator-framework/operator-lifecycle-manager/blob/master/Documentation/install/install.md#install-the-latest-release-version-of-olm-for-okd)
- [OLM user interface](https://github.com/operator-framework/operator-lifecycle-manager#user-interface)
- [Istio add-on for Minishift](https://github.com/minishift/minishift-addons/tree/master/add-ons/istio#istio-add-on)

> **NOTE:** You will need to set the correct configuration before starting Minishift. You can do this by using the commands
>
> `minishift profile set knative
> minishift config set openshift-version v3.11.0
> minishift config set memory 8GB
> minishift config set cpus 4
> minishift config set disk-size 50g
> minishift config set image-caching true
> minishift addons enable admin-user
> minishift addons enable anyuid`  
>
> For more information, see the [Minishift configuration documentation](https://docs.okd.io/latest/minishift/command-ref/minishift_config.html).

> **NOTE:** If this is not your first installation, and you wish to remove your Minishift profile and reinstall OCF, you can do this using the command
> `minishift profile delete knative --force`

## Installing OCF on Minishift

1. Start Minishift.

   `minishift start`  

2. Set the required environment variables.

   `eval $(minishift oc-env)`  
   `eval $(minishift docker-env)`  

3. Navigate to the `knative-operators` directory and run the script

   `prep-knative.sh`  

3. Login as administrator.

   `oc login -u system:admin`  

4. Install the OCF `knative-operators CatalogSource`.

   `oc apply -f https://raw.githubusercontent.com/openshift-cloud-functions/knative-operators/master/knative-operators.catalogsource.yaml`  

## Accessing the OCF console (user interface)

1. Open a new terminal window and set the required environment variables.

   `eval $(minishift oc-env)`  
   `eval $(minishift docker-env)`

2. Start up the [OLM user interface](https://github.com/operator-framework/operator-lifecycle-manager#user-interface).

3. Check your IP address by typing `minishift ip` in the terminal. Use this IP address with port 9000 appended to access the console from your web browser.

   `http://127.0.0.1:9000`


## Installing Knative Operators on Minishift using OLM

### Knative build

1. In the terminal, use the following command to create the `knative-build` project and namespace.

   `oc new-project knative-build`  

2. Ensure that you are in the `knative-build` namespace.

   `oc project knative-build`  

3. In the console, the `knative-build` project from the drop-down menu.

   ![Select knative-build project](images/build-proj.png)  

4. Create a new Subscription, by selecting the **Create Subscription** button for the `knative-build` Operator in the **Package Manifests** tab.

   ![Knative operators](images/ops-for-subs.png "Logo Title Text 1")  

5. You will be able to see and edit the configuration of the Subscription being created before you finalize this.

   For more information about Subscription configurations, see the [OLM documentation](https://github.com/operator-framework/operator-lifecycle-manager#discovery-catalogs-and-automated-upgrades).  

   ![Subscription configuration](images/sub-config-build.png "Logo Title Text 1")  

6. The Subscription will now be created. Once this process is complete, you will see the Subscription information in the **Subscriptions** tab.

7. You can verify the setup by checking that the pods are running by going to the **Workloads** > **Pods** tab.

### Knative serving

1. In the terminal, use the following command to create the `knative-serving` project and namespace.

   `oc new-project knative-serving`  

2. Ensure that you are in the `knative-serving` namespace.

   `oc project knative-serving`  

3. Select the `knative-serving` project from the drop-down menu.

   ![Select knative-serving project](images/serving-proj.png)   

4. Create a new Subscription, by selecting the **Create Subscription** button for the `knative-serving` Operator in the **Package Manifests** tab.

   ![Knative operators](images/ops-for-subs.png "Logo Title Text 1")  

5. You will be able to see and edit the configuration of the Subscription being created before you finalize this.

   For more information about Subscription configurations, see the [OLM documentation](https://github.com/operator-framework/operator-lifecycle-manager#discovery-catalogs-and-automated-upgrades).  

   ![Subscription configuration](images/sub-config-serving.png "Logo Title Text 1")  

6. he Subscription will now be created. Once this process is complete, you will see the Subscription information in the **Subscriptions** tab.

7. You can verify the setup by checking that the pods are running by going to the **Workloads** > **Pods** tab.

### Knative eventing

1. In the terminal, use the following command to create the `knative-eventing` project and namespace.

   `oc new-project knative-eventing`  

2. Ensure that you are in the `knative-eventing` namespace.

   `oc project knative-eventing`  

3. Select the `knative-eventing` project from the drop-down menu.

   ![Select knative-eventing project](images/eventing-proj.png)  

4. Create a new Subscription, by selecting the **Create Subscription** button for the `knative-eventing` Operator in the **Package Manifests** tab.

   ![Knative operators](images/ops-for-subs.png "Logo Title Text 1")  

5. You will be able to see and edit the configuration of the Subscription being created before you finalize this.

   For more information about Subscription configurations, see the [OLM documentation](https://github.com/operator-framework/operator-lifecycle-manager#discovery-catalogs-and-automated-upgrades).  

   ![Subscription configuration](images/sub-config-eventing.png "Logo Title Text 1")  

6. The Subscription will now be created. Once this process is complete, you will see the Subscription information in the **Subscriptions** tab.

7. You can verify the setup by checking that the pods are running by going to the **Workloads** > **Pods** tab.
