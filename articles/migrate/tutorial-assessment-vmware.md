---
title: Discover and assess on-premises VMware VMs for migration to Azure with Azure Migrate | Microsoft Docs
description: Describes how to discover and assess on-premises VMware VMs for migration to Azure, using the Azure Migrate service. 
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: raynew
---

# Discover and assess on-premises VMware VMs for migration to Azure

The [Azure Migrate](migrate-overview.md) services assesses on-premises workloads for migration to Azure.

In this tutorial, you learn how to:

> [!div class="checklist"]
> * Create an Azure Migrate project.
> * Set up an on-premises collector virtual machine (VM), to discover on-premises VMware VMs for assessment.
> * Group VMs and create an assessment.


If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/pricing/free-trial/) before you begin.


## Prerequisites

- **VMware**: The VMs that you plan to migrate must be managed by a vCenter Server running version 5.5, 6.0, or 6.5. Additionally, you need one ESXi host running version 5.0 or higher to deploy the collector VM. 
 
> [!NOTE]
> Support for Hyper-V is in our roadmap and will be enabled soon. 

- **vCenter Server account**: You need a read-only account to access the vCenter Server. Azure Migrate uses this account to discover the on-premises VMs.
- **Permissions**: On the vCenter Server, you need permissions to create a VM by importing a file in .OVA format. 
- **Statistics settings**: The statistics settings for the vCenter Server should be set to level 3 before you start deployment. If lower than level 3, assessment will work, but performance data for storage and network isn't collected. The size recommendations in this case will be done based on performance data for CPU and memory and configuration data for disk and network adapters. 

## Log in to the Azure portal
Log in to the [Azure portal](https://portal.azure.com).

## Create a project

1. In the Azure portal, click **Create a resource**.
2. Search for **Azure Migrate**, and select the service **Azure Migrate (preview)** in the search results. Then click **Create**.
3. Specify a project name, and the Azure subscription for the project.
4. Create a new resource group.
5. Specify the location in which to create the project, then click **Create**. You can only create an Azure Migrate project in the West Central US region for this preview. However, you can still plan your migration for any target Azure location. The location specified for the project is only used to store the metadata gathered from on-premises VMs. 

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)
    


## Download the collector appliance

Azure Migrate creates an on-premises VM known as the collector appliance. This VM discovers on-premises VMware VMs, and sends metadata about them to the Azure Migrate service. To set up the collector appliance, you download an .OVA file, and import it to the on-premises vCenter server to create the VM.

1. In the Azure Migrate project, click **Getting Started** > **Discover & Assess** > **Discover Machines**.
2. In **Discover machines**, click **Download**, to download the .OVA file.
3. In **Copy project credentials**, copy the project ID and key. You need these when you configure the collector.

    ![Download .ova file](./media/tutorial-assessment-vmware/download-ova.png)

### Verify the collector appliance

Check that the .OVA file is secure, before you deploy it.

1. On the machine to which you downloaded the file, open an administrator command window.
2. Run the following command to generate the hash for the OVA:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Example usage: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. The generated hash should match these settings.
    
    For OVA version 1.0.8.49
    **Algorithm** | **Hash value**
    --- | ---
    MD5 | cefd96394198b92870d650c975dbf3b8 
    SHA1 | 4367a1801cf79104b8cd801e4d17b70596481d6f
    SHA256 | fda59f076f1d7bd3ebf53c53d1691cc140c7ed54261d0dc4ed0b14d7efef0ed9

    For the OVA version 1.0.8.40:

    **Algorithm** | **Hash value**
    --- | ---
    MD5 | afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## Create the collector VM

Import the downloaded file to the vCenter Server.

