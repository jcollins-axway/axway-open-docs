---
title: Unpublish an API
linkTitle: Unpublish an API
draft: false
weight: 60
description: Understand the implications of unpublishing your API in API Manager
  in the centralMode (publishToEnvironmentAndCatalog), especially as it relates to subscriptions.
---

## Unpublishing your API in publishToEnvironmentAndCatalog mode

Assume you have an API in API Manager that has been previously discovered by the agent and pushed to AMPLIFY Central as a Catalog item. Now you choose to unpublish that API in API Manager. If the agent's mode is set to `publishToEnvironmentAndCatalog`, then:

* The agent first checks if there are any active subscriptions for that Catalog item. If there are any, then the agent automatically unsubscribes each of them.
* The agent then removes the Catalog item for that API in AMPLIFY Central.
* Finally, the agent removes the underlying ConsumerInstance in the AMPLIFY Central environment.
