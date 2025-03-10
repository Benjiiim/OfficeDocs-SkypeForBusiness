---
title: "What are dial plans?"
author: CarolynRowe
ms.author: crowe
manager: serdars
ms.reviewer: mikedav, roykuntz
ms.date: 11/28/2017
ms.topic: article
ms.tgt.pltfrm: cloud
ms.service: msteams
search.appverid: MET150
ms.collection: 
  - M365-voice
  - m365initiative-voice
  - highpri
audience: Admin
appliesto: 
  - Skype for Business
  - Microsoft Teams
ms.localizationpriority: medium
f1.keywords:
- CSH
ms.custom: 
  - ms.teamsadmincenter.voice.dialplans.overview
  - Calling Plans
description: "Learn what type of dial calling plans (PSTN Calling dial plans) are available with Teams and how to choose one for your organization.  "
---

# What are dial plans?

A dial plan is a named set of normalization rules that translate dialed phone numbers by an individual user into an alternate format (typically E.164) for purposes of call authorization and voice routing.

A dial plan consists of one or more normalization rules that define how phone numbers expressed in various formats are translated to an alternate format. The same dial string may be interpreted and translated differently in different dial plans, so depending on which dial plan is assigned to a given user, the same dialed number may be translated and routed differently. There can be a maximum of 1,000 tenant dial plans.

See [Create and manage dial plans](create-and-manage-dial-plans.md) to create and manage tenant dial plans.

## Tenant dial plan scope

A dial plan's scope determines the hierarchical level at which the dial plan can be applied. Clients get the appropriate dial plan through provisioning settings that are automatically provided when users sign in to Teams. As an admin, you can manage and assign dial plan scope levels by using the Microsoft Teams admin center or Remote PowerShell.

In Teams, there are two types of dial plans: service-scoped and tenant-scoped (which is for your organization). A service-scoped dial plan is defined for every country or region where Phone System is available. Each user is automatically assigned the service country dial plan that matches the usage location assigned to the user. You can't change the service country dial plan, but you can create tenant scoped dial plans, which augment the service country dial plan. As clients are provisioned, they obtain an "effective dial plan," which is a combination of the service country dial plan and the appropriately scoped tenant dial plan. Therefore, it's not necessary to define all normalization rules in tenant dial plans as they might already exist in the service country dial plan.

Tenant dial plans can be further broken into two scopes - tenant-scope or user-scope. If a tenant defines and assigns a user-scoped dial plan, that user will be provisioned with an effective dial plan of the user's service country dial plan and the assigned user dial plan. If a tenant defines a tenant-scoped dial plan but doesn't assign a user-scoped dial plan, then that user will be provisioned with an effective dial plan of the user's service country dial plan and the tenant dial plan.

The following is the inheritance model of dial plans in Teams.

![How dial plans are inherited in Teams.](media/b2744f33-ebbd-4c23-bfba-1747312ab178.png)

The following are the possible effective dial plans:

 **Service Country** If no tenant scoped dial plan is defined and no tenant user scoped dial plan is assigned to the provisioned user, the user will receive an effective dial plan mapped to the service country associated with their usage location.

 **Tenant Global - Service Country** If a tenant user dial plan is defined but not assigned to a user, the provisioned user will receive an effective dial plan consisting of a merged tenant dial plan and the service country dial plan associated with their usage location.

 **Tenant User - Service Country** If a tenant user dial plan is defined and assigned to a user, the provisioned user will receive an effective dial plan consisting of the merged tenant user dial plan and the service country dial plan associated with their usage location.

See [Create and manage dial plans](create-and-manage-dial-plans.md) to create your tenant dial plans.

> [!NOTE]
> In the scenario where no dial plan normalization rules apply to a dialed number, the dialed string is still normalized to prepend "+CC" where CC is the country code of the dialing user's usage location. This applies to Calling Plans, Direct Routing and PSTN Conference dial-out scenarios. Additionally, if a tenant dial plan normalization rule results in a number that does not start with "+", the calling service will attempt to normalize the number received from the Teams client based on the tenant dial plan, and if not matched, on the region dial plan. To avoid double normalization, it's recommended that Direct Routing customers normalize numbers to include a + and then remove the + using Trunk Translation rules. 

## Planning for tenant dial plans

To plan custom dial plans, follow these steps:

- **Step 1** Decide whether a custom dial plan is needed to enhance the user dialing experience. Typically, the need for one would be to support non-E.164 dialing, such as extensions or abbreviated national dialing.

- **Step 2** Determine whether tenant global or tenant user scoped dial plans are needed, or both. User scoped dial plans are needed if users have different local dialing requirements.

- **Step 3** Identify valid number patterns for each required dial plan. Only the number patterns that are not defined in the service level country dial plans are required.

- **Step 4** Develop an organization-wide scheme for naming dial plans. Adopting a standard naming scheme assures consistency across an organization and makes maintenance and updates easier.


## Creating your new dial plan

When you create a new dial plan, you must put in the information that is required.

### Name and simple name

For user dial plans, you should specify a descriptive name that identifies the users to which the dial plan will be assigned. The dial plan Simple Name is pre-populated with a string that is derived from the dial plan name. The Simple Name field is editable, which enables you to create a more descriptive naming convention for your dial plans. The Simple Name value cannot be empty and must be unique. A best practice is to develop a naming convention for your entire organization and then use this convention consistently across all sites and users.

### Description

We recommend that you type the common, recognizable name of the geographic location or group of users to which the corresponding dial plan applies.

### External access prefix
<a name="bkexternalprefix"> </a>

