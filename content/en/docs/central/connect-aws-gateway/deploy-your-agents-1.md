---
title: Deploy agents - advanced functionality
linkTitle: Deploy your agents - advanced functionality
draft: false
weight: 30
description: >-
  Learn how to deploy your Discovery Agent and Traceability Agent using Docker
  containers so that you can manage  your AWS API Gateway environment within
  AMPLIFY Central.

  Once agents are correctly deployed, they can collect the data from the AWS API Gateway and send it securely to AMPLIFY Central.
---
## Before you start

* Read [AMPLIFY Central AWS API Gateway connected overview](/docs/central/connect-aws-gateway/)
* [Prepare AMPLIFY Central](/docs/central/connect-aws-gateway/prepare-amplify-central-1/)
* [Prepare AWS API Gateway](/docs/central/connect-aws-gateway/prepare-aws-api-gateway/)
* Docker must be installed and you will need a basic understanding of Docker commands

## Objectives

Learn how to create your Discovery Agent and Traceability Agent configuration files, then install and run your agents.

## Discovery Agent

The Discovery Agent is used to discover new deployments and stage updates to existing deployments. Once they are discovered, the related APIs are published to AMPLIFY Central so that they become available for any consumer.

As soon as an API is published, a new `APIC_ID` tag is added to the stage so that the Discovery Agent will not published it again.

The Discovery Agent only discovers published APIs where the stage has a  tag(s) defined in the agent configuration file. See AWS_DISCOVERYTAGS.

### Create your Discovery Agent configuration

<!-- HTML table removed here, it will need to be added back manually as a Markdown table -->

