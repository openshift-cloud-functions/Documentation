# OpenShift Cloud Functions (OCF)
Developer Preview 0.2.0
------

> **IMPORTANT:** The functionality introduced by OCF is developer preview only. Red Hat supported is not provided, and OCF should not be used in a production environment.

## Prerequisites

> **IMPORTANT:** You will need cluster administrator privileges to install and use OCF.

### Supported platform versions

| Platform        | Supported versions           |
| ------------- |:-------------:|
| Minishift      | 1.25 or better |
| OKD   | 3.11   |
| OpenShift Container Platform     | 3.11      |


## Installing OCF

### Installing dependencies

You must install the following dependencies before installing OCF on Minishift.

- [Minishift](https://docs.okd.io/latest/minishift/getting-started/installing.html)
- [Operator Lifecycle Manager (OLM)](https://github.com/operator-framework/operator-lifecycle-manager/blob/master/Documentation/install/install.md#install-the-latest-release-version-of-olm-for-okd)
- [OLM user interface](https://github.com/operator-framework/operator-lifecycle-manager#user-interface)

> **NOTE:** You will need to set the correct hardware configuration for the virtual machine before starting Minishift. You can do this by using the commands
> `minishift config set memory 8GB (2)`
> `minishift config set cpus 4 (3)`
> For more information, see the [Minishift configuration documentation](https://docs.okd.io/latest/minishift/command-ref/minishift_config.html).

### Installing OCF on Minishift

1. Start Minishift.

   `minishift start`  

2. Set the required environment variables.

   `eval $(minishift oc-env)`  
   `eval $(minishift docker-env)`  

3. Login as administrator.

   `oc login -u system:admin`  

4. Install the OCF `knative-operators CatalogSource`.

   `oc apply -f https://raw.githubusercontent.com/openshift-cloud-functions/knative-operators/master/knative-operators.catalogsource.yaml`  

## Accessing the OCF console (user interface)

1. Start up the [OLM user interface](https://github.com/operator-framework/operator-lifecycle-manager#user-interface).

2. Check your IP address by typing `minishift ip` in the terminal. Use this IP address with port 9000 appended to access the console from your web browser.

   `http://127.0.0.1:9000`


## Installing Knative Operators on Minishift using OLM

### Creating required Knative namespaces (projects)

Namespaces are created automatically when using the `oc new-project` command.

To use the Knative operators, you must create the following projects.

| Operator name | Required namespace | Command to create |
| ------------- | ------------- | ------------- |
| knative-build   | knative-build  | `oc new-project knative-build` |
| knative-serving   | knative-serving   | `oc new-project knative-serving`  |
| knative-eventing   | knative-eventing  | `oc new-project knative-eventing`  |


### Creating Subscriptions

On accessing the console, you will see **Operators** as a tab in the left panel.
You can create Subscriptions for the available Operators by accessing the **Subscriptions** tab under **Operators**.
![Subscriptions in the left panel](images/subs.png "Logo Title Text 1")

> **IMPORTANT:** Before you create Subscriptions, you must first select the correct project (namespace) from the **Project** drop-down menu.

#### Knative build

1. Select the `knative-build` project from the drop-down menu.
2. To create a new Subscription, select the **Create Subscription** button in the **Package Manifests** tab.
3. Select `knative-build` from the list of Operators, then select **Create Subscription**.

   ![Knative operators](images/ops-for-subs.png "Logo Title Text 1")  

3. You will be able to see and edit the configuration of the Subscription being created before you finalize this.

   For more information about Subscription configurations, see the [OLM documentation](https://github.com/operator-framework/operator-lifecycle-manager#discovery-catalogs-and-automated-upgrades).  

   ![Subscription configuration](images/sub-config-build.png "Logo Title Text 1")  

4. The Subscription will now be created. Once this process is complete, you will see the Subscription information in the **Subscriptions** tab.

   ![Subscription creation complete](images/confirmation-build.png "Subscriptions tab")  

5. You can verify the setup by checking that the pods are running by going to the **Workloads** > **Pods** tab.

   ![Pods running verification](images/pods-build.png "Pods tab")  

#### Knative serving

1. Select the `knative-serving` project from the drop-down menu.
2. To create a new Subscription, select the **Create Subscription** button in the **Package Manifests** tab.
3. Select `knative-serving` from the list of Operators, then select **Create Subscription**.

   ![Knative operators](images/ops-for-subs.png "Logo Title Text 1")  

3. You will be able to see and edit the configuration of the Subscription being created before you finalize this.

   For more information about Subscription configurations, see the [OLM documentation](https://github.com/operator-framework/operator-lifecycle-manager#discovery-catalogs-and-automated-upgrades).  

   ![Subscription configuration](images/sub-config-serving.png "Logo Title Text 1")  

4. The Subscription will now be created. Once this process is complete, you will see the Subscription information in the **Subscriptions** tab.

   ![Subscription creation complete](images/confirmation-serving.png "Subscriptions tab")  

5. You can verify the setup by checking that the pods are running by going to the **Workloads** > **Pods** tab.

   ![Pods running verification](images/pods-serving.png "Pods tab")  

#### Knative eventing

1. Select the `knative-eventing` project from the drop-down menu.
2. To create a new Subscription, select the **Create Subscription** button in the **Package Manifests** tab.
3. Select `knative-eventing` from the list of Operators, then select **Create Subscription**.

   ![Knative operators](images/ops-for-subs.png "Logo Title Text 1")  
   
3. You will be able to see and edit the configuration of the Subscription being created before you finalize this.

   For more information about Subscription configurations, see the [OLM documentation](https://github.com/operator-framework/operator-lifecycle-manager#discovery-catalogs-and-automated-upgrades).  

   ![Subscription configuration](images/sub-config-eventing.png "Logo Title Text 1")  

4. The Subscription will now be created. Once this process is complete, you will see the Subscription information in the **Subscriptions** tab.

   ![Subscription creation complete](images/confirmation-eventing.png "Subscriptions tab")  

5. You can verify the setup by checking that the pods are running by going to the **Workloads** > **Pods** tab.

   ![Pods running verification](images/pods-eventing.png "Pods tab")  
