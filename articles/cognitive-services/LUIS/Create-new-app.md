---
title: Create a new app with LUIS | Microsoft Docs
description: Create and manage your applications on the Language Understanding (LUIS) webpage. 
services: cognitive-services
author: cahann
manager: hsalama

ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 12/13/2017
ms.author: cahann
---

# Create an app
You create a new app in different ways: 

* [Start](#create-new-app) with an empty app and create intents, utterances, and entities.
* [Start](#create-new-app) with an empty app and add a [prebuilt domain](luis-how-to-use-prebuilt-domains.md).
* [Import a LUIS app](#import-new-app) from a JSON file that already contains intents, utterances, and entities.

## What is an app
The app contains [versions](luis-how-to-manage-versions.md) of your model as well as any [collaborators](luis-how-to-collaborate.md) for the app. When you create the app, you select the culture ([language](luis-supported-languages.md)) which **cannot be changed later**. 

The default version of a new app is "0.1." 

You can create and manage your applications on **My Apps** page. You can always access this page by clicking **My apps** on the top navigation bar of the [LUIS](luis-reference-regions.md) website. 

![List of apps](./media/luis-create-new-app/apps-list.png)

## Create new app

1. On **My Apps** page, click **Create new app**.
2. In the dialog box, name your application "TravelAgent".

    ![Create new app dialog](./media/luis-create-new-app/create-app.png)

3. Choose your application culture (for TravelAgent app, we’ll choose English), and then click **Done**. 

    >[!NOTE]
    >The culture cannot be changed once the application is created. 

## Import new app
You can set the name (50 char max), version (10 char max), and description of an app in the JSON file. Examples of application JSON files are available at [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/Examples-BookFlight).

1. On **My Apps** page, click **Import new app**.
2. In the **Import new app** dialog, select the JSON file defining the LUIS app.

    ![Import a new app dialog](./media/luis-create-new-app/import-app.png)

## Next steps

Your first task in the app is to [add intents](Add-intents.md).