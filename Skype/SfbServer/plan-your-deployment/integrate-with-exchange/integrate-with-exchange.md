---
title: "Plan to integrate Skype for Business and Exchange"
ms.reviewer: 
ms.author: serdars
author: SerdarSoysal
manager: serdars
audience: ITPro
ms.topic: conceptual
ms.service: skype-for-business-server
f1.keywords:
- NOCSH
ms.localizationpriority: medium
ms.collection: IT_Skype16
ms.assetid: ea22beb9-c02e-47cb-836d-97a556969052
description: "Summary: Review this topic for information about how to integrate Skype for Business Server with Exchange Server 2016 or Exchange Server 2013."
---

# Plan to integrate Skype for Business and Exchange
 
**Summary:** Review this topic for information about how to integrate Skype for Business Server with Exchange Server 2016 or Exchange Server 2013.
  
Before you can integrate Skype for Business Server and Exchange Server, you must ensure that both Exchange Server and Skype for Business Server are fully installed and up and running. 
  
For details about installing Exchange Server, see the Exchange Server Planning and Deployment documentation for your version of Exchange. 
   
After the servers are up and running, you must assign server-to-server authentication certificates to both Skype for Business Server and Exchange Server; these certificates allow Skype for Business Server and Exchange Server to exchange information and to communicate with one another. When you install Exchange Server, a self-signed certificate with the name Microsoft Exchange Server Auth Certificate is created for you. This certificate, which can be found in the local computer certificate store, should be used for server-to-server authentication on Exchange Server. For details about assigning certificates in Exchange Server, see [Configure Mail Flow and Client Access](/exchange/configure-mail-flow-and-client-access-exchange-2013-help).
  
For Skype for Business Server you can use an existing Skype for Business Server certificate as your server-to-server authentication certificate; for example, your default certificate can also be used as the OAuthTokenIssuer certificate. Skype for Business Server allows you to use any Web server certificate as the certificate for server-to-server authentication provided that:
  
- The certificate includes the name of your SIP domain in the Subject field.
    
- The same certificate is configured as the OAuthTokenIssuer certificate on all of your Front End Servers.
    
- The certificate has a length of at least 2048 bits.
    
For details about server-to-server authentication certificates for Skype for Business Server, see [Assign a server-to-server authentication certificate to Skype for Business Server](../../manage/authentication/assign-a-server-to-server-certificate.md).
  
After the certificates have been assigned, you must then configure the autodiscover service on Exchange Server. In Exchange Server, the autodiscover service configures user profiles and provides access to Exchange services when users log on to the system. Users present the autodiscover service with their email address and password; in turn, the services provide the user with information such as:
  
- Connection information for both internal and external connectivity to Exchange Server.
    
- The location of the user's Mailbox server.
    
- URLs for Outlook features such as free/busy information, Unified Messaging, and the offline address book.
    
- Outlook Anywhere server settings.
    
The autodiscover service must be configured before you can integrate Skype for Business Server and Exchange Server. You can verify whether or not the autodiscover service has been configured by running the following command from the Exchange Server Management Shell and checking the value of the AutoDiscoverServiceInternalUri property:
  
```PowerShell
Get-ClientAccessServer | Select-Object Name, AutoDiscoverServiceInternalUri | Format-List
```

If this value is blank, you must assign a URI to the autodiscover service. Typically this URI will look similar to this: https://autodiscover.litwareinc.com/autodiscover/autodiscover.xml
  
You can assign the autodiscover URI by running a command similar to this:
  
```PowerShell
Get-ClientAccessServer | Set-ClientAccessServer -AutoDiscoverServiceInternalUri "https://autodiscover.litwareinc.com/autodiscover/autodiscover.xml"
```

For details about the autodiscover service, see [Autodiscover Service](/Exchange/architecture/client-access/autodiscover).
  
After the autodiscover service has been configured, you must then modify the Skype for Business Server OAuth configuration settings; this ensures that Skype for Business Server knows where to find the autodiscover service. To modify the OAuth configuration settings in Skype for Business Server, run the following command from within the Skype for Business Server Management Shell. When running this command, be sure that you specify the URI to the autodiscover service running on your Exchange Server, and that you use **autodiscover.svc** to point to the service location instead of **autodiscover.xml** (which points to the XML file used by the service):
  
```PowerShell
Set-CsOAuthConfiguration -Identity global -ExchangeAutodiscoverUrl "https://autodiscover.litwareinc.com/autodiscover/autodiscover.svc" 
```