You can specify an external access prefix of up to four characters (#, *, and 0-9) if users need to dial one or more additional leading digits (for example, 9) to get an external line.

> [!NOTE]
> If you specify an external access prefix, you don't need to create an additional normalization rule to accommodate the prefix.

See [Create and manage dial plans](create-and-manage-dial-plans.md) to create your tenant dial plans.

## Normalization rules
<a name="bknormalizationrule"> </a>

Normalization rules define how phone numbers expressed in various formats are to be translated. The same number string may be interpreted and translated differently, depending on the locale from which it is dialed. Normalization rules may be necessary if users need to be able to dial abbreviated internal or external numbers.

One or more normalization rules must be assigned to the dial plan. Normalization rules are matched from top to bottom, so the order in which they appear in a tenant dial plan is important. For example, if a tenant dial plan has 10 normalization rules, the dialed number matching logic will be tried starting with the first normalization rule, if there isn't a match then the second, and so forth. If a match is made, that rule is used and there is no effort to match any other rules that are defined. There can be a maximum of 50 normalization rules in a given tenant dial plan.

### Determining the required normalization rules

Because any tenant dial plan is effectively merged with a given user's service country dial plan, it is likely that the service country dial plan's normalization rules need to be evaluated in order to determine which tenant dial plan normalization rules are needed. The **Get-CsEffectiveTenantDialPlan** cmdlet can be used for this purpose. The cmdlet takes the user's identity as the input parameter and will return all normalization rules that are applicable to the user.

### Creating normalization rules
<a name="createrule"> </a>

Normalization rules use .NET Framework regular expressions to specify numeric match patterns that the server uses to translate dial strings to E.164 format. Normalization rules can be created by specifying the regular expression for the match and the translation to be done when a match is found. When you finish, you can enter a test number to verify that the normalization rule works as expected.

For details about using .NET Framework regular expressions, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expressions).

See [Create and manage dial plans](create-and-manage-dial-plans.md) to create and manage normalization rules for your tenant dial plans.

> [!NOTE]
> Normalization rules with the first token as optional are currently not supported on 3pip devices (for example, Polycom VVX 601 model). If you want to apply normalization rules with optionality on 3pip devices, you should create two normalization rules instead of one. For example, the rule ^0?(999)$ should be replaced by the following two rules: (999)$ (Translation:$1) and ^0(999)$ (Translation:$1).


### Sample normalization rules

The following table shows sample normalization rules that are written as .NET Framework regular expressions. The samples are examples only and are not meant to be a prescriptive reference for creating your own normalization rules.

<a name="regularexpression"> </a>
**Normalization rules using .NET Framework regular expressions**

| Rule name<br/> | Description<br/> | Number pattern<br/> | Translation<br/> | Example<br/> |
|:-----|:-----|:-----|:-----|:-----|
|4digitExtension  <br/> |Translates 4-digit extensions.  <br/> |^(\\d{4})$  <br/> |+1425555$1  <br/> |0100 is translated to +14255550100  <br/> |
|5digitExtension  <br/> |Translates 5-digit extensions.  <br/> |^5(\\d{4})$  <br/> |+1425555$1  <br/> |50100 is translated to +14255550100  <br/> |
|7digitcallingRedmond  <br/> |Translates 7-digit numbers to Redmond local numbers.  <br/> |^(\\d{7})$  <br/> |+1425$1  <br/> |5550100 is translated to +14255550100  <br/>|
|RedmondOperator  <br/> |Translates 0 to Redmond Operator.  <br/> |^0$  <br/> |+14255550100  <br/> |0 is translated to +14255550100  <br/> |
|RedmondSitePrefix  <br/> |Translates numbers with on-net prefix (6) and Redmond site code (222).  <br/> |^6222(\\d{4})$  <br/> |+1425555$1  <br/> |62220100 is translated to +14255550100  <br/> |
|5digitRange  <br/> |Translates 5-digit extensions starting with the digit range between 3-7 inclusive.  <br/> |^([3-7]\\d{4})$  <br/> |+142555$1 <br/> |54567 is translated to +14255554567  <br/> |
|PrefixAdded  <br/> |Adds a country prefix in front of a 9 digit number with restrictions on the first and third digits.  <br/> |^([2-9]\\d\\d[2-9]\\d{6})$  <br/> |1$1  <br/> |4255554567 is translated to 14255554567  <br/> |
|NoTranslation  <br/> |Match 5 digits but no translation.  <br/> |^(\\d{5})$  <br/> |$1  <br/> |34567 is translated to 34567  <br/> |

 **Redmond dial plan based on normalization rules shown above.**

 The following table illustrates a sample dial plan for Redmond, Washington, United States, based on the normalization rules shown in the previous table.

| Redmond dial plan<br/> |
|:-----------------------|                                                                                                                      
| 5digitExtension <br/> |                                                                                                                                    
| 7digitcallingRedmond <br/> |
| RedmondSitePrefix <br/> |
| RedmondOperator <br/> |

> [!NOTE]
> The normalization rules names shown in the preceding table don't include spaces, but this is a matter of choice. The first name in the table, for example, could have been written "5 digit extension" or "5-digit Extension" and still be valid.

## Related topics

[Create and manage dial plans](create-and-manage-dial-plans.md)

[Different kinds of phone numbers used for Calling Plans](different-kinds-of-phone-numbers-used-for-calling-plans.md)

[Manage phone numbers for your organization](manage-phone-numbers-for-your-organization/manage-phone-numbers-for-your-organization.md)

[Emergency calling terms and conditions](emergency-calling-terms-and-conditions.md)

[Emergency Calling disclaimer label](https://github.com/MicrosoftDocs/OfficeDocs-SkypeForBusiness/blob/live/Teams/downloads/emergency-calling/emergency-calling-label-(en-us)-(v.1.0).zip?raw=true)
