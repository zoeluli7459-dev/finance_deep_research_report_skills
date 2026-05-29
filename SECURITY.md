# Security

Do not commit real API keys, cookies, tokens, `.env` files, generated query outputs, or local caches.

## Eastmoney MX API Key

These skills read the Eastmoney MX key only from the runtime environment:

```bash
export MX_APIKEY="replace-with-your-own-key"
```

The repository must contain only the variable name `MX_APIKEY` and placeholder examples, never a real value.

Before publishing, run:

```bash
if [ -n "$MX_APIKEY" ]; then
  rg -F "$MX_APIKEY" .
fi
```

No output means the current key is not present in the repository.
