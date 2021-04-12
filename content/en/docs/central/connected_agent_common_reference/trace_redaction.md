---
title: Trace redaction
linkTitle: Trace redaction
draft: false
weight: 10
description: "Understand how the Traceability Agent will redact and sanitize
  information that is sent to Amplify Central.  Learn how you can control the
  information that is sent to Amplify Central by using the redaction
  configuration. "
---
## Before you start

* Learn the Regular Expression syntax supported by the agent. [RE2 Syntax](https://github.com/google/re2/wiki/Syntax)

## Objectives

Learn how to to set up redaction and sanitization rules used to ALLOW transaction path, query arguments, request headers, and response headers when sending data to Amplify Central.

## Path show rules

Parts of the URI path will be redacted before the info is sent to Amplify Central.  When this is done the Traceability Agent replaces the path value with "{*}" in the transaction details.

By default all things are redacted and rules must be set to show the path elements.  When using environment variables the variable name is `TRACEABILITY_REDACTION_PATH_SHOW`.

```ini
# Send all path values, no redactions
TRACEABILITY_REDACTION_PATH_SHOW=[{keyMatch:".*"}]
```

Ex. `https://somehost.com/pathof/my/api/uses/thispath` is redacted to `https://somehost.com/pathof/my/api/uses/thispath`

Separate by comma to provide multiple allowed matches.

```ini
# Send paths that start with path or paths that end with path
TRACEABILITY_REDACTION_PATH_SHOW=[{keyMatch:"^path"},{keyMatch:"path$"}]
```

Ex. `https://somehost.com/pathof/my/api/uses/thispath` is redacted to `https://somehost.com/pathof/{*}/{*}/{*}/thispath`

## Query argument and header show rules

Query argument and header show rules work much like the path rules above but only match the key portion and not the value.  When a key does not match a show rule that key and value is completely removed from the transaction details.

The environment variable names are `TRACEABILITY_REDACTION_QUERYARGUMENT_SHOW`, `TRACEABILITY_REDACTION_REQUESTHEADER_SHOW`, and `TRACEABILITY_REDACTION_RESPONSEHEADER_SHOW`

```ini
# Send only query arguments with the key 'id'
TRACEABILITY_REDACTION_QUERYARGUMENT_SHOW=[{keyMatch:"^id$"}]

# Send request headers that have 'header' anywhere in the key
TRACEABILITY_REDACTION_REQUESTHEADER_SHOW=[{keyMatch:"header"}]

# Send all response headers
TRACEABILITY_REDACTION_RESPONSEHEADER_SHOW=[{keyMatch:".*"}]
```

## Query argument and header value sanitization rules

In addition to using the show rules for query arguments and headers it is also possible to sanitize the values of certain keys.  Sanitization is only applied to values that match a show rule and then a sanitization rule.

The environment variable names are `TRACEABILITY_REDACTION_QUERYARGUMENT_SANITIZE`, `TRACEABILITY_REDACTION_REQUESTHEADER_SANITIZE`, and `TRACEABILITY_REDACTION_RESPONSEHEADER_SANITIZE`.

Much like the show rules, the sanitization rules have a keyMatch which is used to match the argument or header key.  When a keyMatch is found the additional valueMatch expression is applied to the value and any matching portions are replaced with "{*}".

The following examples assume that the key has already passed one or more show rules. Also assume all path values are allowed.

```ini
# Sanitize the whole value of the `id` query argument
TRACEABILITY_REDACTION_QUERYARGUMENT_SANITIZE=[{keyMatch:"^id$",valueMatch:".*"}]
```

Ex. `https://somehost.com/api?id=12345` is sanitized to `https://somehost.com/api?id={*}`

```ini
# Sanitize the first five characters of any header with a key that has 'header' in it
TRACEABILITY_REDACTION_REQUESTHEADER_SANITIZE=[{keyMatch:"header",valueMatch:"^.*{0,5}"}]
```

Ex. `x-header-example=header-value` is sanitized to `x-header-example={*}r-value`

```ini
# Sanitize the word password, wherever it is found, of any header that starts with 'response'
TRACEABILITY_REDACTION_RESPONSEHEADER_SANITIZE=[{keyMatch:"^response",valueMatch:"password"}]
```

Ex. `response-header=mypasswordissafe` is sanitized to `response-header=my{*}issafe`
