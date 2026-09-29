# Augustus Banking API sandbox flows

Import these files into Postman and run one folder at a time.

- `augustus-v1.postman_collection.json`
- `augustus-sandbox.postman_environment.json`

## Import

1. Open Postman.
2. Select **Import**.
3. Select both JSON files.
4. Confirm that the **Augustus Banking API Sandbox Flows** collection and **Augustus Sandbox** environment appear.
5. Select the **Augustus Sandbox** environment.
6. Set `apiKey` to a sandbox API key.
7. Set `webhookUrl` to a public HTTPS receiver only when running the Webhook folder. A temporary inbox such as `https://webhook.site` is enough.
8. Leave `cleanupAccount` set to `true`, or set it to `false` to inspect the generated account after the run.

Use a sandbox API key. Every request accepts only `https://api.sandbox.augustus.com` or an HTTP(S) loopback origin such as `http://localhost:3000`.

## Run a flow

1. Open the collection.
2. Select the menu for one of the **ACH**, **Fedwire**, **SWIFT**, or **Webhook** folders.
3. Select **Run folder**.
4. Keep the requests in their generated order and start the run.

Run rail folders independently. Each rail flow creates a fresh USD operating account, simulates a USD 100.00 deposit, sends a USD 21.21 payout, and validates balances and events. With cleanup enabled, it then freezes the account, drains and settles the residual balance, verifies a zero balance, and closes the account. Polling can make a run take several minutes.

The Webhook folder is skipped when `webhookUrl` is empty. When configured, it creates a subscription, sends a `ping.test` event, and validates successful delivery. The receiver must return a 2xx status.

Required rail scopes are `simulations:write`, `accounts:read`, `counterparties:write`, `payouts:read`, `payouts:write`, `deposits:read`, and `events:read`. The Webhook folder also requires `webhook_subscriptions:write` and `webhook_deliveries:read`. The `full_access` alias is sufficient.

Cleanup is best-effort. A failure before the cleanup steps can leave a generated account open. Closed accounts and created counterparties remain in sandbox history.
