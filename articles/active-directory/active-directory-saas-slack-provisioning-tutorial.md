---
title: 'Tutorial: Configure Slack for automatic user provisioning with Azure Active Directory | Microsoft Docs'
description: Learn how to configure Azure Active Directory to automatically provision and de-provision user accounts to Slack.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: mtillman

ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser-msft
ms.reviewer: asmalser

---

# Tutorial: Configure Slack for automatic user provisioning


The objective of this tutorial is to show you the steps you need to perform in Slack and Azure AD to automatically provision and de-provision user accounts from Azure AD to Slack. 

## Prerequisites

The scenario outlined in this tutorial assumes that you already have the following items:

*   An Azure Active Active directory tenant
*   A Slack tenant with the [Plus plan](https://aadsyncfabric.slack.com/pricing) or better enabled 
*   A user account in Slack with Team Admin permissions 

Note: The Azure AD provisioning integration relies on the [Slack SCIM API](https://api.slack.com/scim) which is available to Slack teams on the Plus plan or better.

## Assigning users to Slack

Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps. In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD will be synchronized. 

Before configuring and enabling the provisioning service, you will need to decide what users and/or groups in Azure AD represent the users who need access to your Slack app. Once decided, you can assign these users to your Slack app by following the instructions here:

[Assign a user or group to an enterprise app](active-directory-coreapps-assign-user-azure-portal.md)

### Important tips for assigning users to Slack

*   It is recommended that a single Azure AD user be assigned to Slack to test the provisioning configuration. Additional users and/or groups may be assigned later.

*   When assigning a user to Slack, you must select the **User** or "Group" role in the assignment dialog. The "Default Access" role does not work for provisioning.


## Configuring user provisioning to Slack 

This section guides you through connecting your Azure AD to Slack's user account provisioning API, and configuring the provisioning service to create, update and disable assigned user accounts in Slack based on user and group assignment in Azure AD.

<strong>Tip:</strong> You may also choose to enabled SAML-based Single Sign-On for Slack, following the instructions provided in (Azure portal)[<https://portal.azure.com>]. Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.


### To configure automatic user account provisioning to Slack in Azure AD:


1)  In the [Azure portal](https://portal.azure.com), browse to the <strong>Azure Active Directory > Enterprise Apps > All applications</strong>  section.

2) If you have already configured Slack for single sign-on, search for your instance of Slack using the search field. Otherwise, select <strong>Add</strong> and search for <strong>Slack</strong> in the application gallery. Select Slack from the search results, and add it to your list of applications.

3)  Select your instance of Slack, then select the <strong>Provisioning</strong> tab.

4)  Set the <strong>Provisioning Mode</strong> to <strong>Automatic</strong>.

![Slack Provisioning](./media/active-directory-saas-slack-provisioning-tutorial/Slack1.PNG)

5)  Under the <strong>Admin Credentials</strong> section, click <strong>Authorize</strong>. This opens a Slack authorization dialog in a new browser window. 

6) In the new window, sign into Slack using your Team Admin account. in the resulting authorization dialog, select the Slack team that you want to enable provisioning for, and then select <strong>Authorize</strong>. Once completed, return to the Azure portal to complete the provisioning configuration.

![Authorization Dialog](./media/active-directory-saas-slack-provisioning-tutorial/Slack3.PNG)

7) In the Azure portal, click <strong>Test Connection</strong> to ensure Azure AD can connect to your Slack app. If the connection fails, ensure your Slack account has Team Admin permissions and try the "Authorize" step again.

8) Enter the email address of a person or group who should receive provisioning error notifications in the <strong>Notification Email</strong> field, and check the checkbox below.

9) Click <strong>Save</strong>. 

10) Under the Mappings section, select <strong>Synchronize Azure Active Directory Users to Slack</strong>.

11) In the <strong>Attribute Mappings</strong> section, review the user attributes that will be synchronized from Azure AD to Slack. Note that the attributes selected as <strong>Matching</strong> properties will be used to match the user accounts in Slack for update operations. Select the Save button to commit any changes.

12) To enable the Azure AD provisioning service for Slack, change the <strong>Provisioning Status</strong> to <strong>On</strong> in the <strong>Settings</strong> section

13) Click <strong>Save</strong>. 

This will start the initial synchronization of any users and/or groups assigned to Slack in the Users and Groups section. Note that the initial sync will take longer to perform than subsequent syncs, which occur approximately every 10 minutes as long as the service is running. You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Slack app.

## [Optional] Configuring group object provisioning to Slack 

Optionally, you can enable the provisioning of group objects from Azure AD to Slack. This is different from "assigning groups of users", in that the actual group object in addition to its members will be replicated from Azure AD to Slack. For example, if you have a group named "My Group" in Azure AD, an identitical group named "My Group" will be created inside Slack.

### To enable provisioning of group objects:

1) Under the Mappings section, select <strong>Synchronize Azure Active Directory Groups to Slack</strong>.

2) In the Attribute Mapping blade, set Enabled to Yes.

3) In the <strong>Attribute Mappings</strong> section, review the group attributes that will be synchronized from Azure AD to Slack. Note that the attributes selected as <strong>Matching</strong> properties will be used to match the groups in Slack for update operations. 

4) Click <strong>Save</strong>.

This result in any group objects assigned to Slack in the **Users and Groups** section being fully synchronized from Azure AD to Slack. You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity logs, which describe all actions performed by the provisioning service on your Slack app.

For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](active-directory-saas-provisioning-reporting.md).


## Additional Resources

* [Managing user account provisioning for Enterprise Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md)
