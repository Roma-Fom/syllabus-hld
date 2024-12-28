# Sign and send webhooks

## Overview

This guide explains how to sign and send webhooks to the webhook service.

### Keys

- Server key 32 bytes length: 1d28023fc6320b42781a4d811465e3a23b2a4f21340e27c06e683c1e987ed5f9
- Webhook secret (nanoId): whsec_yxrTwIuHnHs9Wwb9LdYD6

### Steps first we ecrypt the secret, return data after encypt example

```json
{
  "iv": "e21b8662eb68e7f031663e85",
  "ciphertext": "b029e0c3c1557db59b035b5a1c377ca68148950acc49bc17c7e15db25dd63421a8ad55bb7e9960a8fb36a3"
}
```

### Then we hash she secret, return data after hash example

```json
{
  "hash": "2dae15292d45e7ac7aadea0ddc9d783f8fe6a0f50652851668e7a814bfe8dbd0"
}
```

### We store in db the secret encrypted and hashed

```json
{
  "id": "secret_nOel975Je0Jbv5qkjalaT",
  "secret": "e21b8662eb68e7f031663e85.b029e0c3c1557db59b035b5a1c377ca68148950acc49bc17c7e15db25dd63421a8ad55bb7e9960a8fb36a3",
  "hash": "2dae15292d45e7ac7aadea0ddc9d783f8fe6a0f50652851668e7a814bfe8dbd0",
  "webhookId": "wh_9rrQCcj3AiHoMlAdDvo90"
}
```

## Sign and send

### Send request example

- Example payload from api consumer (lets assume some payment)

```json
    "type": "payment.created",
    "data": {
        "id": "seti_1NG8Du2eZvKYlo2C9XMqbR0x",
        "object": "setup_intent",
        "application": null,
        "automatic_payment_methods": null,
        "cancellation_reason": null,
        "created": 1686089970,
        "customer": null,
        "description": null,
        "flow_directions": null,
        "last_setup_error": null,
        "latest_attempt": null,
        "livemode": false,
        "mandate": null,
        "metadata": {},
        "next_action": null,
        "on_behalf_of": null,
        "payment_method": "pm_1NG8Du2eZvKYlo2CYzzldNr7",
        "payment_method_options": {},
        "payment_method_types": ["card"],
        "single_use_mandate": null,
        "status": "requires_confirmation",
        "usage": "off_session"
    }

```

- This is how we store the message in db

```json
{
  "id": "evt_TirU2gmgTkd40HH7zhojY",
  "type": "payment.created",
  "appId": "app_9rrQCcj3AiHoMlAdDvo90",
  "createdAt": 1735420916804,
  "data": {
    "id": "seti_1NG8Du2eZvKYlo2C9XMqbR0x",
    "object": "setup_intent",
    "application": null,
    "automatic_payment_methods": null,
    "cancellation_reason": null,
    "created": 1686089970,
    "customer": null,
    "description": null,
    "flow_directions": null,
    "last_setup_error": null,
    "latest_attempt": null,
    "livemode": false,
    "mandate": null,
    "metadata": {},
    "next_action": null,
    "on_behalf_of": null,
    "payment_method": "pm_1NG8Du2eZvKYlo2CYzzldNr7",
    "payment_method_options": {},
    "payment_method_types": ["card"],
    "single_use_mandate": null,
    "status": "requires_confirmation",
    "usage": "off_session"
  }
}
```

- Sign the payload

```json
{
  // this fields go to the headers
  "timestamp": 1735421209275, // timestamp when we sign the payload
  "signature": "v1,f87a2400817c4ac9cc8f67a1334c8849e66a290399a8933efed1db51dfe1fc41",
  // this fields go to the body
  "event": {
    "id": "evt_TirU2gmgTkd40HH7zhojY",
    "type": "payment.created",
    "timestamp": 1735420916804, // timestamp from message stored in db
    "data": {
      "id": "seti_1NG8Du2eZvKYlo2C9XMqbR0x",
      "object": "setup_intent",
      "application": null,
      "automatic_payment_methods": null,
      "cancellation_reason": null,
      "created": 1686089970,
      "customer": null,
      "description": null,
      "flow_directions": null,
      "last_setup_error": null,
      "latest_attempt": null,
      "livemode": false,
      "mandate": null,
      "metadata": {},
      "next_action": null,
      "on_behalf_of": null,
      "payment_method": "pm_1NG8Du2eZvKYlo2CYzzldNr7",
      "payment_method_options": {},
      "payment_method_types": ["card"],
      "single_use_mandate": null,
      "status": "requires_confirmation",
      "usage": "off_session"
    }
  }
}
```
