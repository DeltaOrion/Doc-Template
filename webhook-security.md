---
layout: default
title: Security
parent: Webhooks
nav_order: 1
---

# Security

This is the description of the security requirements for webhooks.

## Authentication

Webhooks use the API key authentication scheme. This means that whenever we make an HTTPS request to your backend, we will include your API key as one of the header values. You can then use this API key to verify that the request really came from us.

Whenever you <a href="/#register-a-webhook">register</a> a webhook, you will be given your API key. This is your only chance to download it. You should safely store your API key and ensure it is not shared with others. It is crucial to make sure that your API key isn't available on public repositories like GitHub or in client-side code.

{: .important}
You will only have one chance to save your API key. If you exit the page, you will not be able to view it again.

Whenever we push events to your webhook endpoint, we will include the API key in the `x-rapid-logix-key` request header. You should verify that the API key on the request matches the one you received when you registered the webhook. 

{: .warning}
It is important to check that the API Key received on the request is correct. If you see an incorrect API key you should drop the request.

## HTTPS

The RapidLogix webhooks only support the HTTPS protocol to ensure that all data transmitted between our servers and your endpoint is encrypted and secure. This helps prevent man-in-the-middle attacks and ensures data integrity.
