# HalluciTrap 🪤

> **Watch AI agents silently fabricate tool arguments — and see exactly what breaks next.**

[![Live Demo](https://img.shields.io/badge/Live-Demo-f0883e?style=for-the-badge)](https://rlasaf12.github.io/hallucitrap/)
[![Research](https://img.shields.io/badge/ICLR_2026-Reasoning_Trap-blue?style=for-the-badge)](https://iclr.cc)
[![No Framework](https://img.shields.io/badge/Zero_Dependencies-Vanilla_JS-green?style=for-the-badge)]()

---

## What Is This?

HalluciTrap is an interactive simulator that demonstrates **tool argument hallucination** — one of the most dangerous and invisible failure modes in production AI agents.

The ICLR 2026 "Reasoning Trap" paper found that **stronger reasoning models hallucinate tool arguments more often** (statistically significant across 14 model families). 78% of these hallucinations return `HTTP 200 OK` — making them completely invisible to standard error monitoring.

---

## What You Can Test

Three real-world tool schemas, each with 3 pre-built attack scenarios:

| Schema | Risk Level | What Goes Wrong |
|--------|-----------|-----------------|
| `get_customer_order(customer_id, order_id)` | 🟠 HIGH (75%) | Wrong customer data returned silently, refund to wrong account, session bleed → PII |
| `send_notification(user_id, channel, subject, body)` | 🔴 CRITICAL (92%) | Wrong recipient, context PII leaks into message body, security policy breach |
| `query_database(table, filter, limit, order_by)` | 🔴 CRITICAL (88%) | Nonexistent columns silently ignored, stale view used, full table scans without limit |

---

## The Core Pattern

```
Agent fabricates plausible-looking argument
  → Tool executes successfully (HTTP 200)
  → Returns data for wrong entity
  → Agent proceeds with full confidence
  → Cascade: 1–4 downstream steps corrupted
  → Detection: 0 errors raised
```

The agent never suspects. The monitoring never fires. The user gets a confident wrong answer.

---

## Why This Matters

- **96%** of enterprises are running AI agents in production today
- **3.2 steps** — average cascade depth before human detection (ICLR 2026)
- **Named production failure:** ML engineer spent 2+ hours debugging a silent `200 OK` that returned the wrong customer's data

---

## How to Use It

1. **Open the [live demo](https://rlasaf12.github.io/hallucitrap/)**
2. Pick a schema (Customer Order, Email Sender, DB Query)
3. Select an attack scenario
4. Press **▶ Run Simulation** — watch the hallucination unfold step by step
5. Toggle **Defense Mode** to see the exact Python/JS guard that stops it

---

## What Each Panel Shows

**Schema Lab (left):** Tool definition, per-parameter hallucination risk scores, and risk meter.

**Scenario Runner (center):** Animated conversation log. Hallucinated arguments are highlighted in red with ⚠ markers. Watch the agent reason, call the tool, get a 200 back, and proceed confidently with corrupted state.

**Cascade Impact (right):** Visual blast radius — how many downstream steps are corrupted. Includes a Defense Mode toggle that reveals the exact validation code that would catch the hallucination at Step 1.

---

## Defense Patterns Demonstrated

Each scenario includes production-ready defensive code:
- Session-based customer ID validation (never infer from context)
- Recipient confirmation before any outbound action
- Schema column validation before query execution
- Table registry with freshness tagging
- Safe limit enforcement with count-before-fetch

---

## Built By

Part of the **Ben Nightly Builder Loop** — autonomous prototypes built against real developer pain signals.

Research source: ICLR 2026 "The Reasoning Trap" · Noveum.ai 2026 Production Failure Report

---

*Zero dependencies. Single HTML file. Dark theme. No backend.*
