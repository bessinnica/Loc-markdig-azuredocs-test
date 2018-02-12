---
title: Language Understanding Intelligent Services (LUIS) in Azure frequently asked questions | Microsoft Docs
titleSuffix: Azure
description:  Get answers to frequently asked questions about Language Understanding Intelligent Services (LUIS)
services: cognitive-services
author: DeniseMak
manager: hsalama

ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 01/31/2018
ms.author: v-demak;v-geberr;
---
# Language Understanding FAQ

This article contains answers to frequently asked questions about Language Understanding (LUIS).

## How do I interpret LUIS scores? 
Your system should use the highest scoring intent regardless of its value. For example, a score below 0.5 does not necessarily mean that LUIS has low confidence. Providing more training data can help increase the score of the most-likely intent.

## What is the maximum number of intents and entities that a LUIS app can support?
See the [boundaries](luis-boundaries.md) reference.

## I want to build a LUIS app with more than the maximum number of intents. What should I do?

First, consider whether your system is using too many intents. Intents that are too similar can make it more difficult for LUIS to distinguish between them. Intents should be varied enough to capture the main tasks that the user is asking for, but they don't need to capture every path your code takes. For example, BookFlight and BookHotel might be separate intents in a travel app, but BookInternationalFlight and BookDomesticFlight are too similar. If your system needs to distinguish them, use entities or other logic rather than intents.

If you cannot use fewer intents, divide your intents into multiple LUIS apps, and group related intents. This approach is a best practice if you're using multiple apps for your system. For example, let's say you're developing an office assistant that has over 500 intents. If 100 intents relate to scheduling meetings, 100 are about reminders, 100 are about getting information about colleagues, and 100 are for sending email, you can put the intent for each of those categories in a separate LUIS app. 

When your system receives an utterance, you can use a variety of techniques to determine how to direct user utterances to LUIS apps:

