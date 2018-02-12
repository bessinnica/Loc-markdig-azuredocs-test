---
title: 'Connectivity providers and locations: Azure ExpressRoute | Microsoft Docs'
description: This article provides a detailed overview of locations where services are offered and how to connect to Azure regions. Sorted by connectivity provider.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
ms.assetid: c878513a-d594-42ad-8b0e-403efd0c4b25
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/08/2018
ms.author: pareshmu

---
# ExpressRoute partners and peering locations

> [!div class="op_single_selector"]
> * [Locations By Provider](expressroute-locations.md)
> * [Providers By Location](expressroute-locations-providers.md)


The tables in this article provide information on ExpressRoute connectivity providers, ExpressRoute geographical coverage, Microsoft cloud services supported over ExpressRoute, and ExpressRoute System Integrators (SIs).

## <a name="partners"></a>ExpressRoute connectivity providers
ExpressRoute is supported across all Azure regions and locations. The following map provides a list of Azure regions and ExpressRoute locations. ExpressRoute locations refer to those where Microsoft peers with several service providers.

![Location map][0]

You will have access to Azure services across all regions within a geopolitical region if you connected to at least one ExpressRoute location within the geopolitical region.

### Azure regions to ExpressRoute locations within a geopolitical region.
The following table provides a map of Azure regions to ExpressRoute locations within a geopolitical region.

| **Geopolitical region** | **Azure regions** | **ExpressRoute locations** |
| --- | --- | --- |
| **North America** |East US, West US, East US 2, West US 2, Central US, South Central US, North Central US, West Central US, Canada Central, Canada East |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miami, New York, San Antonio, Seattle, Silicon Valley, Washington DC, Montreal, Quebec City, Toronto |
| **South America** |Brazil South |Sao Paulo |
| **Europe** |North Europe, West Europe, UK West, UK South |Amsterdam, Dublin, London, Newport(Wales), Paris |
| **Asia** |East Asia, Southeast Asia |Hong Kong, Singapore, Singapore2 |
| **Japan** |Japan West, Japan East |Osaka, Tokyo |
| **Australia** |Australia Southeast, Australia East |Melbourne, Sydney |
| **India** |India West, India Central, India South |Chennai, Mumbai |
| **South Korea** |Korea Central, Korea South |Busan, Seoul |

### Regions and geopolitical boundaries for national clouds
The table below provides information on regions and geopolitical boundaries for national clouds.

| **Geopolitical region** | **Azure regions** | **ExpressRoute locations** |
| --- | --- | --- |
| **US Government cloud** |US Gov Arizona, US Gov Iowa, US Gov Texas, US Gov Virginia, US DoD Central, US DoD East  |Chicago, Dallas, New York, Seattle, Silicon Valley, Washington DC |
| **China** |China North, China East |Beijing, Shanghai |
| **Germany** |Germany Central, Germany East |Berlin, Frankfurt |

Connectivity across geopolitical regions is not supported on the standard ExpressRoute SKU. You will need to enable the ExpressRoute premium add-on to support global connectivity. Connectivity to national cloud environments is not supported. You can work with your connectivity provider if such a need arises.

## <a name="locations"></a>Connectivity provider locations

