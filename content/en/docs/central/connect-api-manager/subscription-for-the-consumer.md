---
title: Manage subscription workflow
linkTitle: Manage subscription workflow
draft: false
weight: 90
description: A subscription provides the consumer, or subscriber, with the
  required security, quota and endpoint materials to correctly consume the API.
---
## Subscription workflow

1. An administrator creates one or more applications on Axway API Manager and provides the necessary security feature (API key / OAuth...) and quota, if needed:

   * Add a custom field to the application to track the AMPLIFY Central subscription. Refer to `<API_Gateway_install_dir>/apigateway/webapps//apiportal/vordel/apiportal/app/app.config file` in the **customPropertiesConfig** section. For more details, see [Customize API Manager](https://docs.axway.com/bundle/axway-open-docs/page/docs/apim_administration/apimgr_admin/api_mgmt_custom/index.html).

       Sample application:

     ```
     customPropertiesConfig: {
             user: {
                 // custom properties...
             },
             organization: {
                 // custom properties...
             },
             application: {
                 subscriptions: {
                     label: 'Subscriptions'
                 },
             },
             api: {
                 // custom properties...
             }
         }
     ```
2. An administrator adds API access on the application(s) for each API they wish to subscribe to.
3. A consumer initiates the subscription in AMPLIFY Central:

   1. Open an AMPLIFY Catalog item.
   2. Click **Subscribe**.
   3. Select the Team and API Manager Application name (created in Step 1) for which you want to subscribe. **WARNING**: The subscription will fail if you select an application for which no APIs have been given access. For additional information, see [Manage AMPLIFY Catalog subscriptions.](https://docs.axway.com/bundle/axway-open-docs/page/docs/catalog/manage_subscriptions/index.html)

4. The Discovery Agent receives the subscription event:

   * If subscription status: **Subscribing...**

      * Associate the API to the selected application.
      * Send back the subscription status.

   * If subscription status: **Active**

      * Subscription ID is automatically added to the **Custom** field of the application.
      * If failure, subscription status: **Subscription failed**. Refer to the Discovery Agent log for more information. You can delete the subscription and start again from Step 2.

5. The subscriber consumes the API:

   * The API can be consumed once the subscription details are received.

{{< alert title="Note" color="primary" >}}Depending on the poll interval settings for the Discovery Agent, it will take a little time from when the user subscribes an API to an application until AMPLIFY Central shows the subscription state of **Active**. This is because of the time it takes to discover the change on API Manager and send events back and forth between API Manager and AMPLIFY Central.{{< /alert >}}

{{< alert title="Note" color="primary" >}}If the FrontEnd API on API Manager corresponding to the Catalog item is set to **unpublished** at the time the subscription is initiated, the Discovery Agent will receive the event, but will not allow the subscription to be completed. Instead, it will send back a subscription status of **Subscribe failed**.{{< /alert >}}

{{< alert title="Note" color="primary" >}}The API Manager application and the API must be in the same organization. Otherwise,  an error message is displayed in the Discovery Agent log.{{< /alert >}}

**Workaround**: You can grant the API access to the organization where the application belongs:

1. In the UI, select the API.
2. Expand **Manage selected**.
3. Select **Grant access**.

## Unsubscribe workflow

1. A consumer initiates unsubscribe:

   1. Open the AMPLIFY Catalog and navigate to the **Subscription** tab.
   2. Unsubscribe from the active subscription. For additional information, see [Manage AMPLIFY Catalog subscriptions](https://docs.axway.com/bundle/axway-open-docs/page/docs/catalog/manage_subscriptions/index.html).

2. The Discovery Agent receives the Unsubscribe event:

   * The subscription ID is removed from the application's Custom field.

## Impact on subscription when unpublishing an API

1. In API Manager, assume there is a FrontEnd API that is published, has been discovered by the Discovery Agent, and has an active subscription to it in AMPLIFY Central.
2. A user in API Manager unpublishes that API
3. The Discovery Agent discovers the change and:

   * Initiates an unsubscribe to AMPLIFY Central for that Catalog item.
   * The subscription ID is removed from the application's Custom field.
   * The subscription status is set to **Unsubscribed**.

{{< alert title="Note" color="primary" >}}Depending on the poll interval settings for the Discovery Agent, it will take a little time from when the user unsubscribes an API until AMPLIFY Central shows the subscription state of **Unsubscribed**. This is because of the time it takes to discover the change on API Manager and send events back and forth between API Manager and AMPLIFY Central.{{< /alert >}}

## Impact of subscription approval mode on subscription workflow

The configuration setting for central.subscriptions.approvalmode will affect the flow of getting a subscription approved. Allowed settings are **manual**, **auto**, and **webhook**. Each of these are detailed below

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

In webhook approval mode, the Discovery Agent must be configured with a webhook url, and any webhook headers and authentication secret that the webhook needs. Within the webhook, many things are possible. For example, the webhook could generate an email to notify someone that a subscription is awaiting approval. Or, the webhook could do the subscription approval. Assuming that the webhook is correctly configured and coded, the subscription approval flow is as follows:

1. A consumer in AMPLIFY Central clicks on **Subscribe**.
2. The subscription status moves to **Waiting for approval...**.
3. The webhook is notified of the event.
4. The subscription remains in this state until the webhook moves the subscription to **Approved**, or a user with appropriate permissions on AMPLIFY Central locates the subscription and clicks **Approve**.
5. The subscription status moves to  **Subscribing**.
6. The Discovery Agent receives the event and sets the status to **Active**, or **Subscribe failed** if there is a failure to subscribe.

## Subscription failures

The agent might mark a subscription as **Failed to subscribe** or **Failed to unsubscribe** for one of several reasons:

|   | Failure Description                                                                                                                                                                    | Remediation                                                                                                                                                                                     |
|---|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1 | The API on API Gateway Manager is unpublished.                                                                                                                                         | Under the API section in API Manager > Manage the Frontend API: Publish the desired API.                                                                                                      |
| 2 | On API Gateway Manager, the organization to which the application chosen for the subscription has not given API Access to the API, or the access has been given but has been disabled. | Under the Clients section in API Manager > Organizations: Verify that the ORG has "Enabled" toggled correctly (under General) and that the API has been added (under API Access).                 |
| 3 | On API Gateway Manager, the application chosen for the subscription has not given API Access to the API, or the access has been given but has been disabled.                           | Under the Clients section in API Manager > Applications > Application Tab: Verify that the APP has "Enabled" toggled correctly (under General) and that the API has been added (under API Access). |
| 4 | On API Gateway Manager, the application chosen for the subscription has not set up any authentication.                                                                                 | Under the Clients section in API Manager > Applications > Authentication Tab: Verify that the APP has the appropriate authentication type setup.                                                   |
| 5 | On API Gateway Manager, the application chosen for the subscription does not match the inbound security setting for the API.                                                           | Under the API section in API Manager > Manage the Frontend API, click the API > Inbound Tab: Verify that the API has the appropriate Inbound security selected.                               |
| 6 | The agent fails to communicate to API Gateway Manager.                                                                                                                                 | Check your internet connection. API Gateway Manager requires an HTTPS connection.                                                                                                               |

For additional information, see [Manage AMPLIFY Catalog subscriptions](https://docs.axway.com/bundle/axway-open-docs/page/docs/catalog/manage_subscriptions/index.html).
