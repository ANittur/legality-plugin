# legality

India-jurisdiction contract review, drafting, and BCI-verified advocate consultation, available on every Claude surface: Claude Code, Claude.ai web, and the Claude Android and iOS apps.

## Install in 30 seconds

### Claude Code (desktop CLI)

```bash
/plugin marketplace add https://legality-seven.vercel.app/marketplace.json
/plugin install legality@legality
```

Then in any session: `/legality:india` followed by your contract text or question.

### Claude.ai web, Android, iOS (Custom Connector)

1. Open [claude.ai](https://claude.ai) in any browser and sign in.
2. Settings → Connectors → Add Custom Connector.
3. Paste this URL: `https://legality-seven.vercel.app/api/mcp`
4. Complete the one-time OAuth flow.

The connector syncs automatically to the Claude Android and iOS apps. Ask in natural language: *"Review this contract under Indian law"*, and the model calls the legality tools.

## What it does

- **AI-first contract review** under Indian law. legality returns a curated set of relevant Indian-law statutes (Indian Contract Act 1872, Companies Act 2013, FEMA 1999, Income Tax Act 1961, DPDP Act 2023, Transfer of Property Act 1882, Indian Easements Act 1882, Copyright Act 1957, Trade Marks Act 1999, Arbitration Act 1996, CPC, CGST Act 2017, Advocates Act 1961, Partnership Act 1932, LLP Act 2008, Constitution Part IXA — **16 acts**, ~140 sections curated for contract-review use cases) along with structured analysis instructions. Claude performs the review itself, grounded in the served statutes. Citations are statute-only and accurate by construction; no LLM-hallucinated case-law.

- **Contract drafting** from authoritative Indian templates. Full templates ship for nda, vendor_agreement, founder_agreement, shareholders_agreement, lease_agreement, premises_license, and a general `other` template; stub templates for employment, ip_assignment, saas_terms. Each template lands with statute-basis citations woven through.

- **On-demand consultation** with BCI-verified Indian advocates. After a review, optionally escalate to a 30-minute paid call with a licensed practitioner. Booking flows through a self-hosted Cal.com instance with the matter summary attached; payment is invoiced out of band at V0 (₹4,000 / ~$48).

## Matter types covered

`nda` · `vendor_agreement` · `employment` · `ip_assignment` · `founder_agreement` · `shareholders_agreement` · `saas_terms` · `lease_agreement` · `premises_license` · `other`

## Tools exposed

| Tool | Purpose |
|---|---|
| `review_contract` | Returns relevant Indian-law acts and sections for the matter, plus analysis instructions. The model performs the analysis itself. |
| `draft_contract` | Returns an authoritative Indian-law-grounded template structure plus relevant statutes, plus drafting instructions. The model composes the contract. |
| `get_section_text` | Returns the full statutory text of a specific section, identified by (act_id, section_id) values from a prior review or draft response. |
| `get_attorney_info` | Lists BCI-verified Indian advocates available for consultation, filterable by jurisdiction and matter type. |
| `schedule_consultation` | Books a 30-minute slot with a chosen advocate; returns a pre-filled booking URL with the matter context attached. |
| `submit_recommendation` | Records a post-consultation customer attestation (optional). |

## How it works

The HTTP MCP server is hosted at `https://legality-seven.vercel.app/api/mcp` and authenticates via OAuth 2.1 (Clerk). On first use, your Claude client walks you through the OAuth flow once; subsequent tool calls reuse the token. The connection originates from Anthropic's cloud infrastructure, not from your local device, which is why the same connector works on web, mobile, and desktop without separate setup.

For Claude Code, the plugin additionally ships a `/legality:india` slash-command skill that primes the model with the BCI Rule 36 framing and the two-mode (AI / human-advocate) workflow before the first tool call. The MCP server's tool descriptions carry the same framing, so the experience on web and mobile (where slash-commands don't exist) is functionally equivalent.

## Pricing

- AI contract review and drafting: **free**.
- Attorney consultation: **₹4,000** per 30-minute call (about $48), invoiced out of band at V0 (manual / UPI). Razorpay rail is on the roadmap.

## Not a law firm

legality is not a law firm. Tool outputs are general legal information grounded in a curated Indian-law statute corpus — not legal advice. For matters with HIGH-severity risk findings, regulatory exposure (FEMA / RBI / SEBI / DPDP), cross-border element, contract value over INR 10 lakh, or non-compete enforceability questions, the plugin recommends escalating to a BCI-verified advocate, which is the appropriate channel for binding legal advice.

## Privacy

Contract text submitted to `review_contract` is hashed (SHA-256) for dedup and audit; the full text is not stored. Customer-provided name and email used for consultation booking are passed through to Cal.com for the booking record. Full data-handling policy: [legality-seven.vercel.app/privacy](https://legality-seven.vercel.app/privacy).

## Author

Aditya Nittur Anantha · `an@flowblinq.com`
