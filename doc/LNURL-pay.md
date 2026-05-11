# LNURL-pay tipping

Authenticated LNDHub users can claim a public username and share an LNURL-pay endpoint for tips. Tips create normal internal LNDHub invoices for the receiving user, but unpaid tip invoices are hidden from the wallet invoice list to avoid clutter.

## Claim or fetch username

`GET /lnurlpay`

Returns the current user's tipping username, URL, and encoded LNURL. If the user has not claimed a username yet, `username` is `false`.

`POST /lnurlpay/username`

Body:

```json
{
  "username": "alice"
}
```

Usernames are normalized to lowercase and must match `[a-z0-9][a-z0-9_-]{2,31}`.

Response:

```json
{
  "username": "alice",
  "url": "https://example.com/lnurlpay/alice",
  "lnurl": "LNURL1..."
}
```

## Public LNURL-pay endpoint

`GET /lnurlpay/:username`

Returns LNURL-pay metadata:

```json
{
  "tag": "payRequest",
  "callback": "https://example.com/lnurlpay/alice/callback",
  "minSendable": 1000,
  "maxSendable": 10000000000,
  "metadata": "[[\"text/plain\",\"Tip alice on LNDHub\"],[\"text/identifier\",\"alice@example.com\"]]",
  "commentAllowed": 256
}
```

## Callback

`GET /lnurlpay/:username/callback?amount=1000&comment=Thanks`

`amount` is millisatoshis and must be a whole-satoshi amount. The callback returns:

```json
{
  "pr": "lnbc...",
  "routes": []
}
```

## Push notification support

When a paid invoice notification is forwarded to GroundControl, the payload now includes `lndhub_userid` when the payment hash belongs to a LNDHub user. `GET /userid` returns the authenticated user's internal id so clients can subscribe to user-level payment notifications.
