{
    "title": "API Manager settings",
    "linkTitle": "API Manager settings",
    "weight": "10",
    "date": "2019-09-17",
    "description": "Reference guide for the **Settings** tab in the API Manager web UI."
}

## Account settings

You can configure the following settings for a user account. For more information on managing users, see [Manage users](/docs/apim_administration/apimgr_admin/api_mgmt_admin/#manage-users).

### General

**Image**
: Click to add a graphical image for the account (for example, .png, `.gif`, or `.jpeg` file).

**Login name**
: Enter a user login name for the account. The default is `apiadmin`. This is the default API administrator user for API Manager.

**Email**
: Enter an email address for the account. The default is `apiadmin@localhost`.

**Enabled**
: Select whether the account is enabled. The `apiadmin` account is enabled by default.

**Created on**
: Displays the date and time at which the account was created.

**Current state**
: Displays the state of the account. The `apiadmin` account is `Approved` by default.

### Membership

**Role**
: Displays the membership role of the account. The default `apiadmin` account has an **API Manager Administrator** role.

### Additional attributes

**Phone**
: Enter a contact phone number for the account.

**Description**
: Enter a description for the account. The default `apiadmin` account is described as **API Administrator**.

### Password

**Change password**
: Click to change the current password for the account. It is strongly recommended that you change the default password for security reasons.

The following restrictions apply:

* API administrators can change the password for any internal (non-on-boarded) API Manager user.
* Organization administrators can change the password for any internal user associated with their organization.
* External user passwords on-boarded from external identity providers cannot be changed.

## API Manager settings

You can configure the following settings on the **API Manager** tab:

### API Manager

**API Manager name**
: Enter the name displayed for API Manager in the email notifications sent to API providers (for example, your company name or website). Defaults to Axway API Manager. This setting is required.

**API Manager host**
: Enter the host name that API Manager is available on. Defaults to the API Manager IP address. It is not recommended to have spaces or the URL encoded `%20` in the host name.

**Email reply to**
: Enter the reply to address for email sent from API Manager (for example, the automatically generated emails sent when user accounts are created). Defaults to `no-reply@axway.com`.

**Email bounce**
: Enter the email address used to receive messages about the non-delivery of automatically generated email. Defaults to `apiadmin@localhost`.

**Demo mode**
: Select whether demo mode is enabled. When this setting is enabled, API Manager automatically generates random data, and displays metrics on the **Monitoring** tab without needing to send traffic through the API Gateway. Demo mode is disabled by default.

**Case-insensitive table sorting**
: Select whether case-insensitive sorting of data in tables is enabled. This is enabled by default.

**Trial mode**
: Select whether trail mode is enabled for all organizations. Trial mode allows the API administrator to manage the lifespan of the organization, including any resources that belong to that organization (for example, users or applications). When this setting is enabled, API Manager displays **TRIAL** settings for the administrator when editing the organization on the **Client Registry** > **Organizations** page. Trial mode is disabled by default.

**Default trial duration**
: When **Trial mode** is enabled, enter the duration of the trial in days. Defaults to 30 days. When the trial has ended, the organization expires, and users of the expired organization can no longer log in.

### API Portal

**API Portal**
: Select whether to enable API Portal. You should only enable this setting if you have an existing API Portal installation working with API Manager. When enabled, links in email notifications are addressed to the API Portal host or to the API Manager host, depending on whether you are an API consumer or API provider. This setting is disabled by default.

**API Portal name**
: Enter the name displayed for API Portal in email notifications sent to API consumers (for example, your company name or website). Defaults to Axway API Portal. This setting is required.

**API Portal host and port**
: Enter the host name or IP address and port used in auto-generated email links sent to API consumers (for example, `www.example.com:443`). The host is required, and the port is optional. If you do not enter a value, the default port is `443`. Enter the host and port (optional), but not the scheme. For example, `example.com:443` or `example.com` is correct, but `https://example.com:443` or `http://example.com` is incorrect.

For more details on API Portal, see [Administer API Portal](/docs/apim_administration/apiportal_admin/).

### General settings

**User registration**
: Select whether to enable automatic user registration. This is enabled by default.

**Auto-approve user registration**
: Select whether automatic approval of user registration requests is enabled. This is enabled by default.

**Auto-approve applications**
: Select whether automatic approval of client applications is enabled. This is enabled by default.

**Login name regular expression**
: Enter a valid regular expression to restrict the login names that you can enter. This does not retrospectively enforce login names. If you change the default setting, you must update the `loginNameValidationMessage` in `app.config`. Defaults to `[^;,\\/?#<>&;!]{1,}`.

**User name regular expression**
: Enter a valid regular expression to restrict the user names that you can enter.  This does not retrospectively enforce existing user names. Defaults to `^[\\p{L}\\d .,\'_-]+$`.

**Enable application scopes**
: Select whether to enable scopes at the level of the client application. This allows the API administrator to create application-level scopes to permit access to resources that are not covered by API-level scopes (for example, for API method-level authorization). This setting is not enabled by default. For more details, see [Configure API method-level authorization for client applications](/docs/apim_administration/apimgr_admin/api_mgmt_method_authz/).

**Apply application scope restrictions**
: When this option is selected, only the scopes that are enabled at the level of the client application are returned when a request is submitted that contains an empty scope list. This enables applications to have read-only access to an API. Scopes that are not specified in the application are not available when requesting a token for this application.

**Enable query string version routing**
: Select whether to enable routing to different front-end API versions from a single base path using a query string parameter (for example, `https://HOSTNAME:8065/api/helloworld?v=v1`). This setting is unselected by default, and the URL path-based version is used instead. When selected, you must enter a value in the next setting, **Query string version parameter**.

**Query string version parameter**
: Specifies the name of the query string version parameter used to route between different API versions (for example, a value of `v` requires `/my_api?v=1` in the query string, while `version` requires `/my_api?version=1`). The name of the parameter will also be published in the Swagger generated for the front-end API in the API Catalog. For a detailed example, see [Configure API routing based on version query string](/docs/apim_administration/apimgr_admin/api_mgmt_version_routing/).

### Password, login, and session management settings

**Idle session timeout (minutes)**
: Enter the number of minutes after which idle API Manager sessions time out. Defaults to `60` minutes. Changing this value only affects logins made after the change.

**Change password on first login**
: Select whether to enforce a change of password when a user logs into API Manager for the first time. This is enabled by default.

**Enable password reset**
: Select whether to enable the **Forgot Password** tab on the API Manager login page. For some providers (for example, LDAP), you cannot reset the user password, so you might need to disable this feature. This is enabled by default.

**Enable password expiry**
: Select whether to enable password expiry for API Manager users. This is enabled by default. For more details, see [Enforce password changes](/docs/apim_administration/apimgr_admin/api_mgmt_admin/#enforce-password-changes).

**Days before passwords expire**
: Enter the number of days after which a user password expires. The default value is `90` days. This setting is only applicable if password expiry is enabled.

**Minimum password length**
: Select the minimum number of characters required for user passwords. The default value is `6`.

**Lock user account on invalid login**
: Select whether a user should be locked out of API Manager for 30 minutes after 6 invalid logins have been attempted over a time period of 5 minutes. When a user account is locked, the user can either wait for the configured time period to elapse or use the **Forgot Password** capability (the user can log back in immediately when they receive a new autogenerated password).
: You can configure API Manager to use an external Identity Provider (IdP) for authentication. In this case users are considered _external_ and are automatically added as API Manager users when they first login. If the external IdP has its own account lockout feature, and a user has been locked out (for whatever reason), API Manager sees this as invalid login attempt. If you use only an external IdP for user authentication you can disable this setting as the IdP performs the same task.

### Organization administrator delegation

**Delegate user management**
: Select whether organization administrators can create or remove applications, and approve requests from users to create applications. This is enabled by default.

**Delegate application management**
: Select whether organization administrators can create or remove applications, and approve requests from users to create applications. This is enabled by default.

### API registration

**API default virtual host**
: Enter a host and port on which all registered and published APIs are available. The specified host must be DNS resolvable.

**API promotion via policy**
: Select whether APIs can be promoted using a policy specified in Policy Studio. Enabling the **API promotion via policy** setting forces a reload of API Manager, and you must log in again. A **Promote API** option is also then added to the **Frontend API** management menu. This setting is disabled by default.
: For more information on API promotion, see [Promote managed APIs](/docs/apim_administration/apimgr_admin/api_mgmt_promote/).

### API import

**Strict certificate checking**
: Select whether to validate that the certificate is recognised as a valid server certificate during import.

**Server certificate verification**
: Select whether to validate that the certificate presented by the server matches the Remote Host being connected to during import.

**Mime validation**
: Select to perform MIME Type validation during import. MIME Type validation is implemented for OAS3 and Swagger 2 APIs. This setting is enabled by default.

**Import timeout (seconds)**
: Maximum amount of time to wait until an API is imported. This is particularly useful while importing OAS3 files with multiple parts.

**Allow users to modify Backend APIs**
: Select to ensure that all APIs imported are editable by default.

### Global policies

**Enable Global Policies**
: Select whether to enable a global policies that are applied to all front-end API invocations (for example, mandatory security, compliance, or governance policies). This setting is disabled by default.

**Global Request Policy**
: Select an optional global request policy to apply to all front-end API invocations. When a global request policy has been selected, it is displayed on the **Frontend API** > **Outbound** tab when you click **Advanced**. The global request policy is executed after inbound authentication, but before any non-global request, routing, or response policies configured for the front-end API.

**Global Response Policy**
: Select an optional global response policy to apply to all front-end API invocations. When a global response policy has been selected, it is displayed on the **Frontend API** > **Outbound** tabwhen you click **Advanced**. The global response policy is executed last after any non-global response policy configured for the front-end API.

For more details, see [Enforce API Manager global policies](/docs/apim_administration/apimgr_admin/api_mgmt_custom_policies/#enforce-api-manager-global-policies).

### Fault handlers

**Enable API Manager fault handlers**
: Select whether to enable fault handler policies that are applied to front-end API invocations. When this setting is enabled, an API administrator can select a global fault handler policy for all front-end APIs. API developers can also select fault handler policies for specific front-end APIs and methods. This setting is switched off by default.

**Global Fault handler Policy**
: When fault handlers are enabled, you can select a global fault handler to apply to all front-end API invocations. The list of available policies is determined by the fault handler policies that have been configured in Policy Studio. The selected policy will be executed at runtime in the event of an error. This setting defaults to the API Manager **Default Fault Handler** policy.

For more details, see [Add API Manager fault handler policies](/docs/apim_administration/apimgr_admin/api_mgmt_custom_policies/#add-api-manager-fault-handler-policies).

### Advisory banner

Enable this setting to display the advisory banner text on the API Manager login screen. This setting is disabled by default.

## Alerts

You can use API Manager to enable or disable alert notifications for specific events (for example, when an application request is created, or an organization is created). When an alert is generated by API Manager, you can execute a custom policy to handle the alert (for example, to send an email to an interested party, or to forward the alert to an external notification system).

You can use the alert settings in Policy Studio to select which policies are configured to handle each event. For more details, see [Configure API management alerts](/docs/apim_administration/apimgr_admin/api_mgmt_alerts/).

## Remote hosts

The remote host settings enable you to dynamically configure connection settings to back-end servers that are invoked by front-end APIs. API Administrators can edit all remote hosts in all organizations.

The remote host settings available in API Manager are a subset of the settings available in Policy Studio.

### Required settings

**Name**
: Enter the remote host name (for example, `www.google.com`).

**Port**
: Enter the TCP port to connect to on the remote host. Defaults to `80`.

**Maximum connections**
: Enter the maximum number of connections to the remote host. If the maximum number of connections is reached, the underlying API Gateway waits for a connection to drop or become idle before making another request. Defaults to `-1`, which means there is no limit.

**Organization**
: The organization to which the remote host belongs. This is only displayed for API administrators.

### Optional settings

**Allow HTTP 1.1**
: The underlying API Gateway uses HTTP 1.0 by default to send requests to a remote host. This prevents any anomalies if the destination server does not fully support HTTP 1.1. If the API Gateway is routing to a remote host that fully supports HTTP 1.1, you can use this setting to enable the API Gateway to use HTTP 1.1. This is disabled by default.

**Include Content-Length in request**
: When this option selected, the underlying API Gateway includes the `Content-Length` HTTP header in all requests to this remote host. This is disabled by default.

**Include Content-Length in response**
: When this option selected, the underlying API Gateway includes the `Content-Length` HTTP header in all responses to this remote host. This is disabled by default.

**Send SNI TLS extension to server**
: Adds a Server Name Indication (SNI) field to outbound TLS/SSL calls that shows the name the client used to connect. For example, this is useful if the server handles several different domains, and needs to present different certificates depending on the name the client used to connect. This is disabled by default.

**Verify server's certificate matches requested hostname**
: Ensures that the certificate presented by the server matches the name of the remote host connected to. This prevents host spoofing and man-in-the-middle attacks. This setting is enabled by default.

### Advanced settings

**Connection timeout**
: If a connection to this remote host is not established within the time specified in this field, the connection times out and fails. Defaults to `30000` milliseconds (30 seconds). This setting is required.

**Active timeout**
: When the API Gateway receives a large HTTP request, it reads the request off the network when it becomes available. If the time between reading successive blocks of data exceeds the **Active Timeout**, the API Gateway closes the connection. This prevents a remote host from closing the connection while sending data. Defaults to 30000 milliseconds (30 seconds).
: For example, the remote host's network connection is pulled out of the machine while sending data to the API Gateway. When the API Gateway has read all the available data off the network, it waits the **Active Timeout**
period before closing the connection.
: The **Active Timeout** value is also used as a wait time when the maximum number of connections for a remote host is reached. For example, when a remote host reaches the **Maximum connections** value, API Gateway waits the active timeout period before giving up on trying to make a new connection.

**Transaction timeout**
: A configurable transaction timeout that detects slow HTTP attacks (slow header write, slow body write, slow read) and rejects any transaction that keeps the worker threads occupied for an excessive amount of time. The default value is `240000` milliseconds. This setting is required.

**Idle timeout**
: The underlying API Gateway supports HTTP 1.1 persistent connections. The idle timeout is the time that API Gateway waits after sending a message over a persistent connection to the remote host before it closes the connection. Defaults to `15000` milliseconds (15 seconds). Typically, the remote host tells the API Gateway that it wants to use a persistent connection. The API Gateway acknowledges this, and keeps the connection open for a specified period of time after sending the message to the host. If the connection is not reused by within the Idle Timeout period, the API Gateway closes the connection. This setting is required.

**Include correlation ID in headers**
: Specifies whether to insert the correlation ID in outbound messages. This means that an `X-CorrelationID` header is added to the outbound message. This is a transaction ID that is attached to each message transaction that passes through API Gateway, and which is used for traffic monitoring in the API Gateway Manager web console. You can use the correlation ID to search for messages in the web console, and you can also access its value from a policy using the id message attribute. This setting is selected by default. This setting is enabled by default.
