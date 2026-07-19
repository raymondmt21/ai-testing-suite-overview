# ai-testing-suite-overview
# Shopify AI Testing Suite

**Chaos Unleashed** | Proprietary tooling (overview only; source code is private)

A suite of tools for stress-testing AI assistants and third-party integrations that
operate on live e-commerce data, and for auditing what those integrations can actually
access. Built during a consulting engagement to answer one question rigorously: *when an
AI assistant answers questions about a real store, can its answers be trusted?*

## The problem

AI assistants that sit on top of business data are only useful if their answers are
correct. In practice they fail in ways a normal demo never surfaces: they calculate
values that are subtly wrong, they invent data that does not exist, and they answer
confidently in situations where the honest response is "there is no data for that." A
buyer evaluating one of these tools needs a way to measure that, not just take the vendor's
word for it.

## What this suite does

- **Grades AI answers against ground truth.** It pulls the store's real data directly
  from the source of record, then builds a graded question set the assistant is tested
  against, so every answer can be scored against a verifiable fact rather than a vibe.
- **Deliberately baits the failure modes that matter.** The question set includes
  "trap" questions whose only correct answer is *no data exists* - the single most useful
  test for catching an AI that fabricates a confident answer instead of admitting a gap.
- **Tests precision and consistency.** It checks exact values and asks the same questions
  repeatedly across sessions to catch non-deterministic drift.
- **Generates realistic test data on demand.** A companion generator seeds a store with
  fully tagged, teardown-able synthetic data (customers, orders, fulfillments, refunds,
  edge cases) so testing happens against a known dataset without touching real records.
- **Audits integration access as a security exercise.** A diagnostic tool inventories
  what a connected third-party app can actually reach, flags excessive write permissions
  it does not need, and measures how quickly it truly syncs with live data.

## Representative findings

Applied to a live AI-driven commerce assistant, this suite surfaced failure modes that a
happy-path demo would never reveal: calculated values wrong by consistent orders of
magnitude, fabricated entries once a capacity limit was exceeded, and confident specific
answers to questions that had no underlying data at all. The pattern that generalizes:
**the danger is not that these systems cannot answer - it is that they answer confidently
when they should decline.**

## Why it matters

The interesting engineering here is not any single script; it is the testing philosophy -
measuring trustworthiness by deliberately probing for the failure modes that erode it.
That approach transfers to any AI system whose answers people are expected to rely on.

## Built as a framework, not a one-off

This suite was first built and proven against a Shopify store, but the design separates a
**domain-independent testing engine** from **swappable, domain-specific adapters**. The
engine owns the parts that are universal: generating verifiable, trap, and precision
questions, scoring responses against ground truth, and detecting drift across sessions. An
adapter owns only what is specific to a given target: where the ground truth lives, how to
retrieve it, and what that domain's verifiable facts look like.

The Shopify integration is simply the first adapter. The same engine is designed to be
pointed at AI systems in other sectors and applications by adding new adapters, without
changing the core. The failure modes it hunts - confident wrong answers, fabrication past a
limit, answering when the honest response is "I don't know" - are universal, so the testing
methodology is too.

---
*Chaos Unleashed. This repository is a public overview. The implementation is proprietary
and maintained privately.*
