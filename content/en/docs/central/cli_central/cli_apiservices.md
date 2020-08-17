---
title: Build and manage API services in your environments
linkTitle: Build and manage API services in your environments
weight: 110
date: 2020-06-10T00:00:00.000Z
description: Learn how your DevOps process can use AMPLIFY Central CLI to build and manage API services in your environments.
---

## Before you start

* You must [authorize your DevOps service to use the DevOps API](/docs/central/cli_getstarted/)
* Verify the @axway/amplify-central-cli version is at minimum 0.1.3.

## Objectives

Learn how to create and manage your API services to represent your distributed cloud and on-premise environments using the AMPLIFY Central CLI.

* Create a new API service in an environment
* Retrieve a list of all API services in an environment
* Retrieve details for a specific API service
* Update a specific API service
* Delete a specific API service

## Create an API service in your environment

An API service represents an API, including all its versions and deployed endpoints, and additional information to represent your API, for example, description, environment scope, image encoded in base64.

To automate the creation of an API service in your environment:

1. Create an environment by providing the environment name argument, for example, `env1`:

    ```
    amplify central create env env1
    ```

2. Create an API service within environment `env1` by providing a path to a valid .yaml, .yml, or .json file that defines a specific resource (for example, `apiservice.yaml`).  In this example, only one API service called `apisvc1` is created from the resource file:

    ```
    amplify central create -f <filepath>
    ```

Try out this procedure using the [apiservice.json](https://axway-open-docs.netlify.app/samples/central/apiservice.json) or [apiservice.yaml](https://axway-open-docs.netlify.app/samples/central/apiservice.yaml) samples.

## Retrieve a list of API services

Get a list of all API services in all environments:

```
amplify central get apiservices
```

You can also get help with a list of supported resource types:

```
amplify central get
```

## Retrieve details for a specific API Service

Get the details for a specific API service in an environment by providing the environment name and the API service name:

```
amplify central get apisvc <name> --scope env1 -o yaml  # Get API service <name> details for `env1` in YAML format
```

```
amplify central get apisvc <name> -s env1 -o json       # Get API service <name> details for `env1` in JSON format  
```

## Update a specific API Service

Update the details of a specific API service by providing a path to the configuration file:

```
amplify central apply -f apiservice.yaml   # Update API service in YAML format
```

```
amplify central apply -f apiservice.json   # Update API service in JSON format
```

## Delete a specific API Service in an environment

This action will delete all API services and resources in the environment specified. The CLI command can take a few seconds to finish depending on the number of resources represented in the environment.

{{% alert title="Warning" color="warning"%}}This action cannot be reversed.{{% /alert %}}

To delete a specific API service in an environment, provide a path to the configuration file:

```
amplify central delete -f apiservice.yaml   # Delete an API service using a file in YAML format
```

```
amplify central delete -f apiservice.json   # Delete an API service using a file in JSON format
```

Use `--wait` to delete an API service using a YAML file while waiting for resource deletion confirmation. The `--wait` option will check for resource deletion for up to 10 seconds.

```
amplify central delete -f apiservice.yaml --wait
```

Use `--scope` to delete an API service within the scope of and environment named env1.

```
amplify central delete apiservice apisvc1 -scope env1 --wait
```

## Review

You have learned how to use the AMPLIFY Central CLI to build and manage API services in your environments.
