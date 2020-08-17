---
title: Delete an API Stage or API
linkTitle: Delete an API Stage or API
draft: false
weight: 50
description: Understand the implications of deleting an API stage or an API in
  AWS API Gateway in the centralMode (publishToEnvironmentAndCatalog), especially as it relates to subscriptions and
  your AMPLIFY Central Catalog items.
---

## Deleting a stage in publishToEnvironmentAndCatalog mode

Assume you have an API stage in AWS API Gateway that has been previously discovered by the agent and pushed to AMPLIFY Central as a Catalog item. Now you choose to delete that stage using the AWS API Gateway console. If the agent's mode is set to `publishToEnvironmentAndCatalog`, then:

* The agent first checks if there are any active subscriptions for that Catalog item. If there are any, then the agent automatically unsubscribes each of them.
* The agent then removes the Catalog item for that stage in AMPLIFY Central.
* Finally, the agent removes the underlying ConsumerInstance in the AMPLIFY Central environment.

## Deleting an API

Deleting an API in AWS API Gateway in `publishToEnvironmentAndCatalog` mode behaves as follows. During the deleting of the API, each stage of the API is deleted in turn, and the AMPLIFY Central subscriptions and Catalog items are handled according to the descriptions above for deleting a stage, depending on the centralMode.