---
title: AMPLIFY Central overview
linkTitle: Overview
weight: 10
date: 2019-07-30
description: Deploy and secure your services in any environment (for example, cloud, on-premise, and so on) and govern your APIs through a central platform that allows you to integrate your services safely and easily with both internal and external consumers.
---

AMPLIFY Central is a governance platform that enables a self-service centralized API and microservices management across virtualized networks in any cloud platform or in a private datacenter. Its core features provide API security, monitoring, and traffic management independent of the implementation of the API.

The following diagram shows full lifecycle API management, and the role of AMPLIFY Central in governing, exposing, and monitoring API traffic flowing within your organization.

![full lifecycle API management](/Images/central/api_central_overview.png)

## API management

AMPLIFY Central capabilities for managing APIs include registering and deploying your APIs to test and production environments, securing the API, and monitoring the API usage and traffic.

### Register an API

To learn how to register an API as an API proxy in AMPLIFY Central, see [Get started with AMPLIFY Central](/docs/central/quickstart/#register-an-api).

### Secure an API

Apply policies to an API proxy to manage access to your API from client applications.

In [Get started with AMPLIFY Central](/docs/central/quickstart), you learned how to secure your API with a client authentication policy, using an API key as an example.

You can also secure your API with a JWT client authentication policy. To learn how to generate a JWT and how to apply a JWT client authentication policy to an API, see [Secure an API with JWT](/docs/central/secure_api_jwt/).

### Rate limit an API

Apply a rate limit policy to limit the usage of your API.

To learn how to to apply a rate limit configuration to your API, see [Rate limit an API](/docs/central/proxy_rate_limit/).

## DevOps integration

AMPLIFY Central supports your DevOps processes by providing an API and a CLI for you to automate your configuration and deployments.

You can create API proxies and deploy them to runtime groups using the AMPLIFY Central DevOps API and the AMPLIFY CLI. You can test the API proxies and view the traffic monitoring results through the AMPLIFY Central UI.

### What is DevOps?

DevOps can be defined as a combination of people, processes, and technology that increases your organization's ability to quickly deliver quality services to your customers.

* **People**: Developers, operations, product owners, testers, infrastructure engineers, and so on, all work together collaboratively as a team to build and deliver the service.
* **Process**: Continuous improvement of the process to deliver a quality service is key.
* **Technology**: Tools that automate the manual processes and integrate with other tools to make it easier and faster to develop, package, test, release, and deploy the service.

### AMPLIFY Central DevOps approach

Taking a DevOps approach to API management with AMPLIFY Central enables you to streamline the delivery of updates or new capabilities in your services to your customers and partners.

![AMPLIFY Central in your DevOps pipeline](/Images/central/devops.png)

AMPLIFY Central is a CI/CD native solution that you can easily snap in to your existing DevOps pipeline. Simplify and customize how you manage APIs across your organization by using the AMPLIFY CLI and the AMPLIFY Central DevOps API to manage your API proxies in AMPLIFY Central.

### AMPLIFY Central DevOps API

The AMPLIFY Central DevOps API is a DevOps-friendly REST API that you can use to create, update, deploy, and promote API proxies to AMPLIFY Central runtimes. It provides the following DevOps-friendly features:

* The APIs accept a YAML configuration file which represents the desired state of the API proxy.
    * YAML format is human-readable, easily diffed, and maintainable.
    * YAML configuration file can be versioned along with the code for the service in a version control system, such as Git.
* The APIs are coarse-grained, avoiding the need for complex ordered API invocations to multiple fine-grained APIs.
* The APIs use an update or insert (*upsert*) strategy. If the API proxy already exists it is updated, otherwise it is created.

Visit our [API documentation](https://d-api.docs.stoplight.io/).

#### DevOps authentication and authorization

To use the AMPLIFY Central DevOps API in your DevOps pipeline, your DevOps service (for example, Jenkins) must be authenticated with AMPLIFY platform and it must be authorized to use the DevOps API. Use AMPLIFY CLI to log in to AMPLIFY platform with a service account and obtain an access token to perform authorized API calls. For detailed steps, see [Manage an API proxy using AMPLIFY Central CLI](/docs/central/cli_central/cli_proxy_flow).

#### Example DevOps flow

The following shows an example DevOps flow to create, deploy, and promote an API proxy using Git and Jenkins:

1. A developer makes a change to the code for the service, updates the YAML configuration file for the desired state of proxy, and commits the changes to Git.
2. Jenkins build triggers and checks out the latest code and YAML from Git.
3. Jenkins calls AMPLIFY CLI command `amplify auth login` to authenticate to AMPLIFY platform and obtain an access token.
4. Jenkins calls AMPLIFY CLI command `amplify apic create` to create or update the API proxy for this service.
    * CLI calls the DevOps API `POST /proxies` to create the API proxy in AMPLIFY Central. See [Create proxy API reference](https://d-api.docs.stoplight.io/new-subpage/devops-api/create-proxy).
5. Jenkins calls AMPLIFY CLI command `amplify apic promote` to deploy the API proxy to the test runtime.
    * CLI calls the DevOps API `POST /promote` to deploy the API proxy on the test runtime. See [Promote proxy API reference](https://d-api.docs.stoplight.io/new-subpage/devops-api/promote-proxy).
6. Automated tests are run on the AMPLIFY Central test runtime.
7. Jenkins calls AMPLIFY CLI command `amplify apic promote` to promote the API proxy from the test runtime to the production runtime.
    * CLI calls the DevOps API `POST /promote` to deploy the API proxy on the production runtime.

## Traffic management

AMPLIFY Central provides centralized control and governance for the API traffic flowing within the organization across multiple environments. By handling service's needs like common network functions (for example, routing and load balancing), resiliency functions (for example, retries and timeouts), and security functions (for example, authentication, authorization) it frees up the development teams to focus on the business logic of your company.

## AMPLIFY Catalog

AMPLIFY Catalog provides a registry of all endpoints belonging to an organization. Examples of endpoints are APIs, MFT flows, SOAP, or gRPC, and they can be in different environments such as in the cloud, on-premise, within a microservice mesh, or at the edge (DMZ) of an organization. AMPLIFY Catalog allows developers to quickly discover and understand the endpoints and protocols that they can use to integrate quickly with your services.
