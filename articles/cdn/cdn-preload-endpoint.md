---
title: Pre-load assets on an Azure CDN endpoint | Microsoft Docs
description: Learn how to pre-load cached content on an Azure CDN endpoint.
services: cdn
documentationcenter: ''
author: dksimpson
manager: erikre
editor: ''

ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: mazha

---
# Pre-load assets on an Azure CDN endpoint
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

By default, assets are cached only when they are requested. As a result, the first request from each region can take longer than subsequent requests. The reason is because the edge servers have not yet cached the content and need to forward the request to the origin server. By pre-loading content, you can avoid this first-hit latency.

In addition to providing a better customer experience, pre-loading your cached assets can also reduce network traffic on the origin server.

> [!NOTE]
> Pre-loading assets is useful for large events or content that becomes simultaneously available to a large number of users, such as a new movie release or a software update.
> 
> 

This tutorial walks you through pre-loading cached content on all Azure CDN edge nodes.

## To pre-load assets
1. In the [Azure portal](https://portal.azure.com), browse to the CDN profile containing the endpoint you wish to pre-load. The profile pane opens.
    
2. Click the endpoint in the list. The endpoint pane opens.
3. From the CDN endpoint pane, select **Load**.
   
    ![CDN endpoint pane](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    The **Load** pane opens.
   
    ![CDN load pane](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. For **Content path**, enter the full path of each asset you wish to load (for example, `/pictures/kitten.png`).
   
   > [!TIP]
   > More **Content path** textboxes will appear after you start entering text to allow you to build a list of multiple assets. To delete assets from the list, select the ellipsis (...) button, then select **Delete**.
   > 
   > Each content path must be a relative URL that fits the following [regular expressions](https://msdn.microsoft.com/library/az24scfc.aspx):  
   > - Load a single file path: `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;  
   > - Load a single file with query string: `@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";` 
   > 
   > Each asset must have its own path. There is no wildcard functionality for pre-loading assets.
   > 
   > 
   
    ![Load button](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. When you are finished entering content paths, select **Load**.
   

> [!NOTE]
> There is a limitation of 10 load requests per minute per CDN profile. 50 concurrent paths can be processed at one time. Each path has a path-length limit of 1024 characters.
> 
> 

## See also
* [Purge an Azure CDN endpoint](cdn-purge-endpoint.md)
* [Azure CDN REST API reference: Pre-load content on an endpoint](https://docs.microsoft.com/en-us/rest/api/cdn/endpoints/loadcontent)
* [Azure CDN REST API reference: Purge content from an endpoint](https://docs.microsoft.com/en-us/rest/api/cdn/endpoints/purgecontent)

