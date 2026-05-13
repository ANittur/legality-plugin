# legality

Jurisdiction-specific contract review and licensed-advocate consultation as a Claude Code plugin. Initial jurisdiction: India.

## What it does

- **AI-first contract review** under Indian law. The plugin's MCP server returns a curated set of relevant Indian-law statutes (Indian Contract Act, FEMA, DPDP Act, Companies Act 2013, Advocates Act, etc.) and structured analysis instructions. Claude Code performs the review itself, grounded in the served statutes. Citations are statute-only and accurate by construction; no LLM-hallucinated case-law.
- **On-demand consultation** with BCI-verified Indian advocates. After review, optionally escalate to a 30-minute paid call with a licensed practitioner. Booking flows through a self-hosted Cal.com instance; payment is invoiced out of band at V0.

## Install

```bash
/plugin marketplace add https://legality-seven.vercel.app/marketplace.json
/plugin install legality@legality
```

Then in any Claude Code session:

```
/legality:india
```

Followed by the contract text or a question about Indian commercial-contract law.

## How it works

The plugin ships a single skill (`/legality:india`) plus an HTTP MCP server registration. The MCP server is hosted at `https://legality-seven.vercel.app/api/mcp` and authenticates via OAuth 2.1 (Clerk). On first use, Claude Code walks you through the OAuth flow once; subsequent tool calls reuse the token.

Tools exposed by the MCP server:

- `review_contract` — returns relevant Indian-law statutes plus analysis instructions for the client to apply
- `get_attorney_info` — lists BCI-verified Indian advocates available for consultation
- `schedule_consultation` — books a 30-minute slot, returns a pre-filled booking URL
- `submit_recommendation` — post-consultation customer attestation (optional)

## Pricing

- AI contract review: free.
- Attorney consultation: ₹4,000 per 30-minute call, invoiced out of band at V0 (manual / UPI). Razorpay integration is on the roadmap.

## Not a law firm

legality is not a law firm. Tool outputs are general legal information grounded in a curated Indian-law statute set, not legal advice. For matters with HIGH-severity risk findings, regulatory exposure (FEMA / RBI / SEBI / DPDP), or any cross-border element, the plugin recommends escalating to a licensed advocate, which is the appropriate professional channel for legal advice.

## Privacy

Contract text submitted to `review_contract` is hashed (SHA-256) for dedup and audit; the full text is not stored. Customer-provided name and email used for consultation booking are passed through to Cal.com for the booking record. See the privacy policy at `https://legality-seven.vercel.app/privacy` for the full data-handling description.

## Author

Aditya Nittur Anantha (`an@flowblinq.com`)
