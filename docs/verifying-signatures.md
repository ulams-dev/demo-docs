# Verifying signatures

Every delivery carries two headers: `Pigeon-Timestamp` (Unix seconds) and `Pigeon-Signature`
(`v1=<hex>`). The signature is an HMAC-SHA256 of `<timestamp>.<raw body>` with the endpoint's signing secret.

1. Read the raw body, before any JSON parsing.
2. Reject the request when the timestamp is more than 5 minutes away from your clock.
3. Compute the HMAC and compare it with a constant-time comparison.
4. Answer `2xx` only after the signature is valid.

```js
const expected = crypto.createHmac("sha256", secret).update(`${timestamp}.${rawBody}`).digest("hex");
const ok = crypto.timingSafeEqual(Buffer.from(expected), Buffer.from(signature.slice(3)));
```

A secret can be rotated without downtime: for 24 hours after rotation both the old and the new secret sign
each delivery, in two `v1=` values separated by a comma.
