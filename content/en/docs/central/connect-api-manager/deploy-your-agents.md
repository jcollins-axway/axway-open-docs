---
title: Deploy your agents
linkTitle: Deploy your agents
draft: false
weight: 30
description: Learn how to deploy your Discovery Agent and Traceability Agent so
  that you can manage  your Axway API Gateway environment within AMPLIFY
  Central.
---
{{< alert title="Note" color="primary" >}}The Axway API Gateway connectivity to AMPLIFY Central is currently available in an alpha review mode; current functionality and configuration may change before release.   Therefore, this connectivity is available for trial use only and is not supported for production API management or connectivity.{{< /alert >}}

# Before you start

* Read [AMPLIFY Central and Axway API Manager connected overview](/docs/central/connect-api-manager/)
* Prepare AMPLIFY Central
* You will need a basic knowledge of Axway API Management solution

# Objectives

Learn how to install, customize and run your Discovery and Traceability agents.

# Discovery Agent

The Discovery Agent is used to discover new published APIs or any updated APIs. Once they are discovered, the related APIs are published to AMPLIFY Central, in one of the following publication modes, so that they become available for any consumer:

* Catalog item publication (disconnected mode): Customers  expose their APIs globally for their consumers but keep the API management at the Gateway level.
* Environment / API Service publication (connected mode): Customers manage their APIs from the AMPLIFY platform.

The Discovery Agent only discovers APIs that have the tag(s) defined in the agent configuration file. See [filtering apis to be discovered](/docs/central/connect-api-manager/filtering-apis-to-be-discovered/). By default the filter is empty and thus the agent will discover all published API. 

As soon as an API is published, the identifier of the asset in AMPLIFY Central is kept in a custom field at the API level in API Manager to help the agent remember what is already published.

The Agent can run in the following modes:

* With a yaml configuration file:

  * Default: located in the same directory as the agent binary.
  * Optional: use a dedicated folder where the configuration file is located (use the --pathConfig flag in the agent command line to access the file path).

    Configuration file name should be the same as the agent binary (discovery_agent.yaml).
  * Advanced configuration: properties inside the configuration file can reference environment variables. This enables you to set up only one configuration file that addresses different behaviors (depending on the environment variables). See [Discovery Agent variables](/docs/central/connect-api-manager/discovery-agent-variables/).
* With command line arguments. See [Discovery Agent flags](/docs/central/connect-api-manager/discovery-agent-flags/).

# Installing the agent

1. Download the zip file from Axway public repository the latest version using the following command:

```
curl -L "https://axway.bintray.com/generic-repo/v7-agents/v7_discovery_agent/latest/discovery_agent-latest.zip" -o discovery_agent-latest.zip
```

2. Unzip the file discovery_agent-latest.zip to get the agent binary (discovery_agent) and a template configuration file (discovery_agent.yaml).

   ```
   unzip discovery_agent-latest.zip
   ```
