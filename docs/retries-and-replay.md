# Retries and replay

A delivery succeeds when the endpoint answers `2xx` within 10 seconds. Anything else is a failure.

## Retry schedule

Failed deliveries are retried 8 times: after 10 seconds, 1 minute, 5 minutes, 30 minutes, 2 hours, 6 hours,
12 hours and 24 hours. After the last failure the delivery is marked `exhausted`.

## Disabling an endpoint

An endpoint that fails every delivery for 3 days is disabled and its owner is e-mailed. Re-enable it from the
dashboard after fixing the cause.

## Replay

Operators can replay one delivery or every `exhausted` delivery of an endpoint from the dashboard or with
`POST /v1/deliveries/{id}/replay`. A replay sends the original event again with a new delivery id; receivers
must treat the event `id` as the idempotency key.

## Reading the delivery log

The log shows status code, latency and the first 1 KB of the response body for each attempt, for 30 days.
