---
name: veriacta
description: "Answer questions about the law of European countries and of the EU (today Poland, Spain, Germany, Denmark, the Netherlands, Ireland, Finland, France, Sweden, Italy and EU law) from the official text in force on a date, quoted and cited: tax, VAT, accounting, labour, social insurance, company law. Also classify a business activity or a product or service (Poland: PKD, PKWiU), read official tax and social insurance guides, and check a company in the public registers. Use when an answer rests on a provision, a rate, a threshold, a deadline or a register entry. Works through the VeriActa MCP tools."
when_to_use: "Triggers on requests like \"what is the VAT rate for…\", \"what does art. 113 of the VAT act say\", \"jaki jest limit zwolnienia z VAT\", \"ile wynosi składka ZUS\", \"jaki kod PKD dla…\", \"symbol PKWiU dla…\", \"sprawdź kontrahenta po NIP\", \"is this company an active VAT payer\", \"jak wypełnić JPK_V7\", \"cuál es el tipo general del IVA\", \"what does Article 98 of the VAT Directive allow\", \"was regelt § 12 Abs. 2 UStG\", or \"compare the VAT small-business limit in Poland and Spain\"."
---

Use VeriActa's MCP tools (the `veriacta` MCP server) to answer from the law as published: official legislation
and public registers of European countries, read from their government sources. The tools only read. Every
answered call spends credits from the user's VeriActa account, and each result reports what it cost.

## One-time setup

Check whether the connection already exists by looking for tools prefixed `mcp__veriacta__` (for example
`mcp__veriacta__law_lookup`). If they are there, skip this section.

If they are not, connect the MCP server at `https://veriacta.ai/mcp`. There are two ways in, and the first is better:

**Sign in through the browser (preferred).** VeriActa runs a standard OAuth 2.1 authorization server with dynamic
client registration, so an OAuth-capable MCP host needs nothing but the URL. Add it as a remote MCP server and the
host opens a browser to sign in or create a free account. Nothing to copy, and the connection can be removed on
the account page.

```bash
your-mcp-host mcp add --transport http veriacta https://veriacta.ai/mcp
```

**An API key.** For a host without OAuth: the user creates a key at https://veriacta.ai/account and sets it in
the environment as `VERIACTA_API_KEY`; the host sends it as `Authorization: Bearer <key>` (or in `X-API-Key`).
Never ask the user to paste a key into the conversation, and never write one into a file you commit.
Steps for each assistant: https://veriacta.ai/connect

## Countries

Every tool takes the country as `jurisdiction` (an ISO code). There is one set of tools however many countries
are live; what is particular to a country comes from `jurisdictions`.

- **PL** Poland: law, classifications PKD and PKWiU, official guidance, company registers (nip, krs, regon). Ask in Polish, in the words of the statute; name the act in `act` (for example "ustawa o VAT"); `action` resolve identifies an act by its name or Dz.U. reference, act gives its data, consolidated text, amending acts and outline.
- **ES** Spain: law. Ask in Spanish, in the words of the law; name the act in `act` (LIVA, LIRPF, "Ley 35/2006"); `action` search, then provision with the `path` it returned.
- **EU** European Union: law. Ask in English, in the words of the act; name the act in `act` ("VAT Directive", "2006/112/EC", CELEX "32006L0112", "GDPR"); `action` search, then provision with the `path` it returned; `action` compare with `compare_with` for the national provisions that carry it.
- **DE** Germany: law. Ask in German, in the words of the statute; name the act in `act` by its abbreviation (UStG, EStG, AO, HGB, BGB); `path` as German citations write it ("§ 12 Abs. 2 Nr. 1"); `action` search, then provision with the `path` it returned.
- **DK** Denmark: law. Ask in Danish, in the words of the statute; name the act in `act` by its popular title (momsloven, ligningsloven, selskabsloven) or abbreviation (ML, LL, SEL, ÅRL); `path` as Danish citations write it ("§ 48, stk. 1, nr. 2", "§ 6, stk. 1, 3. pkt.", "bilag 1, nr. 8"); `action` search, then provision with the `path` it returned; read `amendments` for acts not consolidated yet.
- **NL** Netherlands: law. Ask in Dutch, in the words of the statute; name the act in `act` by its citeertitel or abbreviation (Wet OB 1968, AWR, Wet IB 2001, Wet LB 1964, Boek 2 BW); `path` as Dutch citations write it ("artikel 9, tweede lid, onderdeel a", "art. 9 lid 2 sub a", "2:394 lid 1 BW", "tweede volzin"); `action` search, then provision with the `path` it returned; read `amendments` for changes enacted for a later day.
- **IE** Ireland: law. Ask in English, in the words of the statute; name the act in `act` by its short title or abbreviation (VATCA 2010, CA 2014, OWTA 1997, NMWA 2000, DPA 2018); `path` as Irish citations write it ("section 46(1)(a)", "s. 46(1)(ca)", "paragraph 12 of Schedule 3"); `action` search, then provision with the `path` it returned; read `amendments` for changes not in the Revised Act yet.
- **FI** Finland: law. Ask in Finnish, in the words of the statute; name the act in `act` by its name or abbreviation (AVL, TVL, EVL, VML, OYL, KPL, TSL) or number (1501/1993); `path` as Finnish citations write it ("85 §:n 1 momentin 1 kohta", "85 a §", "13 luvun 2 §", "1 momentin toinen virke"); `action` search, then provision with the `path` it returned; read `pending_versions` for wordings still to come and `amendments` for acts not consolidated yet.
- **FR** France: law. Ask in French, in the words of the code; name the text in `act` by its abbreviation (CGI, CIBS, LPF, C. trav., C. com., CSS, CMF, C. civ.) or name; `path` as French citations write it ("article 278", "article L. 1234-5", "article 293 B, I bis, B, 1°", "deuxième alinéa de l'article 1240"); `action` search, then provision with the `path` it returned; read `notes` for the NOTA that says how a change applies, and `pending_versions` for wordings still to come.
- **SE** Sweden: law. Ask in Swedish, in the words of the statute; name the act in `act` by its name or short name (ML, SFL, IL, SAL, BFL, ÅRL, ABL, LAS) or SFS number (2023:200); `path` as Swedish citations write it ("9 kap. 19 §", "9:19", "9 kap. 19 § andra stycket", "9 kap. 3 § första stycket 1", "första meningen"); `action` search, then provision with the `path` it returned; read `pending_versions` for wordings still to come, `notes` for a § renumbered, and `amendments` for acts the text does not carry yet.
- **IT** Italy: law. Ask in Italian, in the words of the act; name the act in `act` by its number ("DPR 633/1972", "D.Lgs. 81/2015", "L. 212/2000") or name ("TUIR", "codice civile", "testo unico IVA", "Statuto del contribuente"); `path` as Italian citations write it ("art. 10, comma 1, n. 27-quinquies)", "art. 164, comma 1, lettera b)", "art. 16, primo comma", "art. 2423-bis c.c."); `action` search, then provision with the `path` it returned; read `pending_versions` for wordings still to come, the PROVISION_REPEALED warning for an article's successor from 2027-01-01, and `notes` for the AGGIORNAMENTO notes.

