---
title: Discover and consume catalog assets
linkTitle: Discover and consume catalog assets
weight: 20
date: 2019-12-16T00:00:00.000Z
description: "Learn how to discover and consume assets in AMPLIFY Unified
  Catalog. Assets can be REST APIs, SOAP APIs, other non-REST APIs (for example,
  gRPC, Avro, graphQL), MFT services, and custom catalog assets. "
---

## Find a catalog asset

Start by searching the Unified Catalog to find the asset to use. By default, catalog assets are sorted by the most recently items published or updated first. Those assets are marked with **New** or **Updated** labels at the top of the list.  

To find a catalog assets you can:

* Search by **"name"** or **"tags"**: Type a keyword into the search bar. The result returns all catalog assets that have the search term either in the name or the tags.
* Use the **Filter** menu to filter the results by asset type (for example, API, MFT, CUSTOM) or subtype (for example, swagger2, OAS, WSDL, protobuf).

Watch the animation to learn how to search and filter in Unified Catalog.  

![find demo](/Images/central/catalog/find_demo.gif)

## Subscribe to an API

The provider of the API asset can configure when a subscription is required, and the metadata to be provided for the subscription. In this case, before using the API, you must subscribe to request access to use it. APIs that are protected with a client authentication policy in AMPLIFY Central will require an app with valid credentials to subscribe. The **Subscribe** button is not displayed for APIs that are not protected with an API Key.

Before you start, you will need to create an application and secure it with an [API Key](/docs/central/quickstart/), or [JWT](/docs/central/secure_api_jwt/), or [OAuth](/docs/central/feauth_oauth/).

Follow these steps to subscribe to an API asset:

1. Select **Catalog** in the left navigation bar to open the **Explore Catalog** submenu.
2. Click an API asset in the list to see a detailed view of its description, test methods, and subscriptions.
3. Click **Subscribe** in the top right corner.
4. On the dialog box enter a name for the subscription and select an option from the **Application** list.
  
    AMPLIFY Central administrators can subscribe an application on behalf of API consumers. In this case, they will be required to select the **Team** the API consumer belongs to.

5. Click **Subscribe**.

Watch the animation to learn how to subscribe to an API asset.

![subscribe demo](/Images/central/catalog/subscribe_demo.gif)

When subscription requests to an API are set to be manually approved by the API provider, a **Request access** button is shown. After you click to request access, a message is displayed to inform that the subscription request has been submitted and is awaiting approval.

### Promote an API to Integration Builder

If you are leveraging the AMPLIFY iPaaS to create integrations between different applications, you can promote an API from the Unified Catalog to Integration Builder as a custom connector with a click of a button. This saves you the trouble of exporting the swagger file and manually import it in Integration Builder. To lean more, see [AMPLIFY Integration Builder](https://docs.axway.com/bundle/AMPLIFY_Integration_Builder_allOS_en/page/amplify_integration_builder.html).

You can promote an API published in the Unified Catalog to Integration Builder as a custom connector. Before you start, ensure that you have access to Integration Builder, and that have your API secured, as well as a valid subscription. For details, see [Subscribe to an API](#subscribe-to-an-api).

To promote an API as a connector template:

1. Select the API from the Unified Catalog and click **Export to Integration Builder**.
2. Enter a **Name** for the template, and the **Organization** and **User** secrets for the Integration Builder environment where you want to create the connector template.
3. Click **Export**.

{{< alert title="Tip" color="primary" >}}You can find the **Organization** and **User** secrets in Integration Builder, on the bottom left corner of the menu.{{< /alert >}}

Now you can navigate to Integration Builder and see the connector template definition.

Watch the animation to learn how to perform this task.

![export demo](/Images/central/catalog_export.gif)

### Authenticate an exported API from Unified Catalog in Integration Builder

Watch this short video to learn how to configure and authorize an API that has been promoted from AMPLIFY Unified Catalog into Integration Builder.

{{< youtube tGNXQo-1frE >}}

## Download a Catalog asset

Follow these steps to download an asset from the catalog:

1. Select **Catalog** in the left navigation bar to open the **Explore Catalog** submenu.
2. Click on the asset to download from the list to see a detailed view of its description.
3. Next to the catalog name, click the **Download specification** button (hover over the icon to see it) from the available buttons.
4. The catalog asset is downloaded in the format specified in its specification.

Watch the animation to learn how to download a catalog asset.

![download demo](/Images/central/catalog/download_demo.gif)
