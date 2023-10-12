# IBM Z and Cloud Modernization Stack

# IBM MQ Resources Operator Collection in Red Hat® OpenShift®

By combining the power of IBM Z and the strength of the Red Hat® OpenShift® Container Platform, the [IBM Z and Cloud Modernization Stack](https://www.ibm.com/docs/en/cloud-paks/z-modernization-stack/2023.2) delivers a modern, managed as-a-service model to complement customer production workloads.

The [IBM® z/OS® Cloud Broker](https://www.ibm.com/docs/en/cloud-paks/z-modernization-stack/2023.2?topic=automating-zos-resources-provisioning-zos-cloud-broker&_gl=1*10o436q*_ga*NTY4ODk4ODI5LjE2ODczMzY4NzY.*_ga_FYECCCS21D*MTY4OTA3Nzc3Ni40OS4xLjE2ODkwNzkyNjMuMC4wLjA.) is included in the stack to allow:

- z/OS infrastructure to be integrated with hybrid multi-cloud environments
- z/OS resources and services to be accessed from the Red Hat® OpenShift Container Platform
- z/OS resources to be exposed in the Red Hat® OpenShift Container Platform Catalog
- Developers to interact with z/OS resources without needing deep mainframe expertise
- Resources to be managed, in a stateful way, on **ZosEndpoints** using operators and sub-operators. See [Modernizing and Innovating on IBM Z Just Got a Lot Easier]( https://freemanlatrell.github.io/blog/post/operator-collection-blog/) for further details.

The [IBM Operator Collection SDK](https://github.com/IBM/operator-collection-sdk/tree/main) provides the resources and tools needed to develop Operator Collections against the IBM® z/OS® Cloud Broker.

The [IBM MQ Resources Operator Collection (zos_mq_operator)](https://github.com/IBM/zos_mq_operator) sample is implemented as a sub-operator of the IBM z/OS Cloud Broker, and can be installed and invoked from the Red Hat® OpenShift Console. It includes a set of **Provided APIs** that can be used to manage IBM MQ **Local** and **Alias** queues, and **Server Connection** channels on a ZosEndpoint. ZosEndpoints are a function of the z/OS Cloud Broker and allow a mapping to be created to a z/OS Logical Partition (LPAR) where an existing stand-alone IBM MQ for z/OS Queue Manager is running. If required, an instance of a z/OS Cloud Broker and instances of specific ZosEndpoints can be configured ahead of time by a Systems Programmer. This would allow end users to simply create resources on defined ZosEndpoints.

Some IBM MQ customers were asked if they had a requirement to provision IBM MQ for z/OS Queue Managers. The general view was that this was not currently a requirement since users already had existing queue managers defined. If you do not have an existing Queue Manager defined, you can stand-up one in a **Wazi Sandbox**. Refer to section **IBM Wazi Sandbox** below. 

The Provided APIs can be used to **create** the respective types of IBM MQ resources. Actions can be performed against created resources to **edit** or **delete** a given resource. However, the edit action allows you to alter the YAML form of the results of created resources and **does NOT** provide the equivalent function of the **IBM MQ ALTER command**.

Depending on your role, you may have access to the **Red Hat® OpenShift Administrator, Developer,** or **both console views**. And, depending on which console view you use, you will see different panels as shown below.

The intent here is that **users should not need to have deep skills in IBM z/OS or IBM MQ for z/OS**. Users need only supply a minimum set of attributes when creating resources. If required, the **IBM MQ Script Command (MQSC) 'LIKE' attribute** can be set to name a known, existing resource that acts like a **template** and from which specific MQ resource attribute values can be inherited. Depending on specific user needs, IBM MQ for z/OS Systems Programmers can create a set of such templates ahead of time. If a LIKE attribute value is not set, attributes will by default be inherited from the respective **SYSTEM.DEFAULT.*** or **SYSTEM.DEF.*** resource definition.

Essentially, once these basic types of resources are defined to your Queue Manager, your MQ client applications can connect to your Queue Manager and use messaging to communicate with other MQ applications.

To allow users to try out and get a feel for this function, only a few resource types are initially supported. If you have specific requirements to provision other types of IBM MQ for z/OS resources in this way, please submit an idea under [MQ, MQ Advanced & MQ Appliance](https://integration-development.ideas.ibm.com/?project=MESNS) in the **IBM Ideas Portal**, so that your idea can be assessed and prioritized against other work.

## IBM MQ Resources Operator Collection Content

The collection consists of a set of sample **Variable files** and **Ansible Playbooks**. The Variable files define default values and the Ansible playbooks perform the create or delete action for the respective MQ resource. The playbooks build MQSC to DEFINE or DELETE resources, and issue the commands to a ZosEndpoint that maps to a z/OS Subsystem on which the target IBM MQ for z/OS Queue Manager is running. Users do not need to be too concerned about the playbook files as they are invoked during resource deployment. However, users will need to set values in some of the following **variables** files:

<img src="./docs/IBM MQ Resources Collection Variables.gif"  width="200" height="120">

<br />It is important to note that the files and playbooks are provided as **samples** and, like some other MQ samples, they are currently not officially supported by the MQ Service Team. However, effort will be made to respond to or address, in good time, issues that users may raise.

## Security Considerations when using IBM z/OS Cloud Broker and Operator Collections

1. You can authorize access to IBM® z/OS® Cloud Broker resources by using the Kubernetes and Red Hat® OpenShift® Container Platform role-based access control (RBAC) facilities.

2. An SSH key pair (a private key and a public key ending in .pub) is recommended when you import an operator collection into IBM® z/OS® Cloud Broker and map the collection to ZosEndpoints. Operator collections and their generated suboperators use the SSH key pair and z/OS user credentials to run Ansible automation against specified ZosEndpoints and authenticate connections to them.

3. See [Security considerations for installing IBM z/OS Cloud Broker](https://www.ibm.com/docs/en/cloud-paks/z-modernization-stack/2023.2?topic=broker-security-considerations-zos-cloud) for further information on points 1. and 2. above.

## Security Considerations for IBM MQ for z/OS Commands and Resources

You will of course need to configure **Command and Resource** level security for IBM MQ for z/OS to control which commands users are permitted to issue against which IBM MQ for z/OS resources. See [Setting up security on z/OS](https://www.ibm.com/docs/en/ibm-mq/9.3?topic=security-setting-up-zos) for further information.

## Set-up steps for System Administrators

Depending on what you already have installed, you will need to perform some or all of the steps listed below.

Perform the following steps to install the IBM z/OS Cloud Broker, the IBM Operator Collection SDK and the IBM MQ Resources Operator Collection. Perform all Red Hat® OpenShift functions using the **OpenShift Administrator** panels.

### 1. Plan to install and configure IBM z/OS Cloud Broker into Red Hat® OpenShift

See [Planning to Install z/OS Cloud Broker](https://www.ibm.com/docs/en/cloud-paks/z-modernization-stack/2023.2?topic=broker-planning-install-zos-cloud) for further information. This section includes a topic on Security Considerations for z/OS Cloud Broker.

### 2. Install IBM z/OS Cloud Broker into Red Hat® OpenShift

See [Installing IBM z/OS Cloud Broker](https://www.ibm.com/docs/en/cloud-paks/z-modernization-stack/2023.2?topic=broker-installing-zos-cloud) for further information.

Once you have installed IBM z/OS Cloud Broker in to your project namespace, select **Installed Operators** in the Red Hat® OpenShift console and you should see a screen that looks similar to:

<img src="./docs/RedHat OpenShift Installed Operators.gif"  width="1028" height="520">

Select the **IBM z/OS Cloud Broker** and you should see a screen similar to the following:
<img src="./docs/IBM zOS Cloud Broker.gif"  width="1028" height="520">

### 3. Define a ZosEndpoint in IBM z/OS Cloud Broker

ZosEndpoints allow a mapping to be created to a z/OS Logical Partition (LPAR) where the IBM MQ for z/OS Queue Manager is running. 

The easiest way to define a ZosEndpoint is by selecting **Create Instance** under **z/OS Endpoint** from within the z/OS Cloud Broker (shown on the second screen displayed in step 2. above) and entering fields as shown on the screen below:

<img src="./docs/IBM zOS Cloud Broker Create ZosEndpoint.gif"  width="1028" height="520">

**Note:** The **port number** in the **ZosEndpoint** definition needs to be the number of your **SSH port** (on some systems, the default SSH port is 22) and **not the number of the port your IBM MQ for z/OS TCP/IP Listener is listening on**.

Other ways to define a ZosEndpoint are stated below.

### 4. Create an instance of z/OS Cloud Broker

You need to create an instance of the z/OS Cloud Broker so that you can subsequently log in to the broker user interface and install an Operator Collection.

The easiest way to create an instance of the z/OS Cloud Broker is by selecting **Create Instance** under **z/OS Cloud Broker** from within the z/OS Cloud Broker (shown on the second screen displayed in step 2. above)

See [Creating a z/OS Cloud Broker instance](https://www.ibm.com/docs/en/cloud-paks/z-modernization-stack/2023.2?topic=broker-creating-zos-cloud-instance) for further information.

### 5. Log in to the z/OS Cloud Broker instance

See [Logging in to z/OS Cloud Broker](https://www.ibm.com/docs/en/cloud-paks/z-modernization-stack/2023.2?topic=interface-logging-in-zos-cloud-broker) for further information.

### 6. Install the IBM MQ Resources Operator Collection into a Red Hat® OpenShift Cluster environment

Operator Collections can be installed by importing:

- from an Ansible Galaxy operator collection catalog,
- from a specified URL,
- a manually uploaded collection.

See [Importing an operator collection](https://www.ibm.com/docs/en/cloud-paks/z-modernization-stack/2023.2?topic=interface-importing-operator-collection) for further details on these three methods of installation. Notice that this sections shows another way of defining a ZosEndpoint.

Operator Collections can also be installed using the **The IBM Operator Collection SDK** and **The IBM Operator Collection SDK Extension for VS Code**. See below for further details about these.

The [IBM MQ Resources Operator Collection (zos_mq_operator)](https://github.com/IBM/zos_mq_operator) can be installed by manually uploading the collection. You can either clone the [IBM MQ Resources Operator Collection (zos_mq_operator)](https://github.com/IBM/zos_mq_operator) repository to access the IBM MQ Operator Collection *.tar.gz file, or download the IBM MQ Operator Collection *.tar.gz file from the repository. Save the IBM MQ Operator Collection *.tar.gz file to a location on your local machine and manually upload the collection as per the instructions for [Importing an operator collection](https://www.ibm.com/docs/en/cloud-paks/z-modernization-stack/2023.2?topic=interface-importing-operator-collection). It is recommended that you use an SSH key pair (a private key and a public key ending in .pub) during installation.

**Note:** The MQ Operator *.tar.gz file shipped with this collection was created by entering the following command against this collection:

&nbsp;&nbsp;&nbsp;&nbsp;**ansible-galaxy collection build --force**

## Uninstall steps for System Administrators

### Uninstalling IBM MQ Resources Operator Collection

1. Within z/OS Cloud Broker, select **Configure operator collections**.
2. In the Configure operator collections section, click the **zos-mq-operator** tile.
3. Click the **Delete** button and confirm your choice. Click **Delete** again.
4. The **zos-mq-operator** tile moves back to the **Imported** section. 
5. Click the **recycling bin icon** and confirm your choice. Click **Delete** again.

### Deleting instances of ZosEndpoint and the ZosCloudBroker

1. Selecting **Installed Operators**.
2. Selecting **IBM z/OS Cloud Broker**.
3. Selecting **z/OS Endpoint, z/OS Cloud Broker or All Instances**.
4. Selecting an **action** against an instance by clicking the **three dots** on the right.

<img src="./docs/IBM zOS Cloud Broker Delete ZosEndpoint.gif"  width="1028" height="520">

### Uninstalling z/OS Cloud Broker

See [Uninstalling z/OS Cloud Broker](https://www.ibm.com/docs/en/cloud-paks/z-modernization-stack/2023.2?topic=broker-uninstalling-zos-cloud).

## Where to find the IBM MQ Resources Operator once it has been installed

### 1. Administrator View

The z/OS Cloud Broker publishes the IBM MQ Resources Operator in the **OperatorHub**, which is only accessible to **Administrators**. 

Once you have installed **z/OS Cloud Broker** and **IBM MQ Resources Operator Collection**, the **Administrator->Operators->OperatorHub** should contain a tile for the IBM MQ Resources Operator. Filter items by **mq**:

<img src="./docs/IBM MQ Resources Collection OperatorHub.gif"  width="1028" height="520">

<br />The **Administrator->Operators->Installed Operators** view gives access to the **Provided APIs** and should look similar to this:

<img src="./docs/RedHat OpenShift Installed Operators Provided APIs.gif"  width="1028" height="520">

If you click on **IBM Z and Cloud Modernization Stack - MQ Resources Operator**, you will see a screen similar to the following:

<img src="./docs/IBM MQ Resources Operator Collection.gif"  width="1028" height="520">

### 2. Developer View

The z/OS Cloud Broker also installs the IBM MQ Resources Operator into a namespace to allow Developers access to the Provided APIs, which appear as tiles in the Developer Catalog (**Developer->+Add->Developer Catalog->Operator Backed** and filter items by **mq**):

<img src="./docs/IBM MQ Resources Collection DevCatalog.gif"  width="1028" height="520">

## Running the Provided APIs to Create or Delete IBM MQ resources on a target z/OS Queue Manager

1. Depending on your role, you can either use the **Administrator** or **Developer** Red Hat® OpenShift console views (shown above) to access the Provided APIs. 

2. Once you have selected an API, you can click on **Create** to create an instance of the resource.

   For example, to create a local queue, select **Create** (or **Create LocalQueue**):

<img src="./docs/IBM MQ Resources Collection Local Queue Create.gif"  width="1028" height="520">

3. You can then **enter the properties** for the resource. Properties marked with an **asterisk (*)** are required, and valid values must be entered for these. The properties view is identical in both the Administrator and Developer views. 

   For example, to create a local queue named TEST.LOCAL.QUEUE, enter the queue properties as shown below and click **Create**:

<img src="./docs/IBM MQ Resources Collection Create Local Queue.gif"  width="1028" height="520">

4. To delete a created instance of a resource, select the **Delete Action** from the drop-down **Actions** menu on the top right of the screen and click **Delete** to confirm deletion. The delete action issues the **MQSC DELETE command with the PURGE** option so any messages on the queue will be deleted. Once you have successfully deleted a resource, it will no longer appear in the list of created resources.

   For example, to delete local queue TEST.LOCAL.QUEUE, select the **Delete** action and click **Delete**: 

<img src="./docs/IBM MQ Resources Collection Delete Local Queue.gif"  width="1028" height="520">

There are also other ways to locate resources in Red Hat® OpenShift:

a. Selecting **ibm-zos-mq-operator-operator** from the Topology view and clicking on a **CSV** link on the right:

<img src="./docs/IBM MQ Resources Collection Topology View.gif"  width="1028" height="520">

Then select ** Manage MQ Local Queues** to see a list of local queues from where an action can be performed by selecting the **three dots** on the right:

<img src="./docs/IBM MQ Resources Collection Delete Local Queue 2.gif"  width="1028" height="520">


b. Selecting **Search** followed by **LocalQueue** from the **Resources** dropdown to see a list of all local queues and then selecting **Delete LocalQueue** on the right:

<img src="./docs/IBM MQ Resources Collection Delete Local Queue 3.gif"  width="1028" height="520">

## Observing Task Execution

If required, you can observe **task execution** for the Create or Delete actions by viewing the list of **Conditions** (as they appear during resource creation or deletion, or once a resource has been created) at the bottom of the details view for a resource. Tasks are executed in the order in which they are defined in the Ansible Playbooks for the Operator Collection. 

For example, the tasks executed to create a local queue are:

<img src="./docs/IBM MQ Resources Collection Create Local Queue Conds.gif"  width="1028" height="520">

<br />**Note** You may see conditions like:
<br /><br />&nbsp;&nbsp;&nbsp;&nbsp;Pending. 20 Sept 2023, 11:41. RequirementsUnknown&nbsp;&nbsp;&nbsp;requirements not yet checked
<br />&nbsp;&nbsp;&nbsp;&nbsp;Pending. 20 Sept 2023, 11:41. RequirementsNotMet&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;one or more requirements couldn't be found
<br /><br />These conditions are created by the **Red Hat® OpenShift Operator LifeCycle Manager (OLM)**. They are beyond the control of the IBM MQ Operator Collection and can generally be ignored.

## Objects in Red Hat® OpenShift verses resources in IBM MQ

When you create an IBM MQ for z/OS resource from the Red Hat® OpenShift Console, an object is created to allow you to manage the resource in Red Hat® OpenShift. It is important that you perform actions against the object in Red Hat® OpenShift, and not against the resource on the target IBM MQ for z/OS Queue Manager. Otherwise, you could end up deleting a resource from the Queue Manager but not the object from Red Hat® OpenShift. Resulting in out of step resources and hence additional administrative overhead.

## Troubleshooting

Refer to the [z/OS Cloud Broker Troubleshooting section](https://www.ibm.com/docs/en/cloud-paks/z-modernization-stack/2023.2?topic=broker-troubleshooting) for further information.

## The IBM Operator Collection SDK

The IBM Operator Collection SDK is a comprehensive set of resources and tools that are designed for the development of operator collections against the z/OS Cloud Broker. See [Developing operator collections with the IBM Operator Collection SDK](https://www.ibm.com/docs/en/cloud-paks/z-modernization-stack/2023.2?topic=broker-developing-operator-collections-operator-collection-sdk) for further information.

The above section includes a link to the [IBM Operator Collection SDK](https://github.com/IBM/operator-collection-sdk/tree/main) which in turn links to the [Operator Collection Tutorial](https://github.com/IBM/operator-collection-sdk/blob/main/docs/tutorial.md). The tutorial lists the prerequisites for z/OS Cloud Broker and describes the steps required to install an Operator Collection and create an Operator instance. The tutorial shows an example racf-operator.

## The IBM Operator Collection SDK Extension for VS Code

[The IBM Operator Collection SDK Extension for VS Code](https://github.com/IBM/operator-collection-sdk-vscode-extension) simplifies the Operator Collection development experience by allowing you to manage the deployment of your operator in OpenShift, and the ability to debug directly from your VS Code editor.

## IBM Wazi Sandbox

IBM Wazi and IBM Wazi Sandbox are part of the [IBM Z and Cloud Modernization Stack](https://www.ibm.com/docs/en/cloud-paks/z-modernization-stack/2023.2?topic=overview) delivery. IBM Wazi Sandbox allows users to create sandbox environments on demand and experiment with cloud native tools and IBM products.

IBM Wazi Sandbox uses the same Application Developer Controlled Distribution (ADCD) as used for creating an [IBM Z Development and Test Environment](https://www.ibm.com/docs/en/zdt/14.1?topic=reference-adcd-zos-v2r5-december-edition-2022). IBM MQ for z/OS CD V9.3.0 is included with this distribution so you can install this. Address spaces **CSQ9MSTR** and **CSQ9CHIN**, for stand-alone **Queue Manager CSQ9** and the corresponding **Channel Initiator**, are preconfigured and can be started using a command prefix of **%CSQ9** and entering the following commands:

&nbsp;&nbsp;&nbsp;&nbsp;**%CSQ9 START QMGR**

&nbsp;&nbsp;&nbsp;&nbsp;**%CSQ9 START CHINIT**

## Copyright

© Copyright IBM Corporation 2023

## License
Licensed under [Apache License](https://opensource.org/licenses/Apache-2.0).