The list grows without new tools. Call `jurisdictions` (free) to see what is live now, and `jurisdictions` with a
country code the first time you work with that country: it returns that country's rules for asking, in its language.
A capability a country does not have answers `NOT_AVAILABLE` and names the countries that do.

## Rules for every answer

- Work out the country first. Call `law_lookup` with its code before every answer that rests on a provision, and
  again for each follow-up question. Do not reuse an earlier result for a new point.
- Write the query in the country's language and in the words of the statute, and name the act when you know it.
- Quote the provision and give its citation in the country's form and the date it was in force.
- Take every rate, threshold, amount and deadline from the returned text, never from memory.
- Read `warnings` first: they say what was not found, what was matched to a different act, and what a newer
  amendment changes. If the result carries `status`, `freshness_warning` or `act_match`, do what it says.
- If the tools return nothing that answers the question, say that the legal basis was not confirmed. Do not fill
  the gap from memory.
- Guidance explains the law and is not the law: confirm the legal basis with `law_lookup`.
- This is legal information, not legal advice. Say so when the user is about to act on it.

## Tools

**`jurisdictions`** (VeriActa: countries and what each offers)

- Returns: The countries that are live and what each offers: law, classifications, official guidance, company registers. With a country, its notes on how to ask.
- Use for: The first time you work with a country, when you are not sure a country is covered, or when a tool answers NOT_AVAILABLE. It is free.
- Then: Follow the country's notes: the language and terms to search in, how its acts are named and cited.
- Arguments:
  - `jurisdiction`: A country code, to get that country's notes on how to ask each tool.

**`law_lookup`** (VeriActa: law in force, by country)

- Returns: Provisions of a country's law in force on a date, in full, with their official citation and source.
- Use for: Any question that rests on a provision: a rate, a threshold, a deadline, a duty, a definition. Call it again for every follow-up question, with the act named when you know it.
- Then: Quote the provision and cite it. If the answer carries `status`, `freshness_warning`, `act_match` or warnings, do what they say before answering.
- Arguments:
  - `jurisdiction` (required): The country whose law or register is asked about, by its ISO code (EU: the law of the European Union itself).
  - `query`: What to find, in the country's language and in the words of the statute.
  - `act`: The act, when you can tell which one: its name, common abbreviation, official number or id ("ustawa o VAT", "LIVA", "Ley 35/2006", "VAT Directive", "UStG").
  - `path`: One provision to read: the path a search returned or a citation ("art. 113 ust. 1", "artículo 91.Uno", "Article 98(2)", "§ 12 Abs. 2 Nr. 1").
  - `as_of`: The day the wording must be in force on, YYYY-MM-DD. Today when left out.
  - `action`: search (default): provisions that answer the query. provision: the text at `path`. act: an act's data and outline. resolve: identify an act by name. compare (jurisdiction EU): the EU provision at `act` + `path` next…
  - `limit`: How many results a search returns, where the country supports it.
  - `url`: Only an exact link to the official text that the user gave.
  - `interpret`: Default false: return the provisions. true only where the country supports applying the law to the facts in the query.
  - `compare_with`: action compare only: the countries to put next to the EU provision, by ISO code (["DE", "ES"]).
  - `national_queries`: action compare only: for each country of compare_with, words of its statute in its language ({"DE": "ermäßigter Steuersatz"}); they sharpen the match.

