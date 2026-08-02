# THORChain Agent Rules

This is the public agent contract for thorchain.org.

## Scope

- Use these rules for public website discovery, documentation lookup, and read-only THORChain resource navigation.
- Treat protocol state, pool data, fees, pricing, and endpoint availability as live external data that must be rechecked at request time.
- Do not treat this website contract as permission to trade, sign transactions, move funds, submit governance, or access private systems.

## Preferred Sources

- Site summary: https://thorchain.org/llms.txt
- Expanded resource map: https://thorchain.org/llms-full.txt
- OpenAPI discovery: https://thorchain.org/openapi.json
- MCP server card: https://thorchain.org/.well-known/mcp-server-card.json
- Read-only MCP endpoint: https://thorchain.org/mcp
- Developer documentation: https://dev.thorchain.org/
- Protocol documentation: https://docs.thorchain.org/

## Safety Rules

- Prefer official THORChain documentation for protocol behavior.
- Prefer linked Swagger or API documentation for endpoint-specific request and response shapes.
- Say when data is stale, cached, unavailable, or inferred.
- Do not give financial, investment, tax, or legal advice.
- Do not invent endpoint capabilities. If a capability is not documented, call it unknown.
- Respect robots.txt and Content-Signal policy.

## MCP Contract

- The public MCP endpoint is read-only.
- Tools expose discovery text, resource maps, and public API references.
- Tools do not mutate state, submit transactions, manage accounts, or bypass authentication.
- Clients should expect public API providers to rate-limit or change availability.