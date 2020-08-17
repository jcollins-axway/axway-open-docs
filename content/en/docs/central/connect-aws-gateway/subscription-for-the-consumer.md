---
title: Manage subscription workflow
linkTitle: Manage subscription workflow
draft: false
weight: 60
description: A subscription provides the consumer, or subscriber, with the
  required security, quota and endpoint materials to correctly consume the API.
---
## Subscription workflow

1. An administrator creates one or more usage plans on AWS API Gateway that provides the necessary security feature (API key / authorizer) and quota, if needed.
2. An administrator adds associated API stages to the usage plan(s).
3. A consumer initiates the subscription in AMPLIFY Central:

   1. Open an AMPLIFY Catalog item.
   2. Click **Subscribe**.
   3. Select a Team and usage plan for which you wish to subscribe. **WARNING**: The subscription will fail if you select a usage plan for which no API stages have been added. For additional information, see [Manage AMPLIFY Catalog subscriptions](/docs/catalog/manage_subscriptions/index.html).
4. The Discovery Agent receives the subscription event:

   Subscription status: **Subscribing...**

   1. Associate the API to the selected application.
   2. Add a tag `Subscriptions-<subscriptionID from AMPLIFY Central>` with the value `<apiId>-<Apistage>` on the usage plan.
   3. Send back the subscription status.

   Subscription status: **Active**

   * If failure, subscription status: **Subscription failed**. Refer to the Discovery Agent log for more information. You can delete the subscription and start again from Step 2.
5. The subscriber consumes the API:

   * The API can be consumed once the subscription details are received.

{{< alert title="Note" color="primary" >}}Depending on the poll interval settings for the Discovery Agent, it will take a little time from when the user subscribes an API to a usage plan until AMPLIFY Central shows the subscription state of **Active**. This is because of the time it takes to discover the change on API Manager and send events back and forth between API Manager and AMPLIFY Central.{{< /alert >}}

## Unsubscribe workflow

1. A consumer initiates unsubscribe:

   1. Open the AMPLIFY Catalog and navigate to the **Subscription** tab.
   2. Unsubscribe from the active subscription.  For additional information, see [Manage AMPLIFY Catalog subscriptions](/docs/catalog/manage_subscriptions/index.html).

2. The Discovery Agent receives the Unsubscribe event:

   * The `subscriptions-<subscriptionID from AMPLIFY Central>` is removed from the usage plan.
   * Initiates an unsubscribe to AMPLIFY Central for that Catalog item.
   * The subscription status is set to **Unsubscribed**.

## Impact of subscription approval mode on subscription workflow

The configuration setting for central.subscriptions.approvalmode will affect the flow of getting a subscription approved. Allowed settings are **manual**, **auto**, and **webhook**. Each of these are detailed below.

### Manual approval mode

This is the default setting. In manual approval mode, the subscription approval flow is as follows:

1. A consumer in AMPLIFY Central clicks on **Subscribe**.
2. The subscription status moves to **Waiting for approval...**.
3. The subscription remains in this state until a user with appropriate permissions on AMPLIFY Central locates the subscription and clicks **Approve**.
4. The subscription status moves to  **Subscribing**.
5. The Discovery Agent receives the event and sets the status to **Active**, or **Subscribe failed** if there is a failure to subscribe.

### Auto approval mode

In auto approval mode, the subscription approval flow is as described at the top of this page:

1. A consumer in AMPLIFY Central clicks on **Subscribe**.
2. The subscription status moves immediately to **Subscribing...**.
3. The Discovery Agent receives the event and sets the status to **Active**, or **Subscribe failed** if there is a failure to subscribe.

### Webhook approval mode

In webhook approval mode, the Discovery Agent must be configured with a webhook url, and any webhook headers and authentication secret that the webhook needs. Within the webhook, many things are possible. For example, the webhook could generate an email to notify someone that a subscription is awaiting approval. Or, the webhook could do the subscription approval. Assuming that the webhook is all correctly configured and coded, the subscription approval flow is as follows:

1. A consumer in AMPLIFY Central clicks on **Subscribe**.
2. The subscription status moves to **Waiting for approval...**.
3. The webhook is notified of the event.
4. The subscription remains in this state until the webhook moves the subscription to **Approved**, or a user with appropriate permissions on AMPLIFY Central locates the subscription and clicks **Approve**.
5. The subscription status moves to  **Subscribing**.
6. The Discovery Agent receives the event and sets the status to **Active**, or **Subscribe failed** if there is a failure to subscribe.

{{< alert title="Note" color="primary" >}}Depending on the poll interval settings for the Discovery Agent, it will take a little time from when the user unsubscribes an API until AMPLIFY Central shows the subscription state of **Unsubscribed**. This is because of the time it takes to discover the change on API Manager and send events back and forth between API Manager and AMPLIFY Central.{{< /alert >}}

## Subscription failures

The agent might mark a subscription as **Failed to subscribe** or **Failed to unsubscribe** for one of several reasons:

1. The API on API Gateway Manager is unpublished.
2. No usage plans have been created on AWS API Gateway.
3. On AWS API Gateway, usage plans have been created but no API stages have been added to the plan for the chosen subscription's API.
4. On AWS API Gateway, no API keys have been added to the subscription's chosen usage plan.
5. The agent fails to communicate with AWS API Gateway.