> [!NOTE]
> The Identity parameter in the preceding command is optional; that's because Skype for Business Server only allows you to have a single, global collection of OAuth configuration settings. Among other things, that means that you can configure the autodiscover URL by using this slightly-simpler command: 
> 
> [!NOTE]
> Set-CsOAuthConfiguration-ExchangeAutodiscoverUrl "<https://autodiscover.litwareinc.com/autodiscover/autodiscover.svc>" 
> 
> [!NOTE]
> If you are unfamiliar with the technology, OAuth is a standard authorization protocol used by a number of major websites. With OAuth, user credentials and passwords are not passed from one computer to another. Instead, authentication and authorization is based on the exchange of security tokens; these tokens grant access to a specific set of resources for a specific amount of time. 
  
In addition to configuring the autodiscover service, you must also create a DNS record for the service that points to your Exchange Server. For example, if your autodiscover service is located at autodiscover.litwareinc.com you will need to create a DNS record for autodiscover.litwareinc.com that resolves to the fully qualified domain name of your Exchange Server (for example, atl-exchange-001.litwareinc.com).
  
If you are integrating Skype for Business Server with Exchange Online, your next steps are in [Configure integration between on-premises Skype for Business Server and Outlook Web App](../../deploy/integrate-with-exchange-server/outlook-web-app.md), otherwise see [Integrate Skype for Business Server with Exchange Server](../../deploy/integrate-with-exchange-server/integrate-with-exchange-server.md).
  
## Feature support
<a name="feature_support"> </a>

>[!Important]
> Skype for Business Online was retired on July 31, 2021. The Exchange integrations listed below that include the service are no longer supported.

The following table details the features supported under various combinations of online or on premises for Exchange and Skype for Business.
  
|&nbsp;|Exchange 2016/2013/2010 (on premises) + Skype for Business Server (on premises)|Exchange Online + Skype for Business Server (on premises)**|**Exchange 2010 (on premises) + Skype for Business Online|Exchange 2016/2013(on premises) + Skype for Business Online**|**Exchange Online + Skype for Business Online|
|:-----|:-----|:-----|:-----|:-----|:-----|
|Presence in Outlook   |Y   |Y   |Y   |Y   |Y   |
|Respond via IM, PSTN Call, Skype Call or Video Call from an Outlook email   |Y   |Y   |Y   |Y   |Y   |
|Schedule and join online meetings through Outlook   |Y   |Y   |Y   |Y   |Y   |
|Presence in Outlook Web App   |Y   |Y   |N   |N   |Y   |
|Respond via IM, PSTN Call, Skype Call or Video Call from an OWA email   |Y   |Y   |N   |N   |Y   |
|Schedule and join online meetings through Outlook Web App   |Y   |Y   |N   |N   |Y   |
|IM/Presence in Mobile Clients   |Y   |Y   |Y   |Y   |Y   |
|Join online meetings in Mobile clients   |Y   |Y   |Y   |Y   |Y   |
|Publish status based on Outlook calendar free/busy information   |Y   |Y   |Y   |Y   |Y   |
|Contact List (via Unified Contact Store)   |Y (need Exchange 2016/2013)   |Y   |N   |N   |Y   |
|High-resolution Contact Photo (Requires Lync 2013 or Skype for Business clients at a minimum. Not supported for LWA, mobile apps, Lync 2010, Lync for Mac, and other older clients.)   |Y (need Exchange 2016/2013)   |Y   |N   |Y   |Y   |
|Meeting delegation   |Y   |Y   |Y   |Y   |Y   |
|Missed Conversations history and Call Logs are written to user's exchange mailbox   |Y   |Y   |Y   |Y   |Y   |
|Archiving Content (IM and Meeting) in Exchange   |Y (need Exchange 2016/2013)   |Y   |N   |N   |Y   |
|Search archived content   |Y (need Exchange 2016/2013)   |Y   |N   |N   |Y   |
|Exchange UM Voice Mail   |Y   |Y   |N   |N   |N   |
|Server Side Conversation History   |Y   |Y   |N   |Y   |Y   |

> [!NOTE]
> There is a Cloud Voicemail service which is supported for Skype for Business Online, Skype for Business Server 2019, Skype for Business Server 2015, and Lync Server 2013.
> 

## See also
<a name="feature_support"> </a>

[Configure integration between on-premises Skype for Business Server and Outlook Web App](../../deploy/integrate-with-exchange-server/outlook-web-app.md)
  
[Configure OAuth between Skype for Business Online and Exchange on premises](../../deploy/integrate-with-exchange-server/oauth-with-online-and-on-premises.md)

[Integrate Skype for Business Server with Exchange Server](../../deploy/integrate-with-exchange-server/integrate-with-exchange-server.md)
  
[How to integrate Exchange Server 2013 with Lync Server 2013, Skype for Business Online, or a Lync Server 2013 hybrid deployment](/previous-versions/office/lync-server-2013/lync-server-2013-configuring-hybrid-deployments)
  
[Configure partner applications in Skype for Business Server and Microsoft Exchange Server](../../deploy/integrate-with-exchange-server/configure-partner-applications.md)