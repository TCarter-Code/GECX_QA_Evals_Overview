# GECX QA Evals

**Automated quality assurance for Google Cloud Conversational Agents — built so your team catches a broken
agent before your customers do.**

---

## The problem

Conversational agents drift. An instruction gets tweaked, a tool gets updated, a routing rule changes — and
somewhere in that change, a behavior your customers rely on quietly breaks. Manual QA catches some of it,
eventually, after someone notices. By then it's already shipped.

## What this gives you

A standing QA layer that runs real, live conversations against your deployed agent — the same way a customer
would talk to it — and tells you exactly what changed, what broke, and what still works. Every time.

- **Regression testing that actually talks to your agent.** Not canned transcripts, not mocked responses —
  live simulated conversations against your real deployed agent, scored on whether it *behaved* correctly.
- **Instruction changes never slip through unnoticed.** The moment an agent's instructions change, you know —
  before you've run a single test.
- **A findings trail your team can trust.** Every run leaves a clear record: what passed, what regressed,
  what's still open — not just a pass/fail number that resets every time.
  → [See a sample findings report](examples/sample-findings.md)
- **Real performance data, not estimates.** Token usage, latency, and conversation replay links straight from
  the platform.
- **One report, ready to share.** A clean, self-contained page with the results, built to forward to a
  teammate or a stakeholder without any setup on their end.
  → [See a sample report](examples/sample-qa-report.html) (download and open in a browser — sample data only).
- **Fix-and-retry without the noise.** Re-run only the sims you fixed and the report still shows the whole suite: two runs,
  the first results and the result after the fixes, with the failures at the top and a clear tag on each one that got fixed.
  Your retries don't pile up as extra reports.
- **Scales with your org.** One agent or an entire app, one person or a distributed team syncing results
  through your own source control — no extra infrastructure required.
- **Mock data, built in.** Sims can run against fake tool responses, so no real customer accounts are needed. Every report
  shows which sims ran on mock data.
- **Safety and boundary checks in their own suite.** Ready-made sims for crisis responses, tone, scope, privacy and prompt
  injection. Each rule is tested once, from inside a real topic, so you know it still holds after a hand-off.
- **Hidden problems surface.** A tool error the agent quietly recovers from is listed too, so a sim that passes after a retry
  no longer hides a fault.
- **Self-contained on SCRAPI alone.** No AI subscription required — the whole pipeline runs end to end on
  SCRAPI's own engine. Have Claude access? That's a bonus: layer in AI assistance for writing new test
  cases, spotting coverage gaps, and diagnosing failures, whenever you want it.

## How it works

Two commands, every time:

```
python pull-agent.py              # get the latest deployed agent
python eval-agent.py <Agent>      # test it and write the results
```

The second command does the rest: it checks whether the agent's instructions changed since the saved baseline, keeps the mock
data up to date, runs every simulated conversation three times (a conversation passes when most of its runs pass), and writes the
findings log and a shareable report. The first run for an agent saves its baseline automatically. After you have reviewed a
change and accepted it, one more command saves the new baseline. Want AI help diagnosing failures? Turn it on in one setting;
everything else works without it.

## What a run leaves behind

Every run adds a dated entry to your findings log, newest on top. This is a trimmed excerpt of one
(sample data) — [the full log is here](examples/sample-findings.md):

> **✅ Ready for Evaluation run** · **✅ Valid test cases: 6 (5 EN / 1 FR)** · **✅ Baseline created**
>
> **Status:** ⚠️ Ran 2026-10-06 14:30 PDT (17:30 EDT) — 4/6 passed; 2 fail(s): `billing_switch_to_autopay`,
> `billing_change_due_date`.
>
> **Drift:** ⚠️ instructions changed since the baseline — a failure below may be a stale test rather than an
> agent bug.
>
> **Mock data:** ✅ up to date. **Tool errors seen in this run** (including sims that still passed): `update_autopay`, `sign_in_widget`.
>
> #### Changed since last run
> - **Pass rate:** 4/6 → 4/6 (no change)
> - **Fixed (now passing):** billing_dispute_charge
> - **Regressed (newly failing):** billing_change_due_date
> - **Still failing:** billing_switch_to_autopay
>
> #### What failed
> - **billing_switch_to_autopay** · 3 turns · conversation ID included to replay it
>   - *Expected:* the bot reads back the billing cycle date before enrolling autopay.
>   - *What happened:* the bot enrolled autopay without reading back the cycle date confirmation.

Three things you don't get from a plain pass/fail number: what *changed* since last time, a heads-up when the agent's own
instructions moved underneath your tests, and the faults that a passing conversation was quietly hiding.

## Built for GECX

Purpose-built for agents on Google Cloud's Conversational Agents platform. Drop it into a project, point it
at your deployed agent, and it's ready to run.

Fully integrated with **cxas-scrapi**, Google Cloud's own open-source evaluation framework — every
conversation is run and scored through it, and every performance number is pulled straight from it. Same
engine, same data, as the platform team's own tooling.

---

## Get in touch

Interested in using this for your own GECX agents, or want to see it in action? Reach out to Teresa Carter on
[LinkedIn](https://www.linkedin.com/in/teresa-carter-a5b61a220).
