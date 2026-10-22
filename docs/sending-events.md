# Sending events

Send an event with one request:

```http
POST /v1/events
Authorization: Bearer <api key>
Content-Type: application/json

{ "type": "invoice.paid", "id": "evt_1042", "data": { "invoice": "inv_77", "amount": 4900 } }
```

The response is `202 Accepted` as soon as the event is stored. Delivery happens afterwards.

## Idempotency

The `id` is the idempotency key. Sending the same `id` twice within 24 hours stores the event once and returns
`200 OK` with the original delivery ids. Use a stable id from your own system, not a random one per attempt.

## Limits

- Body size: 256 KB.
- Rate: 100 requests per second per API key. Above that Pigeon answers `429` with a `Retry-After` header.
- Event types: lowercase letters, digits and dots, at most 64 characters.
