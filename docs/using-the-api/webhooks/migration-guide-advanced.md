---
title: "Webhooks migration guide for complex scenarios"
sidebar_label: "Advanced scenarios"
description: "Learn about the additional impact of transitioning to our new service based on your specific setup"
---

## Should I migrate to the new service and endpoints?

If you are considering using Codat's new webhook management endpoints, check that your setup is compatible first. Find the scenarios applicable to your existing setup and see if you can migrate.

| I am using...                                                                    | Action and impact                                                                                                                                                                                                                                                                                                |
|----------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ...email notification functionality                                              | - Do not migrate to the new webhook service <br/> - Review the [critical considerations](/using-the-api/webhooks/migration-guide-advanced#email-notification-functionality)                                       |
| ...event log endpoints (e.g. `/rules/alerts`)                                    | - Development work required <br/> - Do not migrate until you have removed all calls to logs endpoints from your application logic <br/> - Review the [critical considerations](/using-the-api/webhooks/migration-guide-advanced#event-log-endpoints)                                                                                             |
| ...`RuleId` in my application's existing logic                                   | - Development work required <br/> - Do not migrate until you have removed all application logic using the `RuleId` property <br/> - Review the [critical considerations](/using-the-api/webhooks/migration-guide-advanced#ruleid-in-existing-logic)                                                                                        |
| ...only company-agnostic webhook functionality                                   | - Request to migrate to the new webhook service <br/>                                                                                                                                                                |
| ...company-specific webhook functionality                                        | - Request to migrate to the new webhook service <br/> - Review the [additional considerations](/using-the-api/webhooks/migration-guide-advanced#company-specific-webhooks)                                                                                 |
| ...`X-Codat-ClientId` header to determine the source Codat instance of the event | - Request to migrate to the new webhook service <br/> - Configure a custom header, as described in [additional considerations](/using-the-api/webhooks/migration-guide-advanced#source-client-header)                 |
| ...webhook auth header via the `/profile` endpoint                               | - Request to migrate to the new webhook service <br/> - Configure a custom header using the Portal, as described in [additional considerations](/using-the-api/webhooks/migration-guide-advanced#webhook-auth-header) |
| ...webhook auth header via the Portal                                            | - Request to migrate to the new webhook service <br/> - Configure a custom header using the Portal, as described in [additional considerations](/using-the-api/webhooks/migration-guide-advanced#webhook-auth-header) |
| ...`Retry-After` header to control the time between retries                      | - Request to migrate to the new webhook service <br/> - Review the [additional considerations](/using-the-api/webhooks/migration-guide-advanced#retry-after-header)                                                   |

## Critical considerations

### Event log endpoints

Our new service has a robust [retry policy](/using-the-api/webhooks/troubleshooting#retry-policy) that ensures we attempt to deliver an event multiple times over a 28-hour period. This means you don't need to write any complex logic to manage undelivered events. As a result, you no longer need to use any of these endpoints:

- `GET /rules/alerts`
- `GET /rules/{ruleId}/alerts`
- `GET /rules/alerts/{alertId}`

Check out the [deprecation notice](/updates/240306-deprecation-rules-alerts) for full details of these changes. 

### Email notification functionality  

Our new webhooks service does not currently support sending email notifications for events. Remain on our existing service and we will get in touch with an alternative solution.

### `RuleId` in existing logic

If you are using `RuleId` properties returned by our existing webhooks in your application logic, please remain on our existing service until you have changed your application's logic to avoid using the `RuleId`.

## Additional considerations

Depending on your setup, you may need to configure your webhook consumers in Codat further or be aware of changes to the UI. 

### IP allowlist

If your consumer endpoint is behind a firewall or NAT, make sure to allow messages from `4.159.114.108`.

### Source client header

If you are using multiple Codat instances and need to differentiate between them, you can filter the messages by client. 

Add a custom `X-Codat-ClientId` header with the required client ID to the webhook endpoint using our Portal. For more information on creating custom headers in webhook consumers, see [Custom headers](/using-the-api/webhooks/create-consumer#custom-headers).

You can find and copy your client ID in the [Portal](https://app.codat.io) by clicking on your instance dropdown.

![A fragment of the UI that displays the dropdown with client instances and current client details](/img/use-the-api/0049-clientid-portal.png)

### Webhook auth header

If you are currently securing your webhook endpoints with an authorization header, you can add it as a custom `Authorization` header to the webhook consumer endpoint using our Portal. It is no longer possible to do this via our API.

For more information on creating custom headers in webhook consumers, see [Custom headers](/using-the-api/webhooks/create-consumer#custom-headers).

### `Retry-After` header

Our new service has a robust [retry policy](/using-the-api/webhooks/troubleshooting#retry-policy) that ensures we attempt to deliver an event multiple times over a 28-hour period. As a result, you no longer need to set a `Retry-After` header to control the time between retries. 

### Company-specific webhooks

With the new service, you are able to see the `companyId` in the **Channels** field of the endpoint's detailed view or the message log. 

![A fragment of the UI that displays the Channels column of the message log with the company ID value recorded in it](/img/use-the-api/0047-message-channels.png)

---

## Read next

- [Event types](/using-the-api/webhooks/event-types)
- [Configure webhook consumers](/using-the-api/webhooks/create-consumer)
- [Troubleshooting](/using-the-api/webhooks/troubleshooting)