| Variable name                                    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |                                    |                                                                                                    |
| ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------- | -------------------------------------------------------------------------------------------------- |
| AWS variables                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |                                    |                                                                                                    |
| AWS_SQSPOLLINTERVAL                              | The interval at which to poll SQS for messages (ns - default, us, ms, s). Set to 20s. Max value is 20s                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |                                    |                                                                                                    |
| AWS_APIPOLLINTERVAL                              | The interval at which to poll AWS for changes to things like UsagePlans (ns - default, us, ms, s, m, h). Set to 60s.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |                                    |                                                                                                    |
| AWS_REGION                                       | The region where AWS APIs are stored.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |                                    |                                                                                                    |
| AWS_QUEUENAME                                    | The name of the queue where Discovery Agent will read relevant AWS events. It is the QueueName provided during installation.                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                                    |                                                                                                    |
| AWS_LOGGROUPARN                                  | The ARN for the log group where API Gateway will send Execution events (output of Step 5). See [Prepare AWS Gateway to deploy the Discovery Agent AWS config setup](/docs/central/connect-aws-gateway/prepare-aws-api-gateway/).                                                                                                                                                                                                                                                                                                                                                                           |                                    |                                                                                                    |
| AWS_FILTER                                       | Filter conditions for discovery based on AWS Stage tags to determine adding the API to Central. [See Discover APIs for conditional expression samples](/docs/central/connect-aws-gateway/filtering-apis-to-be-discovered-1/).                                                                                                                                                                                                                                                                                                                                                                              |                                    |                                                                                                    |
| AWS_PUSHTAGS                                     | Determines whether the AWS Stage tags should be pushed to AMPLIFY Central along with the API definition. Value must be true or false. Default is false.                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                    |                                                                                                    |
| AWS_AUTH_ACCESSKEY                               | The access key of the AWS account where APIs are stored. Generated when the apigw_cloudformation script is run or you can use your own.                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                    |                                                                                                    |
| AWS_AUTH_SECRETKEY                               | The secret access key of the AWS account where APIs are stored. Generated when the apigw_cloudformation script is run or you can use your own.                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                    |                                                                                                    |
| LOG_LEVEL                                        | The log level for output messages (debug, info, warn, error).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                    |                                                                                                    |
| LOG_FORMAT                                       | The format to print log messages (json, line, package).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                    |                                                                                                    |
| LOG_OUTPUT                                       | The output for the log lines (stdout, file, both).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                    |                                                                                                    |
| LOG_PATH                                         | The path (relative or absolute) to save logs files, if output type file or both.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |                                    |                                                                                                    |
| AMPLIFY Central variables                        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |                                    |                                                                                                    |
| CENTRAL_URL                                      | The URL to the AMPLIFY Central instance being used for this Discovery Agent.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                                    |                                                                                                    |
| CENTRAL_TENANTID                                 | The Organization ID from AMPLIFY Central. Locate this at Platform > User > Organization.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |                                    |                                                                                                    |
| CENTRAL_TEAMID                                   | The Team ID in AMPLIFY Central that all APIs will be linked. Locate this at AMPLIFY Central > Access > Teams. Open the teams details. The team identifier is the last part of the url (AMPLIFY URL/access/teams/detail/`e4ec6c1a69fd0b8e016a0bb0681e0e8f`).                                                                                                                                                                                                                                                                                                                                                |                                    |                                                                                                    |
| CENTRAL_MODE                                     | Method to send endpoints back to Central. (`publishToEnvironment` = API Service, `publishToEnvironmentAndCatalog` = API Service and Catalog asset).                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                    |                                                                                                    |
| CENTRAL_AUTH_URL                                 | The AMPLIFY login URL: <https://login.axway.com/auth>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |                                    |                                                                                                    |
| CENTRAL_AUTH_REALM                               | The Realm used to authenticate for AMPLIFY Central.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                    |                                                                                                    |
| CENTRAL_AUTH_CLIENTID                            | The name of the Service Account created in AMPLIFY Central. Locate this at AMPLIFY Central > Access > Service Accounts.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                    |                                                                                                    |
| CENTRAL_AUTH_PRIVATEKEY                          | The private key associated with the Service Account.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |                                    |                                                                                                    |
| CENTRAL_AUTH_PUBLICKEY                           | The public key associated with the Service Account.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                    |                                                                                                    |
| CENTRAL_AUTH_KEYPASSWORD                         | The password for the private key, if applicable.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |                                    |                                                                                                    |
| CENTRAL_AUTH_TIMEOUT                             | The timeout to wait for the authentication server to respond (ns - default, us, ms, s, m, h). Set to 10s.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |                                    |                                                                                                    |
| CENTRAL_ADDITIONALTAGS                           | Additional tag names to publish separated by a comma.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |                                    |                                                                                                    |
| CENTRAL_APISERVERENVIRONMENT                     | Environment eventually set by download kit in APIC.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                    |                                                                                                    |
| CENTRAL_SSL_MINVERSION                           | String value for the minimum SSL/TLS version that is acceptable. If zero, empty TLS 1.0 is taken as the minimum. Allowed values are: TLS1.0, TLS1.1, TLS1.2, TLS1.3.                                                                                                                                                                                                                                                                                                                                                                                                                                       |                                    |                                                                                                    |
| CENTRAL_SSL_MAXVERSION                           | String value for the maximum SSL/TLS version that is acceptable. If empty, then the maximum version supported by this package is used, which is currently TLS 1.3. Allowed values are: TLS1.0, TLS1.1, TLS1.2, TLS1.3.                                                                                                                                                                                                                                                                                                                                                                                     |                                    |                                                                                                    |
| CENTRAL_SSL_CIPHERSUITES                         | An array of strings. It is a list of supported cipher suites for TLS versions up to TLS 1.2. If CipherSuites is nil, a default list of secure cipher suites is used, with a preference order based on hardware performance. See the [supported cipher suites](/docs/central/connect-aws-gateway/agent-security-aws/).                                                                                                                                                                                                                                                                                      |                                    |                                                                                                    |
| CENTRAL_SSL_NEXTPROTOS                           | An array of strings. It is a list of supported application level protocols, in order of preference, based on the ALPN protocol list. Allowed values are: h2, htp/1.0, http/1.1, h2c.                                                                                                                                                                                                                                                                                                                                                                                                                       |                                    |                                                                                                    |
| CENTRAL_SSL_INSECURESKIPVERIFY                   | InsecureSkipVerify controls whether a client verifies the server's certificate chain and host name. If true, TLS accepts any certificate presented by the server and any host name in that certificate. In this mode, TLS is susceptible to man-in-the-middle attacks.                                                                                                                                                                                                                                                                                                                                     |                                    |                                                                                                    |
| CENTRAL_ADDITIONALTAGS                           | Additional tag names to publish separated by a comma.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | CENTRAL_SUBSCRIPTIONS_APPROVALMODE | The mode for approving subscriptions on AMPLIFY Central (manual, auto, webhook; default = manual). |
| CENTRAL_SUBSCRIPTIONS_APPROVALWEBHOOK_URL        | The url for a subscription approval webhook (if any). CENTRAL_SUBSCRIPTIONS_APPROVALMODE must be set to "webhook" for webhooks to be invoked.                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                    |                                                                                                    |
| CENTRAL_SUBSCRIPTIONS_APPROVALWEBHOOK_HEADERS    | The headers to pass to the subscription approval webhook (if any). For example, "Header=contentType,Value=application/json".                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                                    |                                                                                                    |
| CENTRAL_SUBSCRIPTIONS_APPROVALWEBHOOK_AUTHSECRET | The authentication secret to pass to the subscription approval webhook (if any).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |                                    |                                                                                                    |
| Subscription notification variables              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |                                    |                                                                                                    |
| SUBSCRIPTIONS_WEBHOOK_URL                        | The webhook URL that subscription data will be posted to, see                                                                                                                                                                                                                                                                                                                                                                                                                                                                   [Subscription webhook notifications](#subscription-webhook-notifications). |                                    |                                                                                                    |
| SUBSCRIPTIONS_WEBHOOK_HEADERS                    | The headers that will be used when posting data to the webhook url, see [Subscription webhook notifications](#subscription-webhook-notifications).                                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                    |                                                                                                    |
| SMTP_HOST                                        | The SMTP server that will send email notifications.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                    |                                                                                                    |
| SMTP_PORT                                        | The SMTP port to communicate to the SMTP server over.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |                                    |                                                                                                    |
| SMTP_USERNAME                                    | The username used to authenticate to the SMTP server, if necessary.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                    |                                                                                                    |
| SMTP_PASSWORD                                    | The password used to authenticate to the SMTP server, if necessary.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |                                    |                                                                                                    |
| SMTP_FROMADDRESS                                 | The email address that will be listed in the from field.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |                                    |                                                                                                    |
| SMTP_SUBSCRIBE_SUBJECT                           | The subject of email sent for Subscribe events.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |                                    |                                                                                                    |
| SMTP_SUBSCRIBE_BODY                              | The body of the email for Subscribe events, see [SMTP notifications](#smtp-notifications).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |                                    |                                                                                                    |
| SMTP_UNSUBSCRIBE_SUBJECT                         | The subject of email sent for Unsubscribe events.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |                                    |                                                                                                    |
| SMTP_UNSUBSCRIBE_BODY                            | The body of the email for Unsubscribe events, see [SMTP notifications](#smtp-notifications).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                                    |                                                                                                    |
| SMTP_SUBSCRIBEFAILED_SUBJECT                     | The subject of email sent for Failed to Subscribe events.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |                                    |                                                                                                    |
| SMTP_SUBSCRIBEFAILED_BODY                        | The body of the email for Failed to Subscribe events, see [SMTP notifications](#smtp-notifications).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |                                    |                                                                                                    |
| SMTP_UNSUBSCRIBEFAILED_SUBJECT                   | The subject of email sent for Failed to Unsubscribe events.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |                                    |                                                                                                    |
| SMTP_UNSUBSCRIBEFAILED_BODY                      | The body of the email for Failed to Unsubscribe events, see [SMTP notifications](#smtp-notifications).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |                                    |                                                                                                    |

#### Customizing email servers

The `host`, which represents the email server, can be configured with minimal setup.  This section represents the email servers that have been currently tested. Please note, that all testing has been set up on port 587 signifying TLS support.  

```
# Google/Gmail server
SUBSCRIPTIONS_SMTP_HOST=smtp.gmail.com
SUBSCRIPTIONS_SMTP_PORT=587
SUBSCRIPTIONS_SMTP_USERNAME=your GMAIL account
SUBSCRIPTIONS_SMTP_PASSWORD=your GMAIL password
SUBSCRIPTIONS_SMTP_AUTHTYPE=PLAIN

# Microsoft office server
SUBSCRIPTIONS_SMTP_HOST=smtp.office365.com
SUBSCRIPTIONS_SMTP_PORT=587
SUBSCRIPTIONS_SMTP_USERNAME=your Office Mail account
SUBSCRIPTIONS_SMTP_PASSWORD=your Office Mail password
SUBSCRIPTIONS_SMTP_AUTHTYPE=LOGIN

# Microsoft outlook server
SUBSCRIPTIONS_SMTP_HOST=smtp-mail.outlook.com
SUBSCRIPTIONS_SMTP_PORT=587
SUBSCRIPTIONS_SMTP_USERNAME=your Outlook Mail account
SUBSCRIPTIONS_SMTP_PASSWORD=your Office Mail password
SUBSCRIPTIONS_SMTP_AUTHTYPE=PLAIN

# Yahoo email server
SUBSCRIPTIONS_SMTP_HOST=smtp.mail.yahoo.com
SUBSCRIPTIONS_SMTP_PORT=587
SUBSCRIPTIONS_SMTP_USERNAME=your Yahoo Mail account
SUBSCRIPTIONS_SMTP_PASSWORD=your Yahoo Mail password
SUBSCRIPTIONS_SMTP_AUTHTYPE=PLAIN
```

### Subscription webhook notifications

When configured, a webhook notification will create an HTTP Post request to the URL specified.

The post request can include optional headers specified in the SUBSCRIPTIONS_WEBHOOK_HEADERS  environment variables. Here is an example of specifying multiple headers in the environment file:

...
SUBSCRIPTIONS_WEBHOOK_HEADERS=Header=contentType,Value=application/json, Header=Elements-Formala-Instance-Id,Value=5551212, Header=Authorization,Value=authvalue
...

````
The data posted will be a JSON object with the following structure:

```json
{
  "catalogItemId":"value",
  "catalogItemUrl":"value",
  "catalogItemName":"value",
  "action":"value",           # see below
  "email":"value",            # if applicable
  "message":"value",          # if applicable, any errors that occurred will be here
  "key":"value",              # if applicable
  "keyHeaderName":"value",    # if applicable
  "clientID":"value",         # if applicable
  "clientSecret":"value"      # if applicable
}
````

The action above will be one of the following:

* ACTIVE - Subscription was approved and processed properly by the agent
* FAILED_TO_SUBSCRIBE - Subscription was approved but failed processing in the agent
* UNSUBSCRIBED - Unsubscribe was processed properly by the agent
* FAILED_TO_UNSUBSCRIBE - Unsubscribe process failed in the agent

### SMTP notifications

Subscription notifications with email notifications can set both the subject and the body.  The body, which may be HTML, is able to substitute data from the subscription event into the email that is sent to the event initiator. Here is a sample of the template used for subscribe events:

```
...
SMTP_SUBSCRIBE_SUBJECT=Subscription Notification
SMTP_SUBSCRIBE_BODY=Subscription created for Catalog Item:  <a href= ${catalogItemUrl}> ${catalogItemName} </a> <br/>
Your API is secured using an APIKey credential:header:<b>${keyHeaderName}</b>/value:<b>${key}</b>
...
```

The agent will fill in the appropriate data for the variables specified, ie ${catalogItemUrl}, before sending the SMTP message.  The variables that may be used are the keys for the JSON data sent to the webhook endpoint, see [Subscription webhook notifications](#subscription-webhook-notifications).

### Create your Discovery Agent environment file

Create a configuration file using the above variables. See the variable descriptions for their values. Below is a sample of what the configuration file will look like.

For example:  

```
# AWS connectivity
AWS_REGION=us-east-2
AWS_QUEUENAME=aws-apigw-discovery-us-east-2
AWS_AUTH_ACCESSKEY=<YOUR AWS ACCESS KEY HERE>
AWS_AUTH_SECRETKEY=<YOUR AWS SECRET KEY HERE>
AWS_LOGGROUPARN=<YOUR LOG GROUP ARN>
AWS_FILTER=tag.PushToAmplify == true
AWS_PUSHTAGS=true

# AMPLIFY connectivity
CENTRAL_URL=https://apicentral.axway.com

#AMPLIFY Central connectivity
# organisation config:
CENTRAL_TENANTID=<YOUR ORGANIZATION ID>
CENTRAL_TEAMID=<THE TEAM ID>
CENTRAL_ENVIRONMENT=<NAME OF THE CENTRAL TOPOLOGY ENVIRONMENT>
CENTRAL_AUTH_CLIENTID=<SERVICE ACCOUNT NAME: DOSA_xxxxxxxxx>

CENTRAL_MODE=<publishToEnvironment | publishToEnvironmentAndCatalog>
CENTRAL_AUTH_URL=https://login.axway.com/auth
CENTRAL_AUTH_REALM=Broker
CENTRAL_AUTH_KEYPASSWORD=

#CENTRAL_SSL_MINVERSION=
#CENTRAL_SSL_MAXVERSION=
#CENTRAL_SSL_CIPHERSUITES=
#CENTRAL_SSL_NEXTPROTOS=
#CENTRAL_SSL_INSECURESKIPVERIFY=

LOG_LEVEL=info
LOG_OUTPUT=stdout
LOG_PATH=logs
```

### Install and run Discovery Agent

1. Copy the `private_key.pem` and `public_key.pem` files that were originally created when you set up your Service Account to a keys directory. Make sure the directory is located on the machine being used for deployment. Note that the `public_key.pem` comes from Steps 3 and 4 of [Prepare AWS Gateway to deploy the Discovery Agent AWS config setup](/docs/central/connect-aws-gateway/prepare-aws-api-gateway/).
2. Pull the latest image of the Discovery Agent:

   ```
   docker pull axway-docker-public-registry.bintray.io/agent/aws-apigw-discovery-agent:latest
   ```
3. Start the Discovery Agent pointing to the `env_vars` file and the keys directory:

   ```
   docker run --env-file ./env_vars -v <pwd>/keys:/keys  axway-docker-public-registry.bintray.io/agent/aws-apigw-discovery-agent:latest
   ```

   `pwd` relates to the local directory where the docker command is run. For Windows, the absolute path is preferred.
4. Run the following health check command to ensure the agent is up and running:

   ```
   docker inspect --format='{{json .State.Health}}' <container>
   ```

## Traceability Agent

The Traceability Agent is used to filter the AWS CloudWatch logs that are related to discovered APIs and prepare the transaction events that are sent to AMPLIFY platform. Each time an API is called by a consumer, an event (summary + detail) is sent to AMPLIFY Central and is visible in API Observer.

### Create your Traceability Agent configuration

<!-- HTML table removed here, it will need to be added back manually as a Markdown table -->

| Variable name                  | Description                                                                                                                                                                                                                                                                                                           |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AWS variables                  |                                                                                                                                                                                                                                                                                                                       |
| AWS_AUTH_ACCESSKEY             | The access key of the AWS account where APIs are stored.                                                                                                                                                                                                                                                              |
| AWS_AUTH_SECRETKEY             | The secret access key of the AWS account where APIs are stored.                                                                                                                                                                                                                                                       |
| AWS_REGION                     | The region where AWS APIs are stored.                                                                                                                                                                                                                                                                                 |
| AWS_SQSPOLLINTERVAL            | How often SQS queue is polled.                                                                                                                                                                                                                                                                                        |
| AWS_QUEUENAME                  | The name of the queue (TraceabilityQueueName) from Step 5. This is used for logging custom access log entries. See [Prepare AWS Gateway to deploy the Discovery Agent AWS config setup](/docs/central/connect-aws-gateway/prepare-aws-api-gateway/).                                                                  |
| Amplify Central variables      |                                                                                                                                                                                                                                                                                                                       |
| LOG_LEVEL                      | The log level for the agent.                                                                                                                                                                                                                                                                                          |
| LOG_FORMAT                     | The format to print log messages (json, line, package).                                                                                                                                                                                                                                                               |
| LOG_OUTPUT                     | The output for the log lines (stdout, file, both).                                                                                                                                                                                                                                                                    |
| LOG_PATH                       | LOG_PATH                                                                                                                                                                                                                                                                                                              |
| LOGSTASH_URL                   | The URL of the logstash to forward the transaction log entries. Default value is ingestion-lumberjack.beta.trcblt.com:453                                                                                                                                                                                             |
| CENTRAL_TENANTID               | The Organization ID from AMPLIFY Central. Locate this ID in the Platform > Organization > Org ID.                                                                                                                                                                                                                     |
| CENTRAL_DEPLOYMENT             | The APIC deployment environment.                                                                                                                                                                                                                                                                                      |
| CENTRAL_ENVIRONMENTID          | The unique character string that identifies your monitoring environment. e4e0810XXXXXXXXX                                                                                                                                                                                                                             |
| CENTRAL_AUTH_URL               | The AMPLIFY login URL: <https://login.axway.com/auth>                                                                                                                                                                                                                                                                 |
| CENTRAL_AUTH_REALM             | The Realm used to authenticate for AMPLIFY Central. Locate this in AMPLIFY Central > Access > Service Accounts.                                                                                                                                                                                                       |
| CENTRAL_AUTH_CLIENTID          | The name of the Service Account created in AMPLIFY Central. Locate this in AMPLIFY Central > Access > Service Accounts. DOSA_XXXXXXXXXx                                                                                                                                                                               |
| CENTRAL_AUTH_PRIVATEKEY        | The private key associated with the Service Account.                                                                                                                                                                                                                                                                  |
| CENTRAL_AUTH_PUBLICKEY         | The public key associated to the Service Account.                                                                                                                                                                                                                                                                     |
| CENTRAL_AUTH_KEYPASSWORD       | The password for the private key, if applicable.                                                                                                                                                                                                                                                                      |
| CENTRAL_SSL_MINVERSION         | String value for the minimum SSL/TLS version that is acceptable. If zero, empty TLS 1.0 is taken as the minimum. Allowed values are: TLS1.0, TLS1.1, TLS1.2, TLS1.3.                                                                                                                                                  |
| CENTRAL_SSL_MAXVERSION         | String value for the maximum SSL/TLS version that is acceptable. If empty, then the maximum version supported by this package is used, which is currently TLS 1.3. Allowed values are: TLS1.0, TLS1.1, TLS1.2, TLS1.3.                                                                                                |
| CENTRAL_SSL_CIPHERSUITES       | An array of strings. It is a list of supported cipher suites for TLS versions up to TLS 1.2. If CipherSuites is nil, a default list of secure cipher suites is used, with a preference order based on hardware performance. See the [supported cipher suites](/docs/central/connect-aws-gateway/agent_security-aws/). |
| CENTRAL_SSL_NEXTPROTOS         | An array of strings. It is a list of supported application level protocols, in order of preference, based on the ALPN protocol list. Allowed values are: h2, htp/1.0, http/1.1, h2c                                                                                                                                   |
| CENTRAL_SSL_INSECURESKIPVERIFY | InsecureSkipVerify controls whether a client verifies the server's certificate chain and host name. If true, TLS accepts any certificate presented by the server and any host name in that certificate. In this mode, TLS is susceptible to man-in-the-middle attacks.                                                |

### Create your Traceability Agent environment file

Create a configuration file using the above variables. See the variable descriptions for their values. Below is a sample of what the configuration file will look like.  

For example:

```
# AWS connectivity
AWS_REGION=us-east-2
AWS_QUEUENAME=aws-apigw-traceability-us-east-2
AWS_AUTH_ACCESSKEY=<YOUR AWS ACCESS KEY HERE>
AWS_AUTH_SECRETKEY=<YOUR AWS SECRET KEY HERE>

#AMPLIFY Central connectivity
# organisation config:
CENTRAL_TENANTID=<YOUR ORGANIZATION ID>
CENTRAL_ENVIRONMENT=<NAME OF THE CENTRAL TOPOLOGY ENVIRONMENT>
CENTRAL_AUTH_CLIENTID=<SERVICE ACCOUNT NAME: DOSA_xxxxxxxxx>

CENTRAL_DEPLOYMENT=prod
CENTRAL_AUTH_URL=https://login.axway.com/auth
CENTRAL_AUTH_REALM=Broker

#CENTRAL_SSL_MINVERSION=
#CENTRAL_SSL_MAXVERSION=
#CENTRAL_SSL_CIPHERSUITES=
#CENTRAL_SSL_NEXTPROTOS=
#CENTRAL_SSL_INSECURESKIPVERIFY=


#Condor url
LOGSTASH_URL=ingestion-lumberjack.datasearch.axway.com:453

LOG_LEVEL=debug
LOG_OUTPUT=stdout
LOG_PATH=logs
```

### Install and run Traceability Agent

1. Copy the `private_key.pem` and `public_key.pem` files that were originally created when you set up your Service Account to a keys directory. Make sure the directory is located on the machine being used for deployment.
2. Pull the latest image of the Traceability Agent:

   ```
   docker pull axway-docker-public-registry.bintray.io/agent/aws-apigw-traceability-agent:latest
   ```
3. Start the Traceability Agent pointing to the `env_vars` file and the `keys` directory. Note that `pwd` relates to the local directory where the docker command is run. For Windows, the absolute path is preferred.

   ```
   docker run --env-file ./env_vars -v <pwd>/keys:/keys  axway-docker-public-registry.bintray.io/agent/aws-apigw-traceability-agent:latest
   ```
4. Run the following health check command to ensure the agent is up and running:

   ```
   docker inspect --format='{{json .State.Health}}' <container>
   ```
