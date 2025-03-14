---
title: 'Calling policies in Microsoft Teams'
author: CarolynRowe
ms.author: crowe
manager: serdars
ms.topic: conceptual
ms.service: msteams
ms.reviewer: jastark
ms.date: 04/12/2019
audience: admin
search.appverid: MET150
description: Learn how to create, modify, and add users to custom calling policies in Microsoft Teams, as well as various calling policy settings.
ms.localizationpriority: medium
ms.collection: 
  - M365-voice
f1.keywords:
- CSH
ms.custom: 
  - ms.teamsadmincenter.callingpolicies.overview
  - seo-marvel-apr2020
  - NewAdminCenter_Update
appliesto: 
  - Microsoft Teams
---

# Calling policies in Teams

In Microsoft Teams, calling policies control which calling and call forwarding features are available to users. Calling policies determine whether a user can make private calls, use call forwarding or simultaneous ringing to other users or external phone numbers, route calls to voicemail, send calls to call groups, use delegation for inbound and outbound calls, and so on.

You can use the global (Org-wide default) policy that's created automatically or create and assign custom policies.

## Create a custom calling policy

Follow these steps to create a custom calling policy.

1. In the left navigation of the Microsoft Teams admin center, go to **Voice** > **Calling policies**.
2. Select **Add**.
3. Turn on or turn off the features that you want to use in your calling policy.
    - For example, to control whether users can route inbound calls to voicemail, select **Enabled** or **User controlled**. To prevent routing to voicemail, select **Not enabled**.
4. Select **Save**.

## Edit a calling policy

Follow these steps to edit an existing calling policy.

1. In the left navigation of the Microsoft Teams admin center, select **Voice** > **Calling policies**.
2. Click next to the policy that you want to modify, and then select **Edit**.
3. Make the changes that you want, and then click **Save**.

## Assign a custom calling policy to users

[!INCLUDE [assign-policy](includes/assign-policy.md)]

## Calling policy settings

Here are the settings that you can configure for calling policies.

### Make private calls

This setting controls all calling capabilities in Teams. Turn this setting off to turn off all calling functionality in Teams.

### Cloud recording for calling

This setting controls whether users can record calls. This setting is off by default.

### Transcription

This setting controls whether the transcription of calls is available for your users. This setting is off by default.

### Call forwarding and simultaneous ringing to people in your organization

This setting controls whether incoming calls can be forwarded to other users or can ring another person in your organization at the same time. This setting is on by default.

### Call forwarding and simultaneous ringing to external phone numbers

This setting controls whether incoming calls can be forwarded to an external number or can ring an external number at the same time. This setting is on by default.

### Voicemail is available for routing inbound calls

This setting enables inbound calls to be sent to voicemail. The default setting is **User controlled**. Valid options are:

- **Enabled** Voicemail is always available for inbound calls.
- **Not enabled**  Voicemail is not available for inbound calls.
- **User controlled** Users can determine whether they want voicemail to be available.

### Inbound calls can be routed to call groups

This setting controls whether incoming calls can be forwarded to a call group. This setting is turned on by default.

### Delegation for inbound and outbound calls

This setting enables inbound calls to be routed to delegates, allowing delegates to make outbound calls on behalf of the users for whom they have delegated permissions. This setting is turned on by default. For more information, see [Share a phone line with a delegate](https://support.office.com/article/share-a-phone-line-with-a-delegate-16307929-a51f-43fc-8323-3b1bf115e5a8).

### Prevent toll bypass and send calls through the PSTN

Turning on this setting will send calls through the Public Switched Telephone Network (PSTN)and incur charges rather than sending them through the network and bypassing the tolls. This setting is off by default.

### Music on hold for PSTN calls

This setting allows you to turn on or turn off music on hold when a PSTN caller is placed on hold. It's turned on by default. This setting doesn't apply to call park and boss delegate features. Read more about how to [configure custom music](music-on-hold.md).

### Busy on busy when in a call

Busy on busy when in a call (also called "busy options") lets you configure how incoming calls are handled when a user is already in a call or conference or has a call placed on hold. New or incoming calls can be rejected with a busy signal or can be routed accordingly to the user's unanswered settings. Regardless of how their busy options are configured, users in a call or conference or those with a call on hold are not prevented from initiating new calls or conferences. This setting is set to **Not enabled** by default.

- **Not enabled** No busy option is enabled and new or incoming calls can still go to the user while the user is already in a call.
- **Enabled** New or incoming calls will be rejected with a busy signal.
- **Unanswered** The user's unanswered settings will be used, such as routing to voicemail or forwarding to another user.
- **User controlled** This option is not currently functional; if set, this will be read as setting the value to **Not enabled**.

### Web PSTN calling

This setting enables users to call PSTN numbers using the Teams web client. This setting is on by default.

### Real-time captions in Teams calls

This setting controls whether real-time captions in Teams calls are available for your users. This setting is turned on by default.

### Automatically answer incoming meeting invites

This setting controls whether incoming meeting invites are automatically answered. It's turned off by default. Keep in mind that this setting applies only to incoming meeting invites. It doesn't apply to other types of calls.

### SIP devices can be used for calls

This setting enables users to use a SIP device to make and receive calls. This setting is turned off by default.

### Open apps in browser for incoming PSTN calls

This setting controls whether apps are automatically opened in the browser for incoming PSTN calls to your users. This can be used to pass the phone number of an inbound caller to an app to find the associated customer record while the call is taking place. This setting is off by default.

If turned on, a link to the app will need to be given in the **URL to open apps in browser for incoming PSTN calls** box. You can use the {phone} placeholder to pass the phone number (in E.164 format) to the provided URL. Or, you can give a generic URL without any placeholder. This will simply launch the listed URL.

![Screenshot of Open apps in browser for incoming PSTN calls policy setting.](media/teams-open-apps-in-browser-pstn.png)

## Related articles

[Set-CsTeamsCallingPolicy](/powershell/module/skype/set-csteamscallingpolicy)

[Assign policies to your users in Teams](policy-assignment-overview.md)

[PSTN connectivity options](pstn-connectivity.md)

[Configure call settings for your users](user-call-settings.md)
