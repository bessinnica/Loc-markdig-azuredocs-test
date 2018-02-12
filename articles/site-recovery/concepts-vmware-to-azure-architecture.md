---
title: VMware to Azure replication architecture in Azure Site Recovery | Microsoft Docs
description: This article provides an overview of components and architecture used when replicating on-premises VMware VMs to Azure with the Azure Site Recovery service
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2018
ms.author: raynew
---

# VMware to Azure replication architecture

This article describes the architecture and processes used when you replicate, fail over, and recover VMware virtual machines (VMs) between an on-premises VMware site and Azure, using the [Azure Site Recovery](site-recovery-overview.md) service.


## Architectural components

The following table and graphic provide a high-level view of the components used for VMware replication to Azure.  

**Component** | **Requirement** | **Details**
--- | --- | ---
**Azure** | An Azure subscription, Azure storage account, and Azure network. | Replicated data from on-premises VMs is stored in the storage account. Azure VMs are created with the replicated data when you run a fail over from on-premises to Azure. The Azure VMs connect to the Azure virtual network when they're created.
**Configuration server machine** | A single on-premises machine. We recommend you run it as a VMware VM that can deployed from a downloaded OVF template.<br/><br/> The machine runs all on-premises Site Recovery components, including the configuration server, process server, and master target server. | **Configuration server**: Coordinates communications between on-premises and Azure, and manages data replication.<br/><br/> **Process server**:  Installed by default on the configuration server. It receives replication data, optimizes it with caching, compression, and encryption, and sends it to Azure storage. The process server also installs the Mobility service on VMs you want to replicate, and performs automatic discovery of on-premises machines. As your deployment grows, you can add additional, separate process servers to handle larger volumes of replication traffic.<br/><br/>  **Master target server**: Installed by default on the configuration server. It handles replication data during failback from Azure. For large deployments, you can add an additional, separate master target server for failback.
**VMware servers** | VMware VMs are hosted on on-premises vSphere ESXi servers. We recommend a vCenter server to manage the hosts. | During Site Recovery deployment, you add VMware servers to the Recovery Services vault.
**Replicated machines** | The Mobility service is installed on each VMware VM that you replicate. | We recommend you allow automatic installation from the process server. Alternatively you can install the service manually, or use an automated deployment method such as System Center Configuration Manager.

**VMware to Azure architecture**

![Components](./media/concepts-vmware-to-azure-architecture/arch-enhanced.png)

## Replication process

1.	You prepare Azure resources, and on-premises components.
2.	In the Recovery Services vault, you specify source replication settings. As part of this process, you set up the on-premises configuration server. To deploy this server as a VMware VM, you download a prepared OVF template, and import it to VMware to create the VM.
3. You specify target replication settings, create a replication policy, and enable replication for your VMware VMs.
4.	Machines replicate in accordance with the replication policy, and an initial copy of the VM data is replicated to Azure storage.
5.	After initial replication finishes, replication of delta changes to Azure begins. Tracked changes for a machine are held in a .hrl file.
    - Machines communicate with the configuration server on port HTTPS 443 inbound, for replication management.
    - Machines send replication data to the process server on port HTTPS 9443 inbound (can be modified).
    - The configuration server orchestrates replication management with Azure over port HTTPS 443 outbound.
    - The process server receives data from source machines, optimizes and encrypts it, and sends it to Azure storage over port 443 outbound.
    - If you enable multi-VM consistency, machines in the replication group communicate with each other over port 20004. Multi-VM is used if you group multiple machines into replication groups that share crash-consistent and app-consistent recovery points when they fail over. This is useful if machines are running the same workload and need to be consistent.
6.	Traffic replicates to Azure storage public endpoints, over the internet. Alternately, you can use zure ExpressRoute [public peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering). Replicating traffic over a site-to-site VPN from an on-premises site to Azure isn't supported.


**VMware to Azure replication process**

![Replication process](./media/concepts-vmware-to-azure-architecture/v2a-architecture-henry.png)

## Failover and failback process

After replication is set up and you've run a disaster recovery drill (test failover) to check everything's working as expected, you can run failover and failback as you need to.

1. You can fail over a single machine, or create recovery plans, to fail over multiple VMs.
2. When you run a failover, Azure VMs are created from replicated data in Azure storage.
3. After triggering the initial failover, you commit it to start accessing the workload from the Azure VM.

When your primary on-premises site is available again, you can fail back.
1. You need to set up a failback infrastructure, including:
    - **Temporary process server in Azure**: To fail back from Azure, you set up an Azure VM to act as a process server, to handle replication from Azure. You can delete this VM after failback finishes.
    - **VPN connection**: To fail back, you need a VPN connection (or Azure ExpressRoute) from the Azure network to the on-premises site.
    - **Separate master target server**: By default, the master target server that was installed with the configuration server, on the on-premises VMware VM, handles failback. However, if you need to fail back large volumes of traffic, you should set up a separate on-premises master target server for this purpose.
    - **Failback policy**: To replicate back to your on-premises site, you need a failback policy. This was automatically created when you created your replication policy from on-premises to Azure.
2. After the components are in place, failback occurs in three stages:
    - Stage 1: Reprotect the Azure VMs so that they replicate from Azure back to the on-premises VMware VMs.
    - Stage 2: Run a failover to the on-premises site.
    - Stage 3: After workloads have failed back, you reenable replicate for the on-premises VMs.

**VMware failback from Azure**

![Failback](./media/concepts-vmware-to-azure-architecture/enhanced-failback.png)


## Next steps

Follow [this tutorial](tutorial-vmware-to-azure.md) to enable VMware to Azure replication.