3. Copy those 2 files in a folder (/home/APIC-agents for instance) on the machine  where the API Manager environment is located. 
4. Move the `private_key.pem` and `public_key` files that were originally created when you set up your Service Account to the agent directory (APIC-agents). Note that the `public_key` comes from Steps 3 or 4 of [Create a Service Account](/docs/central/connect-api-manager/prepare-amplify-central/#create-a-service-account) depending if you choose to use the `der` format or not. 

# Customizing your agent configuration file

The `discovery_agent.yaml` configuration file contain 3 sections to personalize: apimanager, central and log.

## Customizing apimanager section:

This section helps the agent to connect to the API Manager and to know which API should be discovered and published to AMPLIFY Central.

`host`: Machine name where API Manager is running. localhost value can be used as the agent is installed on the same machine as the API Manager.

`port`: API Manager port number (8075 by default)

`discoveryIgnoreTags` (optional): comma-separated blacklist of tags. If an API has one or several of this blacklist tags, the agent will ignore this API and not publish it to AMPLIFY Central. This property takes precedence over the filter property below. The default value is empty which means no API will be ignored

`filter` (optional): expression to filter the API you want the agent to discover. See [Filtering APIs to be discovered](/docs/central/connect-api-manager/filtering-apis-to-be-discovered/). Leaving this field empty will tell the agent to discover all publishes API (REST / SOAP).

`proxyApicIDField` (optional): the field name used to store AMPLIFY Central identifier for the front end proxy in API Manager. Default value is **apicId**. If you don't intend to change it, comment this property. Be aware the field will not be visible in API Manager front end proxy as it is a specific configuration. If you want to see that field or customize it, refer to Add a custom property to APIs in [Customize API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_custom/index.html#customize-api-manager-data) documentation. 

`subscriptionApplicationField` (optional): the field name used to store AMPLIFY Central subscription identifier inside the API Manager application securing the front end proxy. Default value is **subscriptions**. If you don't intend to change it, comment this property. Be aware the field will not be visible in API Manager application as it is a specific configuration. If you want to see that field or customize it, refer to Add a custom property to applications in [Customize API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_custom/index.html#customize-api-manager-data) documentation. 

`pollInterval`: The frequency in which API Manager is polled for new endpoints. Default value is 30s 

`auth.username`: an API Manager user the agent will use to connect to the API Manager. This user must have either “API Manager Administrator” or “Organization administrator” role. Based on the role of this user, the agent is able to :

* discover any API from any organisation (“API Manager Administrator”)  
* discovery any API from a specific organisation (“Organization administrator”)

`auth.password`: the password of the API Manager user

Once all data is gathered, this section should looks like:

```
apimanager:
  host: localhost
  port: 8075
  discoveryIgnoreTags: tag1, tag2
  filter: 
#  proxyApicIDField: apicId
#  subscriptionApplicationField: subscriptions
  pollInterval: 30s
  auth:
    username: apiManagerUser
    password: apiManagerUserPassword
```

## Customizing central section:

`url`: the amplify central url. Default value is **https://apicentral.axway.com**   

`teamID`: The Team identifier in AMPLIFY Central that all APIs will be linked. Locate this at AMPLIFY Central > Access > Teams. Open the teams details. The team identifier is the last part of the url (AMPLIFY URL/access/teams/detail/**e4ec6c1a69fd0b8e016a0bb0681e0e8f**).

`tenantID`: The Organization ID from AMPLIFY Central. Locate this at Platform > User > Organization > ORrg ID field.

`environment`: environment name you created when [preparing AMPLIFY Central](/docs/central/connect-api-manager/prepare-amplify-central/)

`apiServerVersion`: version of AMPLIFY Central API the agent is using. Default value is **v1alpha1**

`mode`: Method to send endpoints back to Central. (publishToEnvironment = API Service, publishToCatalog = Catalog, publishToEnvironmentAndCatalog = API Service and as Consumer instance).  

`pollInterval`:  

`auth.url`: The AMPLIFY login URL. Default value is **https://login.axway.com/auth** 

`auth.realm`: The Realm used to authenticate for AMPLIFY Central. Default value is **Broker** `auth.clientId`: The name of the Service Account you created when [preparing AMPLIFY Central](/docs/central/connect-api-manager/prepare-amplify-central/). Locate this at AMPLIFY Central > Access > Service Accounts.

`auth.privateKey`: location of the private key file you created when [preparing AMPLIFY Central](/docs/central/connect-api-manager/prepare-amplify-central/). Absolute file path is recommended to avoid confusion. 

`auth.publicKey`:  location of the public key file you created when [preparing AMPLIFY Central](/docs/central/connect-api-manager/prepare-amplify-central/). Absolute file path is recommended to avoid confusion.  

`auth.keyPassword`: the key password to open the key. None set up by default.

`auth.timeout`: timeout for the authentication. Default value is **10s**

Once all data is gathered, this section should looks like:

```
central:
  url: https://apicentral.axway.com
  teamID: e4ec6c1a69fd0b8e016a0bb0681e0e8f
  tenantID: 68794y2
  environment: my-v7-env
  apiServerVersion: v1alpha1
  mode: publishToEnvironmmentAndCatalog
  pollInterval:
  auth:
    url: https://login.axway.com/auth
    realm: Broker
    clientId: DOSA_66743...
    privateKey: /home/APIC-agents/private_key.pem
    publicKey: /home/APIC-agents/public_key
    keyPassword:
    timeout: 10s
```

## Customizing log section:

The log section will help you to define how the agent is managing its logs.

`level`: The log level for output messages (debug, info, warn, error). Default value is **info**

`format`: The format to print log messages (json, line, package). Default value is **json**

`output`: The output for the log lines (stdout, file, both). Default value is **stdout** 

`path`: The path (relative to the agent binary or absolute) to save logs files, if output type file or both. Default value is relative path **logs**

In the end your discovery_agent.yaml file should look like':

```
apimanager:
  host: localhost
  port: 8075
  discoveryIgnoreTags: tag1, tag2
  filter: 
#  proxyApicIDField: apicId
#  subscriptionApplicationField: subscriptions
  pollInterval: 30s
  auth:
    username: apiManagerUser
    password: apiManagerUserPassword
    
central:
  url: https://apicentral.axway.com
  teamID: e4ec6c1a69fd0b8e016a0bb0681e0e8f
  tenantID: 68794y2
  environment: my-v7-env
  apiServerVersion: v1alpha1
  mode: publishToEnvironmmentAndCatalog
  pollInterval:
  auth:
    url: https://login.axway.com/auth
    realm: Broker
    clientId: DOSA_66743...
    privateKey: /home/APIC-agents/private_key.pem
    publicKey: /home/APIC-agents/public_key
    keyPassword:
    timeout: 10s
    
log:
  level: info
  format: json
  output: stdout
  path: logs

```

# Running Discovery Agent

Open a shell and run the following command to start up your agent:

```
cd /home/APIC-agents
./discovery_agent
```

To verify if the agent is up and running, open another shell command and run:

```
cd /home/APIC-agents
./discovery_agent --status
```

# Traceability Agent

The Traceability Agent is used to filter the Axway API Gateway logs that are related to discovered APIs and prepare the transaction events that are sent to AMPLIFY platform. Each time an already discovered API is called by a consumer, an event (summary + detail) is sent to AMPLIFY Central and is visible in API Observer.

The Agent can run in the following modes:

* With a configuration file

  * Default: located in the same directory as the agent binary.
  * Optional: use a dedicated folder where the configuration file is located (use the --path.config flag). See [Traceability Agent flags](/docs/central/connect-api-manager/traceability-agent-flags/).

    Configuration file name should be the same as the agent binary.

    Properties inside the configuration file can reference environment variables. This enables you to set up only one configuration file that addresses different behaviors (depending on the environment variables). See [Traceability Agent variables](/docs/central/connect-api-manager/traceability-agent-variables/).
* With a YAML configuration file

### Create your env_vars file

To create an `env_vars file`, see [Traceability Agent variables](/docs/central/connect-api-manager/traceability-agent-variables/).

### Create your YAML config file

Most Traceability Agent configurations are overridden by the environment variable, except for the APIGateway event file path(s). Note that the default `traceability_agent.inputs.paths` is set to read multiple files using wildcard.

#### YAML config file template - Traceability Agent

```
traceability_agent:
  inputs:
    - type: log
      paths:
        - <PATH TO>/group-2_instance-1.log
      include_lines: ['.*"type":"transaction".*"type":"http".*']

# Send output to Central Database
output.traceability:
  pipelines: 0
  enabled: true
  hosts: "ingestion-lumberjack.datasearch.axway.com:453"
  ssl:
    enabled: true
    verification_mode: none
  agent:
    central:
      url: "https://apicentral.axway.com"
      deployment: "prod"
      tenantID: "<THE TEAM ID>"
      environment: "<NAME OF THE CENTRAL TOPOLOGY ENVIRONMENT>"
      auth:
        url: "https://login.axway.com/auth"
        realm: "Broker"
        clientId: "<SERVICE ACCOUNT NAME: DOSA_xxxxxxxxx>"
        privateKey: "<PATH TO>/private_key.pem"
        publicKey: "<PATH TO>/public_key"
        keyPassword: ""
        timeout: 10s
    apigateway:
      host: localhost
      status:
      port: 8090
      pollInterval: 1m
      auth:
        username: <APIGATEWAY USER NAME WITH APIGATEWAY OPERATOR ROLE>
        password: <APIGATEWAY USER PASSWORD>
      ssl:
        insecureSkipVerify: true
    apimanager:
      proxyApicIDField: APIC_ID
      host: localhost
      status:
      port: 8075
      apiVersion: 1.3
      pollInterval: 1m
      auth:
        username: <APIMANAGER ADMIN USER>
        password: <APIMANAGE ADMIN PASSWORD>
      ssl:
        insecureSkipVerify: true

logging:
  metrics:
    enabled: false
  # Send all logging output to stderr
  to_stderr: true
  # Send all logging output to file
  to_files: false
  files:
    path: ./
    name: logs.txt
    keepfiles: 7
    permissions: 0644
  # Set log level
  level: debug
```

#### Multiple file paths

```
traceability_agent:
  inputs:
    - type: log
      paths:
        - /home/api/APIM_v7/apigateway/events/group-2_instance-1.log
        - /home/api/APIM_v7/apigateway/events/group-2_instance-2.log
```

#### File path with wildcard

```
traceability_agent:
  inputs:
    - type: log
      paths:
        - /home/api/APIM_v7/apigateway/events/group-2_instance-*.log
```

### Install and run Traceability Agent

1. Move the `private_key.pem` and `public_key` files that were originally created when you set up your Service Account to a keys directory. Make sure the directory is located on the machine being used for deployment.
2. Download the zip file:

   ```
   curl -L "https://axway.bintray.com/generic-repo/v7-agents/v7_traceability_agent/latest/traceability_agent-latest.zip" -o traceability_agent-latest.zip
   ```

   The zip contains the Traceability Agent config yaml and the Traceability Agent executable.
3. Unzip the file and install the binary on a machine that can access the APIM Manager environment.
4. Open a shell and run the following command to start up your agent:

   ```
   ./traceability_agent
   ```
5. Open a shell and run the following health check command to verify if the agent is up and running:

   ```
   ./traceability_agent status
   ```