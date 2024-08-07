---
layout: default
title: Webhooks
nav_order: 1
has_children: true
---

# Webhooks

Webhooks are a way for applications to receive real-time notifications about events happening in your organisation. When building Rapid Logix integrations, you might want your applications to receive events as they occur in your organisation, so that your backend system can execute actions automatically.

To enable webhook events, you need to register them on the dashboard. After you register them, RapidLogix can push real-time event data to your application's webhook endpoint when events happen in your organization. RapidLogix uses [HTTPs](webhook-security#https) to send webhook events to your app as a JSON payload that includes the [Event Object](webhook-payload).

## Register a Webhook

Any user with the super admin or organisation admin role can register a webhook. Webhooks can be registered on the <a href="https://app.rapidlogix.com/webhooks" target="_blank">dashboard</a>. You can register a webhook as follows:

{: .warning }
Only users with the super admin and organisation admin role can register webhooks.

1. Log in to the <a href="https://app.rapidlogix.com/webhooks" target="_blank">Rapid Logix Dashboard</a> using your credentials.
2. Click the Create button, located on the top right of the main table.
3. Fill in the form with your webhook details.

This is a list of the fields on the create webhook form and what they mean:

### Fields
---
**Name\*** \
A short name that allows you to identify the webhook.

---
**URL\***\
The HTTPS endpoint that you want to receive real-time event data from.

---
**Description**\
A longer description that gives you more information about the endpoint.

---
**Status\***\
Describes the current behavior of the webhook.

| Status   | Description                                           |
|----------|-------------------------------------------------------|
| Enabled  | The webhook will actively push real-time updates to the endpoint |
| Disabled | The webhook will NOT push any real-time updates       |

---
**Events\***\
A list of events you want to subscribe to. If you subscribe to multiple events, then the endpoint will receive all of them. A list of the events and what they do can be found [here](#types-of-events).

---

## API Keys

After you register your webhook, you will be given your [API Key](webhook-security#authentication) for authentication. You should use the API key to authenticate that the request really came from us. This is your only chance to download it. You should safely store your API key and should not share it with others. You should ensure that your API key isn't available on public git repositories like GitHub, in client-side code, and so forth. You can find more information on webhook security [here](./webhook-security).

{: .important}
You will only have one chance to save your API key. If you exit the page, you will not be able to view it again.

## Types of Events

You receive events for all of the event types your webhook endpoint is listening for in your configuration. Use the `eventType` field to determine what type of event you received. The `data.object` field contains the corresponding resource.

This is a list of all the types of events we send. This list is not permanent and may change at any time. All events follow the pattern `{resource}.{event}`. For example, for the event `job.completed`, the resource is the job and the event is job completion.

### Event

---

**job.completed**\
Occurs when a job is completed.

---

## Event Payload

The [Event Payload](webhook-payload) is the JSON object you receive when a webhook triggers. The `data.object` contains a snapshot of the event. The following event shows a sample event for a `job.completed` event with the `data` omitted for brevity.

```json
{
  "id": "63070be0372f1e7bc77962a6",
  "webhookId": "66b2e3ddb97f60b5270d5ce9",
  "organisationId": "657bf08c202997ba9d127922",
  "eventType": "job.completed",
  "dateTime": "2024-10-05T00:00:00Z",
  "data": {
    "object": {
        "objectType": "job",
    }
  },
  "version": 1
}
```