# Installing the apipj MCP server (for AI agents such as Cline)

apipj is a **remote** MCP server. There is nothing to clone, build or run: only add the server URL to the client configuration.

- URL: `https://apipj.com.br/mcp`
- Transport: Streamable HTTP
- Tools: `consultar_cnpj` (company record by CNPJ, needs an account), `validar_cnpj` and `status_base` (no account needed)

## Cline

Add this entry to `cline_mcp_settings.json` (Cline → MCP Servers → Configure → "Configure MCP Servers"):

```json
{
  "mcpServers": {
    "apipj": {
      "type": "streamableHttp",
      "url": "https://apipj.com.br/mcp",
      "disabled": false,
      "autoApprove": ["validar_cnpj", "status_base"]
    }
  }
}
```

`validar_cnpj` and `status_base` work right away. To use `consultar_cnpj`:

1. If the client supports MCP OAuth, it opens the apipj sign-in page on the first lookup: sign in with Google (or e-mail and password) and click **Autorizar**.
2. Otherwise, ask the user to create a free account and an API key at https://apipj.com.br/criar-conta, then add the header to the same entry:

```json
"headers": { "Authorization": "Bearer apipj_live_YOUR_KEY" }
```

Never print, log or commit the API key.

## Verify

Call `status_base` (no arguments). A successful response includes `status: "OK"` and the loaded `dataset_month`. Then call `validar_cnpj` with `{"cnpj": "12.ABC.345/01DE-35"}`; the response has `valido: true`.

## Notes

- All tools are read-only.
- Each `consultar_cnpj` call counts toward the account's per-minute limit and monthly quota.
- Docs: https://apipj.com.br/docs · MCP guide: https://apipj.com.br/guias/consultar-cnpj-ia-mcp
