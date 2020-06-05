---
title: Connect API Manager
linkTitle: Connect API Manager
no_list: true
draft: false
weight: 130
description: >
  Understand why you would connect Axway API Manager to AMPLIFY Central. Learn
  how you can publish to the AMPLIFY Catalog from your API Management System in
  order to obtain a global view of your APIS and present this Catalog to your
  consumers.

  Learn how you can collect the traffic of all your gateways and see it in a single place in AMPLIFY Central Observability.
---
## What is Axway API Manager connected?

We will show you how to connect your API Management system (v7.6.2 or above) to AMPLIFY Central by using two agents: Discovery and Traceability. These two agents will help you to represent and expose in AMPLIFY Central your API Management eco-system:

* Create an API Gateway environment in AMPLIFY Central that represent your real API Management eco-system.
* Detect published API using the Discovery Agent. The Discovery Agent discover the API from API Manager and makes them available in AMPLIFY Central. An API Service in Central is created to reference the API from API Manager and optionally, you can also ask the agent to publish it to the AMPLIFY Catalog for allowing your consumer to discover it. 
* Consumer subscription management using the Discovery agent. When a consumer subscribe/unsubscribe to a Catalog asset, the discovery agent keeps track of the changes and maintain the API Management system accordingly.  
* Filter the Axway API Gateway logs using the traceability agent. The traceability agent uses the discovered API to filter API Gateway events to extract the transaction information and sent these into AMPLIFY platform observability module.

### Discovery Agent

The Discovery Agent is used to discover new published API. The Discovery Agent pushes both REST and SOAP APIs definition to AMPLIFY Central.

The Discovery Agent discovers APIs that have PassTrough / API Key / OAuth security only.

The related APIs are published to AMPLIFY Central in either disconnected mode (catalog item publication) or connected mode (Environment / API Service publication and optionally as Catalog item). For additional information, see [Discovery Agent](/docs/central/connect-api-manager/deploy-your-agents/#discovery-agent).

![Service Discovery](/Images/central/ServiceDiscoveryAPIM.png)

### Traceability Agent

The Traceability Agent sends log information about APIs that have been discovered and published to AMPLIFY Central.

## Pre-requisites

* An Axway AMPLIFY Central subscription in the AMPLIFY™ platform. See [Get started with AMPLIFY Central](https://docs.axway.com/bundle/axway-open-docs/page/docs/central/quickstart/index.html).
* An AMPLIFY Central Service Account. See [Manage an API proxy using AMPLIFY CLI](/docs/central/cli_proxy_flow/)

## System requirements

* Axway API Manager / Axway API Gateway versions 7.6.2 SPx, 7.7 SPx or 7.8, up and running using a Linux installation (CentOS 6.x, 7.x, 8.x,  Oracle Linux 6.x, 7.x, Red Hat Enterprise Linux 6.x, 7.x, 8.x, SUSE Linux Enterprise Server 11.x, 12.x)
* The linux machine where API Manager & API Gateway are running should be accessible and have `sudo` rights to run the Agents.

## Connect Axway API Manager to AMPLIFY Central quickstart

1. Generate a key pair.

   * Create a new Service Account user in AMPLIFY Central using the key pair. See [Manage an API proxy using AMPLIFY CLI](/docs/central/cli_getstarted/).
2. Configure an environment in [AMPLIFY Central using the CLI](/docs/central/cli_environments/) or [Add your environment to AMPLIFY Central using the UI](/docs/central/mesh_management/add_env/#add-your-environment-to-amplify-central).
3. [Update default Discovery Agent configuration file](/docs/central/connect-api-manager/deploy-your-agents/#discovery-agent) *

   1. Move key files to a keys directory.
   2. Pull the latest binary of the Discovery Agent.
   3. Start the Discovery Agent.
4. [Update default Traceability Agent configuration file](/docs/central/connect-api-manager/deploy-your-agents/#traceability-agent) *

   1. Move key files to a keys directory.
   2. Pull the latest binary of the Traceability Agent.
   3. Start the Traceability Agent.

\* If you need assistance setting up the configuration files, please contact your Sales Account Manager, or open a support ticket on support.axway.com.