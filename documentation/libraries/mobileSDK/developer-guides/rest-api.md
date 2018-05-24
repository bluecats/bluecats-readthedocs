---
layout: documentation
title: REST API
---

### Schema
All API access is over HTTPS, and accessed via the `https://api.bluecats.com` domain.  All data is sent and received as JSON.

### Version
BlueCats is currently on version three of our API. You must explicitly place `X-Api-Version: 3` in your request header to target version three. By default, all requests are directed at *version two* of the API (some of our customers have legacy apps which we still support).

### Authentication
Authentication with the BlueCats API follows the key/secret usage outlined in the [OAuth 2.0 Client Credentials Flow](https://tools.ietf.org/html/rfc6749#section-4.4). Don't leak your OAuth application's client secret to application users - this is intended for use in server to server scenarios.

Receive your bearer token by posting your your client ID and secret to `https://api.bluecats.com/token` using the `client_credentials` grant type.

If you are new to writing API-only apps, be sure to review our [guide](https://developer.bluecats.com/guides/testing-code-with-your-api-keys) on setting up your API-only app in the BlueCats [Dashboard](http://app.bluecats.com) and adding your API keys to your developer dashboard here.