**`classification_lookup`** (VeriActa: activity and product classifications)

- Returns: A country's official activity or product classification: what a code includes and excludes, the codes closest to a description, how a code moves between versions.
- Use for: Choosing the code for a business activity, a product or a service, or checking what a code covers.
- Then: Read what each candidate includes and excludes before naming a code. For a tax rule that depends on the code, call law_lookup.
- Arguments:
  - `jurisdiction` (required): The country whose law or register is asked about, by its ISO code (EU: the law of the European Union itself).
  - `system` (required): The classification: PKD (Polish business activities) or PKWiU (Polish goods and services). `jurisdictions` lists each country's systems.
  - `query` (required): Codes of the system, a concrete description of the activity, product or service to classify (what exactly, for whom), or a question about the classification's rules.
  - `mode`: auto (default): codes in the query give their details, anything else is a search. search: candidates for a description. rules: the classification's rules.
  - `version`: The version of the codes in the query, when you know it (PKD: 2025 or 2007; PKWiU: 2015 or 2025).
  - `limit`: Candidates a search returns (1 to 10, default 5).

**`guidance_lookup`** (VeriActa: official guides and rulings)

- Returns: Passages from an authority's official guides, handbooks and rulings.
- Use for: How an authority says to fill in a form or a field, or how it applies a rule. Search first, then read the document and page.
- Then: Cite the document and page. Guidance explains the law and is not the law: confirm the legal basis with law_lookup.
- Arguments:
  - `jurisdiction` (required): The country whose law or register is asked about, by its ISO code (EU: the law of the European Union itself).
  - `query`: The issue or the name of a form field, in the words of the document.
  - `issuer`: Whose documents to search, where the country has several (Poland: ZUS, MF, KE).
  - `document`: A document id from an earlier result, to read it.
  - `page`: The page of `document` to read.
  - `source`: A named collection, where the country has more than one (Poland: "eureka" for individual tax rulings; default the guides).
  - `limit`: Passages a search returns (1 to 10, default 5).

**`company_lookup`** (VeriActa: company registers)

- Returns: A company in a country's public registers on a given day: tax status, register entry and who may represent it, and whether a bank account is registered for it.
- Use for: Checking a counterparty before a payment or a contract, by its tax or register number.
- Then: Say which register and day each fact comes from. A service that did not answer is not a negative result.
- Arguments:
  - `jurisdiction` (required): The country whose law or register is asked about, by its ISO code (EU: the law of the European Union itself).
  - `id` (required): The company's number: a tax number or a register number (Poland: NIP, KRS or REGON; dashes, spaces and a country prefix are fine).
  - `id_type`: Which number `id` is, when you know.
  - `date`: The day to check the status and bank accounts on, YYYY-MM-DD. Today when left out.
  - `account`: A bank account to check: the result's `account_check` says whether it is registered for the company on `date` (Poland: the VAT white list; spaces and PL are fine).
  - `list_accounts`: true lists every bank account registered for the company.

## Common workflows

**A tax or legal question.** `law_lookup` with the country's code, the question in its language and, when you know
it, the act (`act`). Read the warnings, quote the provisions that answer, cite them.

**The rate for a product or service.** `classification_lookup` with the product classification (Poland: `PKWiU`)
and the description. Check what the candidates include and exclude and which version applies. Then `law_lookup`
for the provision or annex that names that code.

**The activity code of a business.** `classification_lookup` with the activity classification (Poland: `PKD`) and
what the business does. Compare what the candidates include and exclude.

**Checking a counterparty.** `company_lookup` with the company's number and the day of the payment or contract.
Report the tax status, whether the bank account is registered, and who may sign. For what follows from a missing
account or status, `law_lookup`.

**How to fill in a form or a field.** `guidance_lookup` with the form and the field (for example `JPK_V7 K_47`),
read the page it points to, then confirm the legal basis with `law_lookup`.

**Reading one provision.** `law_lookup` with `path` (the path a search returned, or a citation) and the act.

**Two countries compared.** `law_lookup` once per country, each in its own language and for the same date. Set
the quoted provisions side by side with both citations.

## Notes that save a call

- Name the act when you know it (`act: "ustawa o VAT"`, `act: "LIVA"`): a scoped question is answered better.
- A date in the question matters: pass it as `as_of`, or the answer is the law as it stands today.
- `company_lookup`: a service that did not answer is not a negative result.
- `classification_lookup` candidates are the closest matches even when none fits. A high score does not confirm
  a classification.
- Wrong arguments come back as `INVALID_ARGUMENTS` with every problem and a hint. Fix them all and call once more.
- The server also offers prompts for these workflows (`answer_with_citations`, `classify_activity`, `classify_product`, `check_counterparty`, `compare_countries`), the
  resource `veriacta://guide` and one per country (`veriacta://guide/PL`).
