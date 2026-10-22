# Pigeon overview

Pigeon receives events from your application and delivers them to the HTTPS endpoints your customers register.
It retries failed deliveries, signs every request and keeps a delivery log for 30 days.

## Concepts

- **Endpoint**: an HTTPS URL that receives events. Each endpoint has its own signing secret.
- **Event**: a JSON document with a `type` such as `invoice.paid` and a unique `id`.
- **Delivery**: one attempt to send an event to one endpoint. An event can have several deliveries.
- **Subscription**: the list of event types an endpoint wants. An endpoint with no subscription gets nothing.

## Who uses what

Developers send events with the API and verify signatures. Operators watch delivery health, replay failed
deliveries and rotate secrets. Both roles read the same delivery log.