1. In the vSphere Client console, click **File** > **Deploy OVF Template**.

    ![Deploy OVF](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. In the Deploy OVF Template Wizard > **Source**, specify the location of the .ova file.
3. In **Name** and **Location**, specify a friendly name for the collector VM, and the inventory object in which the VM
will be hosted.
5. In **Host/Cluster**, specify the host or cluster on which the collector VM will run.
7. In storage, specify the storage destination for the collector VM.
8. In **Disk Format**, specify the disk type and size.
9. In **Network Mapping**, specify the network to which the collector VM will connect. The network needs internet connectivity, to send metadata to Azure. 
10. Review and confirm the settings, then click **Finish**.

## Run the collector to discover VMs

1. In the vSphere Client console, right-click the VM > **Open Console**.
2. Provide the language, time zone and password preferences for the appliance.
3. On the desktop, click the **Run collector** shortcut.
4. In the Azure Migrate Collector, open **Set up prerequisites**.
    - Accept the license terms, and read the third-party information.
    - The collector checks that the VM has internet access.
    - If the VM accesses the internet via a proxy, click **Proxy settings**, and specify the proxy address and listening port. Specify credentials if the proxy needs authentication.

    > [!NOTE]
    > The proxy address needs to be entered in the form http://ProxyIPAddress or http://ProxyFQDN. Only HTTP proxy is supported.

    - The collector checks that the collectorservice is running. The service is installed by default on the collector VM.
    - Download and install the VMware PowerCLI.

5. In **Specify vCenter Server details**, do the following:
    - Specify the name (FQDN) or IP address of the vCenter server.
    - In **User name** and **Password**, specify the read-only account credentials that the collector will use to discover VMs on the vCenter server.
    - In **Collection scope**, select a scope for VM discovery. The collector can only discover VMs within the specified scope. Scope can be set to a specific folder, datacenter, or cluster. It shouldn't contain more than 1000 VMs. 

6. In **Specify migration project**, specify the Azure Migrate project ID and key that you copied from the portal. If didn't copy them, open the Azure portal from the collector VM. In the project **Overview** page, click **Discover Machines**, and copy the values.  
7. In **View collection progress**, monitor discovery, and check that metadata collected from the VMs is in scope. The collector provides an approximate discovery time.

> [!NOTE]
> The collector only supports "English (United States)" as the operating system language and the collector interface language. Support for more languages is coming soon.


### Verify VMs in the portal

Discovery time depends on how many VMs you are discovering. Typically, for 100 VMs, after the collector finishes running it takes around an hour for discovery to finish. 

1. In the Migration Planner project, click **Manage** > **Machines**.
2. Check that the VMs you want to discover appear in the portal.


## Create and view an assessment

After VMs are discovered, you group them and create an assessment. 

1. In the project **Overview** page, click **+Create assessment**.
2. Click **View all** to review the assessment properties.
3. Create the group, and specify a group name.
4. Select the machines that you want to add to the group.
5. Click **Create Assessment**, to create the group and the assessment.
6. After the assessment is created, view it in **Overview** > **Dashboard**.
7. Click **Export assessment**, to download it as an Excel file.

### Sample assessment

Here's an example assessment report. It includes information about whether VMs are compatible for Azure, and estimated monthly costs. 

![Assessment report](./media/tutorial-assessment-vmware/assessment-report.png)

#### Azure readiness

This view shows the readiness status for each machine.

- For VMs that are ready, Azure Migrate recommends a VM size in Azure.
- For VMs that aren't ready, Azure Migrate explains why, and provides remediation steps.
- Azure Migrate suggests tools that you can use for the migration. If the machine is suitable for lift and shift migration, the Azure Site Recovery service is recommended. If it's a database machine, the Azure Database Migration Service is recommended.

  ![Assessment readiness](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### Monthly cost estimate

This view shows the total compute and storage cost of running the VMs in Azure along with the details for each machine. Cost estimates are calculated using the performance-based size recommendations for a machine and its disks, and the assessment properties. 

> [!NOTE]
> The cost estimation provided by Azure Migrate is for running the on-premises VMs as Azure Infrastructure as a service (IaaS) VMs. Azure Migrate does not consider any Platform as a service (PaaS) or Software as a service (SaaS) costs. 

Estimated monthly costs for compute and storage are aggregated for all VMs in the group. 

![Assessment VM cost](./media/tutorial-assessment-vmware/assessment-vm-cost.png) 

You can drill down to see details for a specific machine.

![Assessment VM cost](./media/tutorial-assessment-vmware/assessment-vm-drill.png) 

## Next steps

- [Learn](how-to-scale-assessment.md) how to discover and assess a large VMware environment.
- Learn how to create high-confidence assessment groups using [machine dependency mapping](how-to-create-group-machine-dependencies.md)
- [Learn more](concepts-assessment-calculation.md) about how assessments are calculated.
