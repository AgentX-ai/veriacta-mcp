# VeriActa: EU and national law for AI assistants

<!--
  Generated from the VeriActa MCP server's tool table and published with each
  release of its registry listing. Changes made here are overwritten.
-->

Official EU and national law for AI assistants: cited provisions as in force on any date.

VeriActa is an MCP server and legislation API that gives AI assistants and AI agents the official text of EU law and the law of Poland, Spain, Germany, Denmark, the Netherlands and Ireland. Connect it to Claude, ChatGPT or any other AI assistant that supports MCP connectors, ask a tax or legal question, and the answer quotes the provisions in force on the date you mean, in full, with their citation and a link to the official source.

Sources: EU law from EUR-Lex, Polish law from the Dziennik Ustaw (ISAP), Spanish law from the BOE, German federal law from Gesetze im Internet, Danish law from Retsinformation, Dutch law from wetten.overheid.nl and Irish law from the Law Reform Commission's Revised Acts. Ask about today or a past date: every wording in the official history is kept, and a date the history does not cover is flagged, never guessed. Cross-border compare puts an article of an EU directive or regulation next to the national provisions that carry it.

For Poland it also finds PKD activity codes and PKWiU product and service symbols, reads official guidance (ZUS, the Ministry of Finance, eureka tax rulings), and checks a company in the public registers: VAT status, white-list bank accounts, the KRS entry and who may represent it.

Five read-only tools, with the country as a parameter. Developers get the same law from the REST API (`/v1/law`). Sign in with OAuth or an API key; a new account gets 200 free credits. VeriActa gives legal information from official sources, not legal advice.

## Countries

- **Poland** (`PL`): law, classifications (PKD, PKWiU), official guidance, company registers
- **Spain** (`ES`): law
- **European Union** (`EU`): law
- **Germany** (`DE`): law
- **Denmark** (`DK`): law
- **Netherlands** (`NL`): law
- **Ireland** (`IE`): law

There is nothing to install or run on your machine. The server is hosted at `https://veriacta.ai/mcp`, and this repository
holds its listing in the MCP Registry.

## Connect

- **Endpoint:** `https://veriacta.ai/mcp`
- **Transport:** streamable HTTP
- **Sign-in:** OAuth 2.1 with dynamic client registration, or an API key sent as a bearer token

You need a VeriActa account (200 free credits at sign-up): https://veriacta.ai/account

### Claude, ChatGPT and other assistants with connectors

Add a custom connector (or custom MCP server) with the address `https://veriacta.ai/mcp`. The assistant opens a VeriActa page
where you sign in or create an account and allow the app. There is no key to copy, and you can disconnect the app
on your account page.

### Claude Code

```bash
claude mcp add --transport http veriacta https://veriacta.ai/mcp
```

### VS Code and GitHub Copilot

Add it to `.vscode/mcp.json`:

```json
{
  "servers": {
    "veriacta": {
      "type": "http",
      "url": "https://veriacta.ai/mcp"
    }
  }
}
```

Start the server and VS Code opens a browser to sign in. The tools are then available to Copilot Chat in agent mode.

### Cursor

Add it to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "veriacta": {
      "url": "https://veriacta.ai/mcp"
    }
  }
}
```

### Gemini CLI

This repository is also a Gemini CLI extension:

```bash
gemini extensions install https://github.com/AgentX-ai/veriacta-mcp
```

The CLI opens a browser to sign in on the first call.

### Hosts without OAuth

Create an API key at https://veriacta.ai/account and add it in the host's own settings as an `Authorization: Bearer` header
(or `X-API-Key`). Keep the key in an environment variable, not in a file you commit.

### As a plugin or a skill

This repository is also a plugin: the skill plus this server, in one install. In Claude Code:

```
/plugin marketplace add AgentX-ai/veriacta-mcp
/plugin install veriacta@veriacta
```

Or the skill alone, in any host that reads Agent Skills:

```bash
npx skills add AgentX-ai/veriacta-mcp --skill veriacta
```

## Ask it

- What is the VAT exemption limit for small businesses in Poland this year, and which article sets it?
- Which PKD code covers software consulting?
- Is the company with NIP 774-000-14-54 an active VAT payer today, and is this bank account on the white list?
- How do I fill in field K_47 of JPK_V7?
- Compare the general VAT rate in Poland and Spain, with the articles.

## Tools (5)

Every tool only reads: none creates, changes or deletes anything. Every tool takes the country as `jurisdiction`.

- **`jurisdictions`** [read-only]: The countries that are live and what each offers: law, classifications, official guidance, company registers. With a country, its notes on how to ask.
- **`law_lookup`** [read-only]: Provisions of a country's law in force on a date, in full, with their official citation and source.
- **`classification_lookup`** [read-only]: A country's official activity or product classification: what a code includes and excludes, the codes closest to a description, how a code moves between versions.
- **`guidance_lookup`** [read-only]: Passages from an authority's official guides, handbooks and rulings.
- **`company_lookup`** [read-only, reads live registers]: A company in a country's public registers on a given day: tax status, registered bank accounts, register entry and who may represent it.

The server also offers a guide (`veriacta://guide`), a guide per country and prompts for the common workflows. Every result carries
its text and the same body as structured content, under a published output schema.

## Where your data goes

Your assistant sends data to one VeriActa endpoint, and to no other service:

- **`https://veriacta.ai/mcp`**, the MCP server this plugin connects. It receives every tool call and its arguments: the
  country, the question or search text, an act name, a classification code, a company identifier.

To answer a call VeriActa reads its own copy of the official legislation; `company_lookup` asks the public company
registers (the VAT white list, the court register, the EU's VIES) about the identifier you gave. The text of a
question is sent to AI model providers to search and rank the law. VeriActa does not use your questions to train
models and does not sell data.

What is kept, for how long, and your rights:

- Privacy policy: https://veriacta.ai/privacy
- Terms of service: https://veriacta.ai/terms
- Contact: https://veriacta.ai/contact

VeriActa gives legal information from official sources. It is not legal advice.
