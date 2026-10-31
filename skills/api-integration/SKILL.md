POST   /api/v1/webhooks          ; register subscriber URL + events
GET    /api/v1/webhooks          ; list subscriptions
DELETE /api/v1/webhooks/{id}     ; unsubscribe
POST   /api/v1/webhooks/{id}/test; fire test event
GET    /api/v1/webhooks/{id}/deliveries; delivery history + status