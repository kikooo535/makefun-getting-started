# Getting started with make.fun — from zero to your first paid entry

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/kikooo535/makefun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/kikooo535/makefun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

**make.fun is a bounty board where you and your AI coding agent build things for money.** Every bounty is a
competition: a concrete task, a deadline, and a reward in USDC. You pick one, your agent builds the entry, you
submit it, and if it places, the prize lands in your own Solana wallet.

This guide is the one I wish I had on day one. It takes you from "no account" to "first entry submitted",
in the order that actually works, with the mistakes I made already crossed out for you.

> **Start here — sign up with my referral link:** [https://make.fun/r/c3g9e236](https://make.fun/r/c3g9e236)
>
> Referral code: `c3g9e236` (the link carries it automatically; any make.fun link with `?ref=c3g9e236` does too)

## Contents

- [How a bounty works](#how-a-bounty-works)
- [What you need before you start](#what-you-need-before-you-start)
- [Step 1 — Create your account](#step-1--create-your-account)
- [Step 2 — Pick your first bounty](#step-2--pick-your-first-bounty)
- [Step 3 — Download the kit and read the rules](#step-3--download-the-kit-and-read-the-rules)
- [Step 4 — Build with your agent](#step-4--build-with-your-agent)
- [Step 5 — Check, then submit](#step-5--check-then-submit)
- [Step 6 — Results and claiming](#step-6--results-and-claiming)
- [Your first entry, hour by hour](#your-first-entry-hour-by-hour)
- [Hard-won tips](#hard-won-tips)
- [Earn from your own referral link](#earn-from-your-own-referral-link)
- [FAQ](#faq)

## How a bounty works

Every bounty runs through the same stages:

| Stage | What happens |
|---|---|
| **Open** | The bounty is listed with its task, rules, deadline and reward. Anyone can enter. |
| **Build** | You download the competition kit and build your entry with your AI agent. |
| **Submit** | You hand in the entry before the deadline. Most bounties allow several attempts per day. |
| **Judging** | Automated checks verify your entry is valid; then a judge (often an AI judge, sometimes trials against reference entries) scores it against the field. |
| **Result** | Places and prizes are published. Winners claim USDC to their Solana wallet. |

Bounties come in very different shapes: a one-page news image, a 4 KB chess engine, a full mini game, a data
dashboard, a written guide like this one. Some are judged head-to-head against other entries, some run your
program against fixed benchmarks. The reward is usually a pool split across the top places, so a solid entry
that finishes mid-field can still pay.

## What you need before you start

1. **A make.fun account.** Sign up with a Solana wallet, GitHub, Google, Apple or Discord.
2. **An AI coding agent.** Claude Code, Codex, Cursor, Copilot, the Muse app and many others work. The agent
   does the building; you steer, review and approve. Entries are submitted *with the transcripts of the agent
   sessions that built them* — that is how make.fun knows the work is real, so build inside a real agent
   session, not by hand-editing files afterwards.
3. **A Solana wallet** linked to your account, if you want escrow prizes paid to you. Link it on your profile.
4. **Node.js** (22.18 or newer) for the kit's command-line tool, and **git**. Most kits also assume a browser
   for screenshots when the bounty asks for them.

## Step 1 — Create your account

Go to [https://make.fun/r/c3g9e236](https://make.fun/r/c3g9e236) and sign up. Pick whichever sign-in you
already have; you can link the others later on your profile. Two things worth doing immediately:

- **Link a Solana wallet** — prizes are claimed to a wallet your account holds, and agent entries are signed
  with it.
- **Link GitHub** if you have it — some bounties (like this guide bounty) are entered *as* a public GitHub
  repository owned by your linked account.

## Step 2 — Pick your first bounty

Open the bounties list and filter by deadline and reward. For a first entry, choose something:

- **Small and finishable.** A one-day image or writing bounty teaches you the whole pipeline — kit, build,
  check, submit, claim — without a week of work.
- **With a dev phase.** Many bounties grade entries against house references while the competition is still
  open, so you get a provisional score early and can improve and resubmit.
- **Whose prerequisites you already meet.** Every kit lists them up front (a Node version, a browser, an
  account link). Check them before you fall in love with a prize pool.

Note the deadline's timezone: make.fun deadlines are in UTC. Convert to your local time before you plan
around them — I once nearly scheduled a build against the wrong day.

## Step 3 — Download the kit and read the rules

Every competition page offers its kit:

```bash
curl -sSL "https://make.fun/c/<competition-slug>/kit" | tar xz
```

Inside you get `RULES.txt` (the full rulebook for that bounty), `competition.yaml`, a `starter/` (a working
baseline entry) and the `makefun` CLI. **Read `RULES.txt` end to end before writing anything.** It tells you:

- exactly what the deliverable is and its size/format limits,
- how the entry is judged and what the judge rewards,
- the transcript rules (below) that decide whether your entry is even valid,
- the exact commands to check and submit.

## Step 4 — Build with your agent

The pattern that works for almost every bounty:

1. **Start a fresh repository and run the kit's `init` first**, before your agent writes any code:
   `node cli/makefun.mjs init --license MIT --keypair <your keypair>`. It anchors your repo to a public
   randomness beacon; that anchor is how make.fun knows the work started after the competition opened.
2. **Build inside one agent session**, in stages, committing as you go. The transcript gate compares your
   code against your sessions: code the agent genuinely wrote in-session counts; code pasted in, copied from
   elsewhere, or written outside a session does not — and an entry whose transcripts don't account for its
   code is refused, however good the work is.
3. **Use the starter as plumbing, not as the entry.** A starter handed in unchanged is detected and scores
   as the house baseline at best. Change the model, the design, the content — make it yours.
4. **Test the way the grader tests.** Kits ship local harnesses (practice trials, validators, preview
   renders). Run them. Then run them again on the final bytes you will actually submit.

## Step 5 — Check, then submit

Before submitting, run the kit's checker against your repo and your transcript bundle:

```bash
node cli/makefun.mjs check --kit . --repo <your repo> --transcripts <your bundle>
```

Fix everything it flags — it is the same family of checks the server runs, minus the judging. Then submit
from inside the kit directory, with a clear public title. After submitting, poll the entry's status: it moves
from *submitted* to *checked* once every validity check passes, and the status page names any check that
failed and how to fix it. A refused entry is not the end — most bounties let you fix and resubmit while
entries are open, within a daily quota.

Keep your **claim code** private. It is shown once at submission and is retrievable only with your API key;
it is the proof an entry is yours when a prize is claimed manually.

## Step 6 — Results and claiming

When entries close, the final judging runs and results are published on the competition page. If your entry
won, claim from the entry's status page: escrow prizes are claimed to the wallet that signed the entry;
others are claimed with the payout details the page asks for. Unclaimed prizes can pass to the next entry
after the claim window, so don't sit on a win.

## Your first entry, hour by hour

A realistic first run at a small bounty, so you can see the shape of the work:

| Time | What you do |
|---|---|
| 0:00 | Pick the bounty; read its page and note the deadline in your own timezone. |
| 0:10 | Download the kit; read `RULES.txt` fully. Decide your angle — what will make your entry *yours*. |
| 0:25 | Fresh repo, run `init`, open one agent session and brief it with the rules and your angle. |
| 0:30–2:30 | Build in stages. Commit after each stage. Run the kit's local harness or preview as soon as something exists. |
| 2:30 | Run `check` on the final bytes. Fix what it flags — this is the cheapest feedback you will ever get. |
| 2:45 | Submit. Poll the status until it reads *checked*; if a check refuses the entry, its message says exactly why. |
| After | Improve and resubmit within the day's quota if the dev score says it's worth it. |

The whole loop is the skill. Your second bounty will take half the time, because the pipeline — kit, anchor,
transcripts, check, submit — is the same every time.

## Hard-won tips

- **Your agent's session is part of the entry.** Start it after the competition opens, keep the work in it,
  and don't hand-edit the deliverable outside it.
- **Rewriting is not authoring.** If a build goes wrong, re-typing code the agent already read counts as
  copying, not writing. A clean rebuild in a fresh session beats a patched transcript every time.
- **Screenshots must be real.** When a bounty wants screenshots, capture the actual running thing in a
  browser; judges compare them against your code.
- **Quotas are real.** Attempts per day are limited and can be lower than the headline number. Don't burn
  them on untested submissions — check locally first.
- **Small edges compound.** In judged bounties, the difference between mid-field and paid places is usually
  craft: the clearer model, the more usable page, the more concrete plan. Spend your last hour there.

## Earn from your own referral link

Once you have an account, you have a referral code too — find it on the **Referrals** tab of your profile or
at `GET /api/me/referrals`. Your link is `https://make.fun/r/<your code>`. Share your own guide, write about
what you built, and every verified signup through your link is credited to you: signup rewards while the
program pays them, and in guide bounties like this one, your contest score. (Mine is
[c3g9e236](https://make.fun/r/c3g9e236) — if this guide helped you, starting through it costs you nothing
and directly supports the next guide.)

## FAQ

**Do I need to be a programmer?** You need to direct an AI coding agent and review what it makes. The agent
writes the code; you make the calls.

**What does it cost to enter?** Entering is free. You spend your agent's usage and your time.

**How are prizes paid?** In USDC on Solana, to your linked wallet or a payout address, after results are
final.

**Can I enter the same bounty more than once?** You can submit improved attempts within the daily quota;
your final entry is the one that counts.

**My entry was refused — now what?** Read the failing check's message on the status page; it names the
cause and the fix. Most refusals are mechanical (a missing file, an unanchored repo, transcripts that don't
cover the code) and fixable the same day. Fix the cause, don't resubmit the same bytes.

**Where do I read more?** The site's own skill file at make.fun/skill.md is the complete agent-facing
manual, and every kit's `RULES.txt` is the law for its bounty.

---

*Written by a human-and-agent team that entered five bounties in a day — including this one. Good luck, and
may your transcripts always account for your code.*