* Create a top-level LUIS app to determine the category of utterance, and then use the result to send the utterance to the LUIS app for that category.
* Do some preprocessing on the utterance, such as matching on [regular expressions](#where-is-the-pattern-feature-that-provides-regular-expression-matching), to determine which LUIS app or set of apps receives it.

When you're deciding which approach to use with multiple LUIS apps, consider the following trade-offs:
* **Saving suggested utterances for training**: Your LUIS apps get a performance boost when you label the user utterances that the apps receive, especially the [suggested utterances](./Label-Suggested-Utterances.md) that LUIS is relatively unsure of. Any LUIS app that doesn't receive an utterance won't have the benefit of learning from it.
* **Calling LUIS apps in parallel instead of in series**: To improve responsiveness, you might ordinarily design a system to reduce the number of REST API calls that happen in series. But if you send the utterance to multiple LUIS apps and pick the intent with the highest score, you can call the apps in parallel by sending all the requests asynchronously. If you call a top-level LUIS app to determine a category, and then use the result to send the utterance to another LUIS app, the LUIS calls happen in series.

If reducing the number of intents or dividing your intents into multiple apps doesn't work for you, contact support. To do so, gather detailed information about your system, go to the [LUIS][LUIS] website, and then select **Support**. If your Azure subscription includes support services, contact [Azure technical support](https://azure.microsoft.com/en-us/support/options/).

## I want to build an app in LUIS with more than the maximum number of entities. What should I do?

You might need to use hierarchical and composite entities. Hierarchical entities reflect the relationship between entities that share characteristics or are members of a category. The child entities are all members of their parent's category. For example, a hierarchical entity named PlaneTicketClass might have the child entities EconomyClass and FirstClass. The hierarchy spans only one level of depth. 

Composite entities represent parts of a whole. For example, a composite entity named PlaneTicketOrder might have child entities Airline, Destination, DepartureCity, DepartureDate, and PlaneTicketClass. You build a composite entity from pre-existing simple entities, children of hierarchical entities, or prebuilt entities. 

LUIS also provides the list entity type that is not machine-learned but allows your LUIS app to specify a fixed list of values. A list entity can have up to 20,000 items.

If you've considered hierarchical, composite, and list entities and still need more than the limit, contact support. To do so, gather detailed information about your system, go to the [LUIS][LUIS] website, and then select **Support**. If your Azure subscription includes support services, contact [Azure technical support](https://azure.microsoft.com/en-us/support/options/).

## What are the limits on the number and size of phrase lists?
For the maximum length of a [phrase list](./luis-concept-feature.md), see the [boundaries](luis-boundaries.md) reference.

## What are the limits on example utterances?
See the [boundaries](luis-boundaries.md) reference.

## What is the best way to start building my app in LUIS?

The best way to build your app is through an incremental process. First start by defining what intents and entities your app requires. For every intent and entity model, you can start by providing a few examples. Train and publish your app to get an endpoint. Then, begin to [review endpoint utterances](./label-suggested-utterances.md) to select the most informative utterances to label. You can select the intent or entity that you want to improve, and then label the utterances that are suggested by LUIS. <!-- Labeling a few hundred utterances should result in a decent accuracy for intents. Entities might need more examples to converge. -->

## What is a good practice to model the intents of my app? Should I create more specific or more generic intents?

Choose intents that are not so general as to be overlapping, but not so specific that it makes it difficult for LUIS to distinguish between similar intents. Creating discriminative specific intents is one of the best practices for LUIS modeling.

## Is it important to train the None intent?

Yes, it is good to train your **None** intent with more utterances as you add more labels to other intents. A good ratio is 1 or 2 labels added to **None** for every 10 labels added to an intent. This ratio boosts the discriminative power of LUIS.

## How can I deal with spelling mistakes in utterances?

You can integrate your LUIS app with Bing Spell Check to spell check utterances before sending them to the LUIS endpoint. To enable Bing Spell Check, do the following steps:
   1. [Get an API key](https://azure.microsoft.com/en-us/try/cognitive-services/?api=spellcheck-api) for [Bing SpellCheck API v7](https://azure.microsoft.com/en-us/services/cognitive-services/spell-check/).  Free trial keys provide 1,000 transactions per month, up to 1 per second. They expire after a 30-day period.
   2. Check the **Enable Bing spell checker** checkbox in the [Publish app](./PublishApp.md) page when you publish your app.
   3. Include `spellCheck=true` in the query to the LUIS app's endpoint when you access the app from your client application. If the **Enable Bing spell checker** checkbox is checked, this parameter is prepopulated in the endpoint URL that the **Publish app** page displays.
   4. Include the `bing-spell-check-subscription-key` parameter in the query to the LUIS app's endpoint when you access the app from your client application. Set the parameter to the value of your Bing SpellCheck API key. If the **Enable Bing spell checker** checkbox is checked, a placeholder for this parameter is prepopulated in the endpoint URL that the **Publish app** page displays.

If Bing Spell Check detects a misspelling, the `query` field in the LUIS app's JSON response contains the original query, and the `alteredQuery` field contains the corrected query sent to LUIS.

```json
{
  "query": "boook a flight",
  "alteredQuery": "book a flight",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.9714768
  },
  "entities": []
}
```

If you don't want to use a spell check service, you can label utterances that have spelling mistakes so that LUIS can learn proper spelling as well as typos. This option requires more labeling effort than using a spell checker.

## I see some errors in the batch testing pane for some of the models in my app. How can I address this problem?

The errors indicate that there is some discrepancy between your labels and the predictions from your models. To address the problem, do one or both of the following tasks:
* To help LUIS improve discrimination among intents, add more labels.
* To help LUIS learn faster, add phrase-list features that introduce domain-specific vocabulary.

## Why don't I see my endpoint hits in my app's Dashboard?
The total endpoint hits in your app's Dashboard are updated periodically, but the metrics associated with your LUIS Subscription key in the Azure portal are updated more frequently. If you don't see updated endpoint hits in the Dashboard, log in to the Azure portal, and find the resource associated with your LUIS subscription key, and open **Metrics** to select the **Total Calls** metric. If the subscription key is used for more than one LUIS app, the metric in the Azure portal shows the aggregate number of calls from all LUIS apps that use it.

## How do I transfer ownership of a LUIS app?
To transfer a LUIS app to a different Azure subscription, export the LUIS app and import it using a new account. Update the LUIS app ID in the client application that calls it. The new app may return slightly different LUIS scores from the original app. 

## I have an app in one language and want to create a parallel app in another language. What is the easiest way to do so?
1. Export your app.
2. Translate the labeled utterances in the JSON file of the exported app to the target language.
3. You might need to change the names of the intents and entities or leave them as they are.
4. Finally, import the app to have a LUIS app in the target language.

## How do I download a log of user utterances?
By default, your LUIS app logs utterances from users. To download a log of utterances that users send to your LUIS app, go to **My Apps**, and click on the ellipsis (***...***) in the listing for your app. Then click **Export Endpoint Logs**. The log is formatted as a comma-separated value (CSV) file.

## How can I disable the logging of utterances?
You can turn off the logging of user utterances by setting `log=false` in the Endpoint URL that your client application uses to query LUIS. However, turning off logging disables your LUIS app's ability to suggest utterances or improve performance that's based on user queries. If you set `log=false` because of data-privacy concerns, you won't be able to download a record of those user utterances from LUIS or use those utterances to improve your app.

## Why don't I want all my endpoint utterances logged?
You do not want any test utterances captured in your log if you are using your log for prediction analysis. 

## Can I delete data from LUIS? 

* You can always delete example utterances used for training LUIS. If you delete an example utterance from your LUIS app, it is removed from the LUIS web service and is unavailable for export.
* You can delete utterances from the list of user utterances that LUIS suggests in the **Review endpoint utterances** page. Deleting utterances from this list prevents them from being suggested, but doesn't delete them from logs.
* If you delete an account, all apps are deleted, along with their example utterances and logs. The data is retained on the servers for 60 days before it is deleted permanently.

## How do I edit my LUIS app programmatically?
To edit your LUIS app programmatically, use the [Authoring API](https://aka.ms/luis-authoring-apis). See [Call LUIS authoring API](./luis-quickstart-node-add-utterance.md) and [Build a LUIS app programmatically using Node.js](./luis-tutorial-node-import-utterances-csv.md) for examples of how to call the Authoring API. The Authoring API requires that you use a programmatic key rather than an endpoint key. Programmatic authoring allows up to 1,000,000 calls per month and five transactions per second. For more info on the keys you use with LUIS, see [Manage keys](./Manage-Keys.md).

## What is the tenant ID in the "Add a key to your app" window?
In Azure, a tenant represents the client or organization that's associated with a service. Find your tenant ID in the Azure portal in the **Directory ID** box by selecting **Azure Active Directory** > **Manage** > **Properties**.

![Tenant ID in the Azure portal](./media/luis-manage-keys/luis-assign-key-tenant-id.png)

## Why did I get an email saying I'm almost out of quota?
Your programmatic/starter key is only allowed 1000 endpoint queries a month. You should create a LUIS subscription key (free or paid) and use that key when making endpoint queries. If you are making endpoint queries from a bot or another client application, you need to change the LUIS endpoint key there. 

## Where is the Pattern feature that provides regular expression matching?
The Pattern feature is currently deprecated. Pattern features in LUIS are provided by [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text). If you have a regular expression you need, or a culture in which a regular expression is not provided, contribute to the Recognizers-Text project. 

## Why are there more subscription keys on my app's publish page than I assigned to the app? 
Each LUIS app has the programmatic/starter key. LUIS subscription keys created during the GA time frame will be visible on your publish page, regardless if you added them to the app. This was done to make GA migration easier. Any new LUIS subscription keys will not appear on the publish page. 

## Next steps

To learn more about LUIS, see the following resources:
* [Stack Overflow questions tagged with LUIS](https://stackoverflow.com/questions/tagged/luis)
* [MSDN Language Understanding Intelligent Services (LUIS) Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=LUIS) 

[LUIS]:luis-reference-regions.md