The following table shows locations by service provider. If you want to view available providers by location, see [Service providers by location](expressroute-locations-providers.md#locations).


### Production Azure

|                                                                                                                                                     <strong>Service provider</strong>                                                                                                                                                     | <strong>Microsoft Azure</strong> | <strong>Office 365 and Dynamics 365</strong> |                                                                                             <strong>Locations</strong>                                                                                              |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      <strong><a href="https://www.aarnet.edu.au/network-and-services/cloud-services-applications/azure-expressroute/" data-raw-source="[AARNet](https://www.aarnet.edu.au/network-and-services/cloud-services-applications/azure-expressroute/)">AARNet</a></strong>                                      |            Supported             |                  Supported                   |                                                                                                  Melbourne, Sydney                                                                                                  |
|                                                                                             <strong><a href="http://www.airtel.in/business/connexion" data-raw-source="[Airtel](http://www.airtel.in/business/connexion)">Airtel</a></strong>                                                                                             |            Supported             |                  Supported                   |                                                                                                   Chennai, Mumbai                                                                                                   |
|                                                                                                     <strong><a href="http://www.aryaka.com/" data-raw-source="[Aryaka Networks](http://www.aryaka.com/)">Aryaka Networks</a></strong>                                                                                                     |            Supported             |                  Supported                   |                                                                    Amsterdam, Dallas, Hong Kong, Silicon Valley, Singapore, Tokyo, Washington DC                                                                    |
|                                   <strong><a href="https://ascenty.com/solucoes/conectividade-e-interconexoes/Microsoft-express-route/" data-raw-source="[Ascenty Data Centers](https://ascenty.com/solucoes/conectividade-e-interconexoes/Microsoft-express-route/)">Ascenty Data Centers</a></strong>                                   |           Coming soon            |                 Coming soon                  |                                                                                                      Sao Paulo                                                                                                      |
|                                                   <strong><a href="https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm" data-raw-source="[AT&amp;T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)">AT&T NetBond</a></strong>                                                   |            Supported             |                  Supported                   |                                                        Amsterdam, Chicago, Dallas, London, Silicon Valley, Singapore, Sydney, Tokyo, Toronoto, Washington DC                                                        |
|                                        <strong><a href="https://business.bell.ca/shop/enterprise/cloud-connect-access-to-cloud-partner-services" data-raw-source="[Bell Canada](https://business.bell.ca/shop/enterprise/cloud-connect-access-to-cloud-partner-services)">Bell Canada</a></strong>                                        |            Supported             |                  Supported                   |                                                                                           Montreal, Toronto, Quebec City                                                                                            |
|                                  <strong><a href="http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure" data-raw-source="[British Telecom](http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)">British Telecom</a></strong>                                  |            Supported             |                  Supported                   |                                                                Amsterdam, Hong Kong, London, Silicon Valley, Singapore, Sydney, Tokyo, Washington DC                                                                |
|                                                                                                                 <strong><a href="https://c3ntro.com/" data-raw-source="[C3ntro](https://c3ntro.com/)">C3ntro</a></strong>                                                                                                                 |           Coming soon            |                 Coming soon                  |                                                                                                        Miami                                                                                                        |
|                                             <strong><a href="http://www.centurylink.com/business/enterprise/services/data-network/mpls-vpn.html" data-raw-source="[CenturyLink](http://www.centurylink.com/business/enterprise/services/data-network/mpls-vpn.html)">CenturyLink</a></strong>                                             |           Coming soon            |                 Coming soon                  |                                                                                                   Silicon Valley                                                                                                    |
|                                                                                                                                                   <strong>China Telecom Global</strong>                                                                                                                                                   |            Supported             |                Not Supported                 |                                                                                                      Hong Kong                                                                                                      |
|                                                      <strong><a href="http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/" data-raw-source="[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)">Cologix</a></strong>                                                      |            Supported             |                  Supported                   |                                                                                              Dallas, Montreal, Toronto                                                                                              |
|                              <strong><a href="http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm" data-raw-source="[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)">Colt</a></strong>                              |            Supported             |                  Supported                   |                                                                                       Amsterdam, Dublin, London, Paris, Tokyo                                                                                       |
|                                                                           <strong><a href="https://business.comcast.com/landingpage/microsoft-azure" data-raw-source="[Comcast](https://business.comcast.com/landingpage/microsoft-azure)">Comcast</a></strong>                                                                           |            Supported             |                  Supported                   |                                                                                       Chicago, Silicon Valley, Washington DC                                                                                        |
|                              <strong><a href="http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute" data-raw-source="[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)">CoreSite</a></strong>                              |            Supported             |                  Supported                   |                                                                        Chicago, Denver, Los Angeles, New York, Silicon Valley, Washinton DC                                                                         |
|                                                                                                                                                           <strong>eir</strong>                                                                                                                                                            |            Supported             |                  Supported                   |                                                                                                       Dublin                                                                                                        |
|                                                                                                                                              <strong>Epsilon Global Communications</strong>                                                                                                                                               |            Supported             |                  Supported                   |                                                                                                      Singapore                                                                                                      |
|                                                                                   <strong><a href="http://www.equinix.com/partners/microsoft-azure/" data-raw-source="[Equinix](http://www.equinix.com/partners/microsoft-azure/)">Equinix</a></strong>                                                                                   |            Supported             |                  Supported                   |            Amsterdam, Atlanta, Chicago, Dallas, Hong Kong, London, Los Angeles, Melbourne, New York, Osaka, Paris, Sao Paulo, Seattle, Silicon Valley, Singapore, Sydney, Tokyo, Toronto, Washington DC             |
|                                                                                                                                                        <strong>euNetworks</strong>                                                                                                                                                        |            Supported             |                  Supported                   |                                                                                                  Amsterdam, London                                                                                                  |
|                                                                                                                                                          <strong>GÉANT</strong>                                                                                                                                                           |            Supported             |                  Supported                   |                                                                                                      Amsterdam                                                                                                      |
|                          <strong><a href="http://globalcloudxchange.com/cloud-platform/cloud-x-fusion/cloud-x-fusion-for-azure/" data-raw-source="[Global Cloud Xchange (GCX)] (http://globalcloudxchange.com/cloud-platform/cloud-x-fusion/cloud-x-fusion-for-azure/)">Global Cloud Xchange (GCX)</a></strong>                           |            Supported             |                  Supported                   |                                                                                                   Chennai, Mumbai                                                                                                   |
|                                                                                                     <strong><a href="https://www.intercloud.com/" data-raw-source="[InterCloud](https://www.intercloud.com/)">InterCloud</a></strong>                                                                                                     |            Supported             |                  Supported                   |                                                                                 Amsterdam, London, Paris, Singapore, Washington DC                                                                                  |
|                                                                                                                                                        <strong>Internet2</strong>                                                                                                                                                         |            Supported             |                  Supported                   |                                                                                                    Washington DC                                                                                                    |
|                                            <strong><a href="http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html" data-raw-source="[Internet Initiative Japan Inc. - IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)">Internet Initiative Japan Inc. - IIJ</a></strong>                                            |            Supported             |                  Supported                   |                                                                                                    Osaka, Tokyo                                                                                                     |
|                                                                                                                                            <strong>Internet Solutions - Cloud Connect</strong>                                                                                                                                            |            Supported             |                  Supported                   |                                                                                                  Amsterdam, London                                                                                                  |
|                                                 <strong><a href="http://www.interxion.com/why-interxion/colocate-with-the-clouds/Microsoft-Azure/" data-raw-source="[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/Microsoft-Azure/)">Interxion</a></strong>                                                 |            Supported             |                  Supported                   |                                                                                          Amsterdam, Dublin, London, Paris                                                                                           |
|                                                              <strong><a href="https://www.ixreach.com/services/cloud-connectivity/microsoft-azure/" data-raw-source="[IX Reach](https://www.ixreach.com/services/cloud-connectivity/microsoft-azure/)">IX Reach</a></strong>                                                              |            Supported             |                  Supported                   |                                                                                                       Toronto                                                                                                       |
|                                                                                                                                                           <strong>Jisc</strong>                                                                                                                                                           |            Supported             |                  Supported                   |                                                                                                       London                                                                                                        |
|                                                                                                                                                           <strong>KINX</strong>                                                                                                                                                           |            Supported             |                  Supported                   |                                                                                                        Seoul                                                                                                        |
|                                                                                                        <strong><a href="http://www.kpn.com/cloudconnect" data-raw-source="[KPN](http://www.kpn.com/cloudconnect)">KPN</a></strong>                                                                                                        |            Supported             |                  Supported                   |                                                                                                      Amsterdam                                                                                                      |
|                                             <strong><a href="http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText" data-raw-source="[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)">Level 3 Communications</a></strong>                                             |            Supported             |                  Supported                   |                                 Amsterdam, Chicago, Dallas, Las Vegas, London, Newport, New York, San Antonio, Sao Paulo, Seattle, Silicon Valley, Singapore, Tokyo, Washington DC                                  |
|                                                                                                                                                          <strong>LG CNS</strong>                                                                                                                                                          |            Supported             |                  Supported                   |                                                                                                    Busan, Seoul                                                                                                     |
|                                                                         <strong><a href="https://www.megaport.com/services/microsoft-expressroute/" data-raw-source="[Megaport](https://www.megaport.com/services/microsoft-expressroute/)">Megaport</a></strong>                                                                         |            Supported             |                  Supported                   | Amsterdam, Chicago, Dallas, Dublin, Hong Kong, Las Vegas, London, Los Angeles, Melbourne, Miami, New York, Quebec City, San Antonio, Seattle, Silicon Valley, Singapore, Singapore2, Sydney, Toronto, Washington DC |
|                                                                                                                                                           <strong>MTN</strong>                                                                                                                                                            |            Supported             |                  Supported                   |                                                                                                       London                                                                                                        |
|                                                                    <strong><a href="http://www.neutrona.com/index.php/azure-expressroute/" data-raw-source="[Neutrona Networks](http://www.neutrona.com/index.php/azure-expressroute/)">Neutrona Networks</a></strong>                                                                    |            Supported             |                  Supported                   |                                                                                                  Miami, Sao Paulo                                                                                                   |
|                                                                <strong><a href="http://www.nextgenerationdata.co.uk/ngd-cloud-gateway/" data-raw-source="[Next Generation Data](http://www.nextgenerationdata.co.uk/ngd-cloud-gateway/)">Next Generation Data</a></strong>                                                                |            Supported             |                  Supported                   |                                                                                                   Newport(Wales)                                                                                                    |
|                                                                                                                                                          <strong>NEXTDC</strong>                                                                                                                                                          |            Supported             |                  Supported                   |                                                                                                  Melbourne, Sydney                                                                                                  |
|                                                     <strong><a href="http://www.ntt.com/en/services/network/virtual-private-network.html" data-raw-source="[NTT Communications](http://www.ntt.com/en/services/network/virtual-private-network.html)">NTT Communications</a></strong>                                                     |            Supported             |                  Supported                   |                                                                             London, Los Angeles, Osaka, Singapore, Tokyo, Washington DC                                                                             |
|                                                                                    <strong><a href="http://cloud.nttsmc.com/cxc/azure.html" data-raw-source="[NTT SmartConnect](http://cloud.nttsmc.com/cxc/azure.html)">NTT SmartConnect</a></strong>                                                                                    |            Supported             |                  Supported                   |                                                                                                        Osaka                                                                                                        |
|                                                                     <strong><a href="http://www.orange-business.com/en/products/business-vpn-galerie" data-raw-source="[Orange](http://www.orange-business.com/en/products/business-vpn-galerie)">Orange</a></strong>                                                                     |            Supported             |                  Supported                   |                                                                Amsterdam, Hong Kong, London, Paris, Silicon Valley, Singapore, Sydney, Washington DC                                                                |
|                                                                       <strong><a href="https://www.packetfabric.com/packetcor/microsoft-azure/" data-raw-source="[PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/)">PacketFabric</a></strong>                                                                       |            Supported             |                  Supported                   |                                                                                               Chicago, Silicon Valley                                                                                               |
|                                                                                                                                                   <strong>PCCW Global Limited</strong>                                                                                                                                                    |            Supported             |                  Supported                   |                                                                                                      Hong Kong                                                                                                      |
|                                                                                                                                                      <strong>Sejong Telecom</strong>                                                                                                                                                      |            Supported             |                  Supported                   |                                                                                                        Seoul                                                                                                        |
|                                                                                       <strong><a href="http://telecom.sify.com/azure-expressroute.html" data-raw-source="[SIFY](http://telecom.sify.com/azure-expressroute.html)">SIFY</a></strong>                                                                                       |            Supported             |                  Supported                   |                                                                                                   Chennai, Mumbai                                                                                                   |
|                  <strong><a href="http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud" data-raw-source="[SingTel](http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)">SingTel</a></strong>                  |            Supported             |                  Supported                   |                                                                                                Singapore, Singapore2                                                                                                |
|                                                               <strong><a href="http://www.softbank.jp/biz/cloud/cloud_access/direct_access_for_az/" data-raw-source="[Softbank](http://www.softbank.jp/biz/cloud/cloud_access/direct_access_for_az/)">Softbank</a></strong>                                                               |            Supported             |                  Supported                   |                                                                                                    Osaka, Tokyo                                                                                                     |
|                                                        <strong><a href="http://www.tatacommunications.com/lp/izo/azure/azure_index.html" data-raw-source="[Tata Communications](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)">Tata Communications</a></strong>                                                        |            Supported             |                  Supported                   |                                                               Amsterdam, Chennai, Hong Kong, London, Mumbai, Silicon Valley, Singapore, Washington DC                                                               |
|                                 <strong><a href="http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&amp;xml" data-raw-source="[TeleCity Group](http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&amp;xml)">TeleCity Group</a></strong>                                 |            Supported             |                  Supported                   |                                                                                              Amsterdam, Dublin, London                                                                                              |
| <strong><a href="https://www.business-solutions.telefonica.com/es/enterprise/solutions/efficient-infrastructure/managed-voice-data-connectivity/" data-raw-source="[Telefonica](https://www.business-solutions.telefonica.com/es/enterprise/solutions/efficient-infrastructure/managed-voice-data-connectivity/)">Telefonica</a></strong> |            Supported             |                  Supported                   |                                                                                                Amsterdam, Sao Paulo                                                                                                 |
|                                                              <strong><a href="http://www.telehouse.net/solutions/cloud-services/cloud-link" data-raw-source="[Telehouse - KDDI](http://www.telehouse.net/solutions/cloud-services/cloud-link)">Telehouse - KDDI</a></strong>                                                              |            Supported             |                  Supported                   |                                                                                                       London                                                                                                        |
|                                                                                                                                                         <strong>Telenor</strong>                                                                                                                                                          |            Supported             |                  Supported                   |                                                                                                  Amsterdam, London                                                                                                  |
|                          <strong><a href="http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/" data-raw-source="[Telstra Corporation](http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)">Telstra Corporation</a></strong>                          |            Supported             |                  Supported                   |                                                                                                  Melbourne, Sydney                                                                                                  |
|                                                                                                                 <strong><a href="http://www.telus.com" data-raw-source="[Telus](http://www.telus.com)">Telus</a></strong>                                                                                                                 |            Supported             |                  Supported                   |                                                                                                  Montreal, Toronto                                                                                                  |
|                                                                               <strong><a href="http://www.uoldiveo.com.br/solucoes/cloud.html#rmcl" data-raw-source="[UOLDIVEO](http://www.uoldiveo.com.br/solucoes/cloud.html#rmcl)">UOLDIVEO</a></strong>                                                                               |            Supported             |                  Supported                   |                                                                                                      Sao Paulo                                                                                                      |
|                                                    <strong><a href="http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/" data-raw-source="[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)">Verizon</a></strong>                                                    |            Supported             |                  Supported                   |                                                       Amsterdam, Chicago, Dallas, Hong Kong, London, Silicon Valley, Singapore, Sydney, Tokyo, Washington DC                                                        |
|                              <strong><a href="http://www.vodafone.com/business/global-enterprise/global-connectivity/vodafone-ip-vpn-cloud-connect" data-raw-source="[Vodafone](http://www.vodafone.com/business/global-enterprise/global-connectivity/vodafone-ip-vpn-cloud-connect)">Vodafone</a></strong>                              |            Supported             |                Not Supported                 |                                                                                                       London                                                                                                        |
|                                                    <strong><a href="http://www.zayo.com/solutions/industries/cloud-connectivity/microsoft-expressroute" data-raw-source="[Zayo](http://www.zayo.com/solutions/industries/cloud-connectivity/microsoft-expressroute)">Zayo</a></strong>                                                    |            Supported             |                  Supported                   |                                                     Amsterdam, Chicago, Dallas, London, Los Angeles, Montreal, New York, Silicon Valley, Toronto, Washington DC                                                     |

 **+** denotes coming soon

### National cloud environment

### US Government cloud

| **Service provider** | **Microsoft Azure** | **Office 365** | **Locations** |
| --- | --- | --- | --- |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Supported |Supported |Chicago, Washington DC |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Supported |Supported |Chicago, Dallas, New York, Seattle, Silicon Valley, Washington DC |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Supported |Supported |Chicago, New York+, Silicon Valley, Washington DC |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Supported | Supported | Chicago, Dallas |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** |Supported |Supported |Chicago, Dallas, New York, Washington DC |

### China

| **Service provider** | **Microsoft Azure** | **Office 365** | **Locations** |
| --- | --- | --- | --- |
| **China Telecom** |Supported |Not Supported |Beijing, Shanghai |

To learn more, see [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/).

### Germany

| **Service provider** | **Microsoft Azure** | **Office 365** | **Locations** |
| --- | --- | --- | --- |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Supported |Not Supported |Berlin+, Frankfurt |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Supported |Not Supported |Frankfurt |
| **[e-shelter](https://www.e-shelter.de/en/microsoft-expressroutetm)** |Supported |Not Supported |Berlin |
| **Interxion** |Supported |Not Supported |Frankfurt |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Supported  | Not Supported | Berlin |
| **T-Systems** |Supported |Not Supported |Berlin |

## Connectivity Through Exchange Providers

If your connectivity provider is not listed in previous sections, you can still create a connection.

* Check with your connectivity provider to see if they are connected to any of the exchanges in the table above. You can check the following links to gather more information about services offered by exchange providers. Several connectivity providers are already connected to Ethernet exchanges.
  * [Cologix](http://www.cologix.com/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [Interxion](http://www.interxion.com/products/interconnection/cloud-connect/)
  * [IX Reach](https://www.ixreach.com/services/cloud-connectivity/microsoft-azure/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [NextDC](http://www.nextdc.com/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* Have your connectivity provider extend your network to the peering location of choice.
  * Ensure that your connectivity provider extends your connectivity in a highly available manner so that there are no single points of failure.
* Order an ExpressRoute circuit with the exchange as your connectivity provider to connect to Microsoft.
  * Follow steps in [Create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) to set up connectivity.

## Connectivity Through Additional Service Providers

| **Connectivity provider** | **Exchange** | **Locations** |
| --- | --- | --- |
| **[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)** |Equinix |Singapore |
| **[Airgate Technologies, Inc.](http://airgate.ca/cloud-express/)** | Equinix, Cologix | Toronto, Montreal |
| **[Alaska Communications](http://www.alaskacommunications.com/For-Your-Business/Direct-Cloud-Service)** |Equinix |Seattle |
| **[Altice Business](https://golightpath.com/transport)** |Equinix |New York, Washington DC |
| **[Arteria Networks Corporation](https://arteria-net.com/business/service/cloud_access/sca/)** |Equinix |Tokyo |
| **[Bezeq International Ltd.](https://www.bezeqint.net/english)** | euNetworks | London |
| **[BroadBand Tower, Inc.](http://www.bbtower.co.jp/product-service/data-center/network/dcconnect-for-azure/)** | Equinix | Tokyo |
| **[C3ntro Telecom](http://www.c3ntro.com/data/cloud-conectivity/)** | Equinix, Megaport | Dallas |
| **[Cinia](https://www.cinia.fi/en/services/connectivity-services/direct-public-cloud-connection.html)** | Equinix, Megaport | Frankfurt, Hamburg |
| **[Cogeco Peer 1](https://www.cogecopeer1.com/en/)**| Equinix | Montreal, Toronto |
| **[Cox Business](https://www.cox.com/business/networking/cloud-connectivity.html)** | Equinix | Dallas, Silicon Valley, Washington DC | 
| **[Data Foundry](https://www.datafoundry.com/services/cloud-connect)** | Megaport | Dallas |
| **[Epsilon Telecommunications Limited](http://www.epsilontel.com/data-connectivity/cloud-access/)** | Equinix | London, Singapore, Washington DC |
| **[Eurofiber](https://eurofiber.nl/microsoft-azure/)** | Equinix | Amsterdam |
| **[Exponential E](http://www.exponential-e.com/services/connectivity-services/cloud-connect-exchange)** | Equinix | London |
| **[Fastweb S.p.A](http://www.fastweb.it/grandi-aziende/connessione-voce-e-wifi/scheda-prodotto/rete-privata-virtuale/)** | Equinix | Amsterdam |
| **[Gtt Communications Inc](https://www.gtt.net/wp-content/uploads/2017/04/EtherCloud-Data-Sheet.pdf)** |Equinix | Washington DC |
| **[HSO](http://www.hso.co.uk/products/cloud-direct)** |Equinix | London, Slough |
| **LGA Telecom** |Equinix |Singapore|
| **[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure)** |Equinix | Chicago, New York, Washington DC |
| **[Macroview Telecom](http://www.macroview.com/en/scripts/catitem.php?catid=solution&sectionid=expressroute)** |Equinix |Hong Kong |
| **[Macquarie Telecom Group](https://macquariegovernment.com/secure-cloud/secure-cloud-exchange/)** | Megaport | Sydney |
| **[MainOne](https://www.mainone.net/services/connectivity/cloud-connect/)** |Equinix | Amsterdam |
| **[Masergy](https://www.masergy.com/solutions/hybrid-networking/cloud-marketplace/microsoft-azure)** | Equinix | Washington DC |
| **[NexGen Networks](http://www.nexgen-net.com/nexgen-networks-direct-connect-microsoft-azure-expressroute.html)** | Interxion | London |
| **[Nianet](https://nianet.dk/produkter/internet/microsoft-expressroute)** |Telecity | Amsterdam, Frankfurt |  
| **[QSC AG](https://www.qsc.de/de/produkte-loesungen/cloud-services-und-it-outsourcing/pure-enterprise-cloud/multi-cloud-management/azure-expressroute/)** |Interxion | Frankfurt |  
| **Rogers** | Cologix, Equinix | Montreal, Toronto |
| **[Tamares Telecom](http://www.tamarestelecom.com/our-services/#Connectivity)** | Telecity | London | 
| **[Telia](https://www.telia.se/foretag/losningar/produkter-tjanster/datanet)** | Equinix | Amsterdam |
| **[ThinkTel](http://www.thinktel.ca/services/agile-ix-data/expressroute/)** | Equinix | Toronto | 
| **[Transtelco](http://www.transtelco.net/tcloud/microsoft)** |Equinix | Dallas, Los Angeles |  
| **[United Information Highway (UIH)](https://www.uih.co.th/en/internet-solution/cloud-direct/uih-cloud-direct-for-microsoft-azure-expressroute)**| Equinix | Singapore |
| **[Webair](https://www.webair.com/microsoft-express-route-partnership/)**| Megaport | New York |
| **[Windstream](http://www.windstreambusiness.com/solutions/cloud-services/cloud-and-managed-hosting-services)**| Equinix | Chicago, Silicon Valley, Washington DC |
| **Zain** |Equinix |London|
| **[Zertia](http://www.zertia.es/index.php/novedades)**| Level 3 | Madrid |
| **[Zirro](https://zirro.com/services/)**| Equinix | Montreal, Toronto |

## Connectivity Through Datacenter Providers

| **Provider** | **Exchange** |
| --- | --- |
| **[Cyrus One](https://cyrusone.com/enterprise-data-center-services/connectivity-and-interconnection/cloud-connectivity-reaching-amazon-microsoft-google-and-more/microsoft-azure-expressroute/?doing_wp_cron=1498512235.6733090877532958984375)** | Megaport |
| **[Digital Realty](https://www.digitalrealty.com/services/interconnection/service-exchange/)** | Megaport |
| **[EdgeConnex](http://www.edgeconnex.com/services/edge-data-centers-proximity-matters/)** | Megaport |
| **[RagingWire Data Centers](http://www.ragingwire.com/wholesale/wholesale-data-centers-worldwide-nexcenters)** | IX Reach |
| **[T5 Datacenters](http://t5datacenters.com/network-cloud-connect/)** | IX Reach |

## Connectivity Through National Research and Education Networks (NREN)

| **Provider**|
| --- |
| **AARNET**| 
| **DeIC, through GÉANT**|
| **GARR, through GÉANT**|
| **GÉANT**|
| **HEAnet, through GÉANT**|
| **Internet2**|
| **JISC**|
| **RedIRIS, through GÉANT**|
| **SINET**|
| **Surfnet, through GÉANT**|

* If your connectivity provider is not listed here, please check to see if they are connected to any of the ExpressRoute Exchange Partners listed above.

## ExpressRoute system integrators
Enabling private connectivity to fit your needs can be challenging, based on the scale of your network. You can work with any of the system integrators listed in the following table to assist you with onboarding to ExpressRoute.

| **System integrator** | **Continent** |
| --- | --- |
| **[Altogee](http://www.altogee.be/expressroute)** | Europe |
| **[Avanade Inc.](http://www.avanade.com/)** | Asia, Europe, North America, South America |
| **[Bright Skies GmbH](http://bskies.io/expressroute)** | Europe
| **[Ensyst](http://www.ensyst.com.au)** | Asia
| **[Equinix Professional Services](http://www.equinix.com/services/consulting/)** | North America |
| **[FlexManage](http://www.flexmanage.com/cloud)** | North America |
| **[Inframon](http://www.inframon.com/partner/microsoft/)** | Europe |
| **[The IT Consultancy Group](http://itconsult.com.au/microsoft-expressroute)** | Australia |
| **[MOQdigital](http://www.moqdigital.com.au/insights/technical/network-connectivity-options-for-azure)** | Australia |
| **[MSG Services](https://www.msg-services.de/it-services/managed-services/cloud-outsourcing/)** | Europe (Germany) |
| **[Nelite](http://nelite.com/)** | Europe |
| **[New Signature](https://www.newsignature.co.uk/)** | Europe |
| **[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** | Asia |
| **[Orange Networks](https://orange-networks.com/blog/88-azureexpressroute)** | Europe |
| **[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | North America |
| **[Presidio](http://info.presidio.com/microsoft-azure-expressroute)** | North America |
| **[sol-tec](http://www.sol-tec.com/Technologies)** | Europe |
| **[Vigilant.IT](https://vigilant.it/expressroute)** | Australia |


## Next steps
* For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).
* Ensure that all prerequisites are met. See [ExpressRoute prerequisites](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Location map"
