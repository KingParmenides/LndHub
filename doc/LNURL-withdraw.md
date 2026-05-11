# LNURL Withdraw Links

Authenticated LNDHub users can create expiring LNURL-withdraw links that reserve funds until they are claimed, canceled, or expired.

Set `baseUrl` in `config.js` or `CONFIG` when LNDHub is behind a reverse proxy and generated links should use a public URL that differs from the request host.

## Create a Link

`POST /lnurlwithdraw/create`

Headers:

`Authorization: Bearer <access_token>`

Body:

```json
{
  "amount": 1000,
  "memo": "Gift sats",
  "expiry": 86400
}
```

Response:

```json
{
  "id": "8e8f...",
  "status": "active",
  "amount": 1000,
  "memo": "Gift sats",
  "created_at": 1760000000,
  "expires_at": 1760086400,
  "url": "https://example.com/lnurlwithdraw/8e8f...",
  "lnurl": "LNURL1..."
}
```

The link URL also renders a simple browser page with a QR code, wallet deep link, and BlueWallet download link. LNURL wallets receive the standard `withdrawRequest` JSON from the same URL.

## List Links

`GET /lnurlwithdraw`

Headers:

`Authorization: Bearer <access_token>`

## Cancel a Link

`POST /lnurlwithdraw/:id/cancel`

Headers:

`Authorization: Bearer <access_token>`

Canceling an active link releases its reserved balance. Expired links are automatically ignored by the balance calculation.
