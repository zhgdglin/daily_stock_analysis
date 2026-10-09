# ChatGPT Runtime Integration Boundary

## Current state

The repository's functionality is implemented as Python modules and Claude Code command skills. A ChatGPT Skill is an instruction package; importing `SKILL.md` does not grant code execution, network access to the repository, access to local files, or persistent storage.

This adaptation currently provides the conversational instruction layer only. It does not claim to be an API, MCP server, or deployed ChatGPT app.

## Proposed tools

Expose a deliberately small, read-only API first:

| Tool | Inputs | Output |
|---|---|---|
| `screen_stocks` | region, preset, optional sector/theme, result limit | candidates, metric values, currency, source, retrieved-at timestamp, warnings |
| `stock_report` | ticker | company/fund metrics, source timestamps, missing-data fields |
| `portfolio_analysis` | positions and optional weights/currency | allocation and concentration metrics; no account lookup |
| `stress_test` | positions, weights, named scenario and explicit assumptions | scenario estimates, assumptions, limitations |
| `market_research` | subject type and query | sourced research with publication/retrieval dates |

Use explicit JSON schemas, bounded inputs, result limits, request timeouts, and structured errors. Include data provenance and freshness on every market-data response. Keep stateful note/watchlist storage out of the first iteration unless users explicitly opt in and have a clear deletion/export path.

## Security and safety

- Keep provider keys on the server in a secret manager or environment configuration; never accept arbitrary shell commands from a model.
- Validate tickers, regions, presets, numeric ranges, and portfolio size. Apply rate limits and outbound network restrictions.
- Do not expose Neo4j or database credentials to the model. Implement narrowly scoped, parameterized queries.
- Make read-only operations the default. Require explicit confirmation for any persistent write; never provide trade execution.
- Return source URLs, source timestamps, quote currency, and stale/missing-data indicators.
- Log minimally and avoid retaining user portfolio data unless retention is disclosed and needed.

## Deployment checklist

1. Choose an API or MCP transport supported by the target ChatGPT workspace and verify current account requirements.
2. Implement the tool schemas and call the existing core through stable application-level functions, not ad-hoc shell strings.
3. Add authentication, secrets handling, input validation, rate limiting, and observability.
4. Add unit and integration tests using mocked market data, including stale, partial, and failed responses.
5. Deploy to an HTTPS endpoint and configure the ChatGPT app/connector or GPT Action.
6. Run end-to-end tests and confirm the assistant accurately describes data dates, limitations, and any persistent side effects.
