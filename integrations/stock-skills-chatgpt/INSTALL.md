# ChatGPT Stock Skills

This directory adapts the stock-skills analysis workflows into a ChatGPT Skill. It is intentionally clear about the difference between an instruction skill and an executable integration.

## Import as a ChatGPT Skill

1. Download this repository or the `chatgpt/stock-investment-analyst` folder.
2. Create a ZIP whose top-level folder is `stock-investment-analyst/` and whose contents include `SKILL.md`. Do not zip the entire repository unless you intend to attach the full source as reference material.
3. In ChatGPT, open **Settings → Personalization → Skills** (the exact menu can differ by account/workspace), choose **Create** or **Upload**, and upload the ZIP. If Skills is unavailable on your plan or workspace, use the GPT configuration fallback below.
4. Start a new chat and ask, for example, “Analyze AAPL using the stock-investment-analyst skill.”

Skill availability and import UI can vary by plan, region, and workspace policy. Consult the current ChatGPT help center if the menu is absent.

## GPT configuration fallback

If your account does not expose Skills, create a custom GPT and paste the contents of `SKILL.md` into its Instructions. You may add selected repository documentation as Knowledge, but uploaded source files do not execute Python and do not provide live market data.

## What works without an integration

The skill guides conversational analysis, user-provided-data calculations, research structure, scenario framing, and investment-note drafting. It does **not** itself:

- Run `src/` Python code or the original `.claude/skills` scripts.
- Query Yahoo Finance/yfinance, current news, X/Grok, or market APIs.
- Read this repository's local portfolio, watchlists, or Neo4j database.
- Persist trades, watchlists, or notes outside the conversation.

For current facts, use ChatGPT search/data tools when available and cite the sources and as-of date. Never represent a qualitative answer as a live screener result.

## Enabling executable stock-skills functionality

To run the original calculations and data clients from ChatGPT, deploy a service that wraps selected repository functions and expose it through an authenticated API or a supported MCP app. The integration needs:

- A reachable HTTPS endpoint and an explicit deployment/runtime strategy.
- Authentication and secret management; never put API keys in the Skill, GPT instructions, or a public repository.
- A narrow, validated tool surface (for example: `screen_stocks`, `stock_report`, `portfolio_analysis`, `stress_test`).
- A ChatGPT connector/app or GPT Action configured for that endpoint. Availability depends on ChatGPT plan and workspace settings.
- User consent and confirmation for any state-changing operation; this repo adapter should not execute trades.
- Tests for schemas, input validation, timeouts, data freshness, and failure behavior.

There is no production API/MCP server in this directory yet. The original project is a local Python/Claude Code system, so the Skill import alone cannot activate its executable features. See [INTEGRATION.md](INTEGRATION.md) for the proposed integration boundary.

## Packaging locally

From the repository root, create a ZIP with the skill folder at its root:

```powershell
Compress-Archive -Path .\chatgpt\stock-investment-analyst -DestinationPath .\chatgpt\stock-investment-analyst.zip -Force
```

The ZIP can be uploaded from `chatgpt/stock-investment-analyst.zip`.
