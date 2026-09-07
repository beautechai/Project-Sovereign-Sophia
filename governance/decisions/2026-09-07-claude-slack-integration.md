# PSS Decision Brief — Claude ↔ Slack Integration

**Prepared by:** Claude (Builder / Orchestrator)
**Decision owner (Governor):** Nico / ChatGPT — PSS Architect
**Final approver (root):** Beau Johnson — BEAU TECH
**Date:** 2026-09-07
**Scope:** How Claude operates as an agent inside Slack, focused on channel `C0BQYL1QJHE`
**Status:** PROPOSAL — awaiting Architect ruling, then Beau approval. No configuration, plan, or canon changed by this document.

---

## Summary

Claude is currently reachable in Slack through the **legacy "Claude in Slack" bot**, which has **no PSS memory access and no persistent collaboration/continuity**. Beau has flagged this as a dealbreaker for using Slack as a PSS surface. This brief lays out the current state, the outstanding issues, and the build decisions the Architect must rule on before Claude-in-Slack can be trusted for PSS work — especially in channel `C0BQYL1QJHE`. Each decision carries a Builder recommendation; the ruling belongs to the Architect and final approval to Beau.

## Current State (verified from this Slack thread, 2026-09-07)

- The surface answering in Slack is the **legacy Claude in Slack bot** — text-only, no PSS memory, no cross-message continuity, no tool/codebase connection.
- **Claude Tag** (the shared, memory-bearing `@Claude` teammate that replaced the legacy bot on other tiers) is **Team/Enterprise only** — it is **not** included in Beau's individual **Max** plan.
- Claude Tag, where enabled, runs in an **Anthropic-hosted ephemeral sandbox**, not on local/BEAU TECH infrastructure — a direct tension with PSS's local-first / sovereignty bias.
- The current agent environment **hard-blocks all Anthropic domains at the egress proxy** (claude.com, anthropic.com, Help Center), so Claude cannot self-verify official product/pricing docs from inside Slack.
- Voice playback is not supported (text-only surface).
- Purpose of channel `C0BQYL1QJHE` is **not yet documented** — assumption below pending Architect confirmation.

## Outstanding Issues

1. **No memory / continuity** on the legacy bot → Slack cannot act as a PSS surface today.
2. **Plan gap** → the memory-bearing option (Claude Tag) requires a Team/Enterprise plan Beau does not currently hold.
3. **Sovereignty conflict** → Claude Tag's Anthropic-hosted sandbox vs PSS local-first canon.
4. **Egress policy** → Anthropic domains blocked; unclear if intentional.
5. **Undefined channel role** → no documented purpose, tool scope, or spend cap for `C0BQYL1QJHE`.
6. **No governance wiring** → no defined identity, audit logging, or approval gates for any write-scoped tools (ConnectWise, QuickBooks Online, Microsoft Graph) if Claude-in-Slack is connected to them.

## Decisions Required

> Format: Options → Pros / Cons / Automation Fit / Reversibility → Builder recommendation. Architect rules; Beau approves.

### D1 — Canonical Claude surface for PSS work
- **A. Keep Slack on the legacy bot** for quick Q&A only; do all memory/collaboration work in the Claude app (Max) and agentic work in Claude Code. *Reversible; $0; no Slack memory.*
- **B. Adopt Claude Team plan to unlock Claude Tag in Slack.** *Adds memory + multiplayer + tool connection in Slack; adds a plan + consumption billing; sovereignty tension.*
- **C. Build a custom BEAU TECH Slack app** backed by the PSS local-first memory/knowledge graph (Slack API → PSS ingestion → agentic workflows → draft → approval). *Full sovereignty; highest build cost.*
- **Builder recommendation:** **A now, C as the PSS-aligned target.** Reserve B only if multiplayer/client-facing Slack delegation becomes a near-term revenue driver. Rationale: A costs nothing and Beau already has app-based memory on Max; C matches local-first canon; B trades sovereignty for speed.

### D2 — Plan / procurement (only if D1 = B)
- Adopt Claude **Team** (consumption-based token billing, org-level spend caps). Confirm current pricing and any launch credits before commit — treat all figures as unverified until checked against official pages (currently egress-blocked).
- **Builder recommendation:** Do not procure until D1 rules for B and Beau signs off on a monthly token cap.

### D3 — Role and scope of channel `C0BQYL1QJHE`
- Define: (a) channel purpose, (b) which tools Claude may use there (default: read-only), (c) whether any write scopes are permitted and behind what approval gate, (d) monthly spend cap if Tag/custom app is used.
- **Assumption pending confirmation:** treated as an internal PSS build/ops channel, not client-facing.
- **Builder recommendation:** Start read-only, no write scopes, explicit spend cap; escalate scope only per D6 gates.

### D4 — Memory / continuity architecture
- **A. Anthropic-hosted (Claude Tag native).** *Fast; off-prem; conflicts with local-first.*
- **B. Local-first PSS memory** (Obsidian/SQLite/knowledge graph) exposed to the Slack surface via a controlled connector. *Sovereign; provenance-preserving; more build.*
- **Builder recommendation:** **B.** Pipeline: ingestion → normalization → structured memory/knowledge graph → agentic workflows → draft outputs → human approval. Keep provenance on every write.

### D5 — Egress / network policy
- Decide whether the Anthropic-domain egress block is intentional. If Claude-in-Slack must self-verify product/pricing/docs, allow-list the required Anthropic domains through the proxy.
- **Builder recommendation:** Confirm intent; if verification is needed, allow-list read-only; otherwise document the block so future agents don't retry blindly.

### D6 — Governance wiring
- Define: the identity Claude acts under in Slack, audit logging/query logs, and explicit approval gates for any write scope, money movement, external comms, or config change (per PSS: agents propose, Beau approves; no silent execution).
- **Builder recommendation:** Read-only by default everywhere; every write scope behind an explicit Beau approval gate; all actions logged with provenance.

## Proposed Acceptance Criteria (for the Architect to confirm/amend)

1. A single canonical Slack surface is chosen (D1) and documented in PSS canon.
2. Channel `C0BQYL1QJHE` has a written purpose, tool scope, and spend cap (D3).
3. Memory architecture decision (D4) is recorded with a provenance/sovereignty rationale.
4. Egress policy intent (D5) is documented.
5. Governance gates (D6) are defined and no write scope is live without a Beau approval gate.

## Builder Next Steps (only after Architect ruling + Beau approval)

- Implement the chosen surface (config for A/B, or build for C).
- Wire the D3 channel scope and spend caps.
- Stand up the D4 memory connector if B is chosen.
- File the ruling back into PSS canon and close this brief.

## Risks / Rollback

- **Risk:** Choosing Claude Tag (B/D4-A) moves PSS context into an Anthropic-hosted sandbox — off-prem, against local-first canon. **Rollback:** revert to legacy bot / Claude app; no PSS memory is exposed until D4 is settled.
- **Risk:** Connecting write scopes without D6 gates enables silent execution against client systems. **Rollback:** keep all scopes read-only until gates exist.
- **Risk:** Pricing/plan figures are currently unverifiable (egress block) — do not commit spend on unconfirmed numbers.

---

*Builder proposal. Ruling: PSS Architect (Nico/ChatGPT). Approval: Beau Johnson (root).*
