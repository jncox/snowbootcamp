.. _snow_preparingenv:

-------------------------------
Preparing Calm Blueprint
-------------------------------

Before we can use ServiceNow to provide self-service access to users, you must first define a Nutanix Calm project and an application Blueprint.

Creating A Calm Project
+++++++++++++++++++++++

Nutanix Calm allows you to build, provision, and manage your applications across both private (AHV, ESXi) and public cloud (AWS, Azure, GCP) infrastructure.

In order for non-infrastructure administrators to access Calm, allowing them to create or manage applications, users or groups must first be assigned to a **Project**, which acts as a logical container to define user roles, infrastructure resources, and resource quotas. Projects define a of set users with a common set of requirements or a common structure and function, such as a team of developers collaborating on the Fiesta application.

#. In **Prism Central**, select :fa:`bars` **> Services > Calm**.

#. Select **Projects** from the left-hand toolbar and click **+ Create Project**.

   .. figure:: images/12.png

#. Specify **USER**\ *##*\ **-Project** (ex. USER01-Project) as your **Project Name**.

#. Click **Create**.

#. Under **Users, Groups, & Roles**, click **Add Users**.

   .. figure:: images/24.png

#. Click **+ Add User** and fill out the following fields:

   - **Name** - operator\ *##*\ @ntnxlab.local (ex. operator01@ntnxlab.local)
   - **Role** - Operator

   .. figure:: images/25.png

#. Click **Save Users and Project**.

#. Under **Accounts**, click **Add Accounts**.

#. Click **Select Account > NTNX_LOCAL_AZ** to configure your Nutanix cluster.

#. Click **Add/Edit Clusters and Subnets**.

   .. figure:: images/26.png

#. Select your cluster from the dropdown menu and select your **Secondary** virtual network. Only selected subnets will appear available for assignment to Calm workloads.

   .. figure:: images/27.png

#. Click **Confirm**.

   .. note::

      If multiple networks are selected, you can select the **Default** network by clicking the :fa:`star`.

#. Leave **Quotas** blank and click **Save Accounts and Project**.

   This will redirect you back to the **Project Overview** page. You do **NOT** need to configure anything under **Environments** to proceed. This additional section is for defining default values used when provisioning Blueprints from the Calm Marketplace.

Uploading A Calm Blueprint
++++++++++++++++++++++++++

A Blueprint is the framework for every application that you model by using Nutanix Calm. Blueprints are templates that describe all the steps that are required to provision, configure, and execute tasks on the services and applications that are created. A Blueprint also defines the lifecycle of an application and its underlying infrastructure, starting from the creation of the application to the actions that are carried out on a application (updating software, scaling out, etc.) until the termination of the application.

You can use Blueprints to model applications of various complexities; from simply provisioning a single virtual machine to provisioning and managing a multi-node, multi-tier application.

For the purposes of this exercise, you will upload an existing Blueprint of a single VM application deployment. Within the customer environment, this Blueprint could represent a pre-configured build tools envrionment for a developer.

#. `Download the Single VM CentOS Blueprint by right-clicking here and saving. <https://raw.githubusercontent.com/nutanixworkshops/snowbootcamp/master/plugins/CentOS%20VM.json>`_

#. From the left-hand toolbar in **Calm**, select **Blueprints**.

   .. figure:: images/16.png

#. Click **Upload Blueprint** and select the **CentOS VM.json** file downloaded in Step 1.

#. Update the **Blueprint Name** to include your **USER**\ *##* and select the Calm Project you created in the previous exercise.

   .. figure:: images/17.png

#. Click **Upload**.

   Before the Blueprint can be used, the networks, disk images, and credentials must be configured for your environment. Additionally, you will incorporate the categories associated with your data protection and network isolation policies.

#. Within your **CentOS VM** Blueprint, click **VM Details**.

#. Select the **Cloud** dropdown and observe that, in this environment, Nutanix AHV is the only option.

   While Calm provides the ability to define deployment requirements for multiple different cloud providers within a single Blueprint, one of the key advantages of Nutanix Clusters is being able to utilize a single configuration (Nutanix AHV) regardless of whether the app is being provisioned on-premises or in your elastic, public cloud hosted cluster.

#. Click **VM Configuration**.

   Here you'll see the specifications for the VM being provisioned. Observe that a Calm macro, or variable, is being used to customize the VM name by prepending the user's initials.

#. Click the **Runtime** icon for both **vCPUs** and **Memory** to allow for customization of these values at the time of launch.

   We will use this in a later exercise to allow a ServiceNow administrator to create multiple catalog offerings from the same Blueprint.

   .. figure:: images/18.png

#. Under **Disks > Disk (1) > Image** select **CentOS7.qcow2** to clone from the existing disk stored within the Prism Image Service.

   .. figure:: images/19.png

#. Under **NICs**, select **Secondary** with a **Dynamic** IP.

   If you had multiple clusters available as part of your project, this selection would control the Cluster on which the Blueprint would be provisioned. Configuring it as a runtime variable would allow a ServiceNow administrator additional flexibility in defining the self-service offering to provision the Blueprint to multiple different Nutanix clusters.

   .. figure:: images/21.png

#. Click **Advanced Options**.

#. Under **Credentials**, click **Add/Edit credentials**. Specify a password the **ROOT** credential (ex. *nutanix/4u*).

   This will be configurable for the user at runtime, but Calm requires a default value be provided before the Blueprint can be launched.

   .. figure:: images/22.png

#. Click **Done**.

#. Click **Save**.

   .. note::

      You should no longer see any red error alerts for the Blueprint, but warning alerts related to missing variable values are expected and will not impact the Blueprint.

Takeaways
+++++++++

- Calm Projects allow you to define pools of resources for specific users and groups.

- Calm Blueprints enable repeatable application deployments and lifecycle operations.
