---
title: Allow list
description: Learn how to use the allowed list.
feature: Deliverability
topic: Content Management
role: User
level: Intermediate
exl-id: 70ab8f57-c132-4de1-847b-11f0ab14f422
---
# Allowed list {#allow-list}

It is possible to define a specific sending-safe list at the [sandbox](../administration/sandboxes.md) level, to have a safe environment for testing purpose.

For example, on a non-production instance, where mistakes can occur, the allowed list ensures you have no risk of sending out unwanted messages to your customers.

>[!NOTE]
>
>This feature is now available on production and non-production sandboxes.

The allowed list enables you to specify individual email addresses or domains that will be the only recipients or domains authorized to receive the emails you are sending from a specific sandbox. This can prevent you from sending emails accidentally to real customer addresses when you are in a testing environment.

>[!CAUTION]
>
>This feature only applies to the email channel.

## Enable the allowed list {#enable-allow-list}

<!--To enable the allowed list on a non-production sandbox, you need to update the general settings using the corresponding API end point in the Message Presets Service. Using this API, you can also disable the feature at any time.

You can update the allowed list before or after enabling the feature.-->

To enable the allowed list, follow the steps below.

1. Access the  **[!UICONTROL Channels]** > **[!UICONTROL Email configuration]** > **[!UICONTROL Allow list]** menu.

    ![](assets/allow-list-access.png)

1. Click **[!UICONTROL Edit]**.

    ![](assets/allow-list-edit.png)

1. Select **[!UICONTROL Enable allowed list]**.

    ![](assets/allow-list-enable.png)

1. Click **[!UICONTROL Save]**. The allowed list is enabled.

    ![](assets/allow-list-enabled.png)

The allowed list logic applies when the feature is enabled **and** if the allowed list is **not** empty. Learn more in [this section](#logic).

>[!NOTE]
>
>When enabled, the allowed list feature is honored when executing journeys, but also when testing messages with [proofs](../design/preview.md#send-proofs) and testing journeys using the [test mode](../building-journeys/testing-the-journey.md).

## Add entities to the allowed list {#add-entities}

To add new email addresses or domains to the allowed list for a specific sandbox, you must call the suppression API with the `ALLOWED` value for the `listType` attribute. For example:

![](assets/allow-list-api.png)

You can perform the **Add**, **Delete** and **Get** operations.

>[!NOTE]
>
>The allowed list can contain up to 1,000 entries.

Learn more on making API calls in the [Adobe Experience Platform APIs](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-guide.html){target="_blank"} reference documentation.

## Allowed list logic {#logic}

When the allowed list is **empty**, the allowed list logic is not applied. This means that you can send emails to any profiles, provided they are not on the [suppression list](suppression-list.md).

When the allowed list is **not empty**, the allowed list logic is applied:

* If an entity is **not on the allowed list**, and not on the suppression list, the corresponding recipient will not receive the email, the reason being **[!UICONTROL Not allowed]**.

* If an entity is **on the allowed list**, and not on the suppression list, the email can be sent to the corresponding recipient. However, if the entity is also on the [suppression list](suppression-list.md), the corresponding recipient will not receive the email, the reason being **[!UICONTROL Suppressed]**.

>[!NOTE]
>
>The profiles with **[!UICONTROL Not allowed]** status are excluded during the message sending process. Therefore, while the **Journey reports** will show these profiles as having moved through the journey ([Read Segment](../building-journeys/read-segment.md) and [Message](../building-journeys/journeys-message.md) activities), the **Email reports** will not include them in the **[!UICONTROL Sent]** metrics as they are filtered out prior to email sending.
>
>Learn more on the [Live Report](../reports/live-report.md) and [Global Report](../reports/global-report.md).

## Exclusion reporting {#reporting}

When this feature is enabled on a non-production sandbox, you can retrieve email addresses or domains that were excluded from a sending because they were not on the allowed list. To do this, you can use the [Adobe Experience Platform Query Service](https://experienceleague.adobe.com/docs/experience-platform/query/api/getting-started.html){target="_blank"} to make the API calls below.

To get the **number of emails** that were not sent because the recipients were not on the allowed list, use the following query:

```sql
SELECT count(distinct _id) from cjm_message_feedback_event_dataset WHERE
_experience.customerJourneyManagement.messageExecution.messageExecutionID = '<MESSAGE_EXECUTION_ID>' AND
_experience.customerJourneyManagement.messageDeliveryfeedback.feedbackStatus = 'exclude' AND
_experience.customerJourneyManagement.messageDeliveryfeedback.messageExclusion.reason = 'EmailNotAllowed'
```

To get the **list of email addresses** that were not sent because the recipients were not on the allowed list, use the following query:

```sql
SELECT distinct(_experience.customerJourneyManagement.emailChannelContext.address) from cjm_message_feedback_event_dataset WHERE
_experience.customerJourneyManagement.messageExecution.messageExecutionID IS NOT NULL AND
_experience.customerJourneyManagement.messageDeliveryfeedback.feedbackStatus = 'exclude' AND
_experience.customerJourneyManagement.messageDeliveryfeedback.messageExclusion.reason = 'EmailNotAllowed'
```
