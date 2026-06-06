---
name: bdw-voice-profile-builder
description: Walk a user step by step through building a portable "My Writing Voice" reference document from samples of their own writing — a document they attach to future AI chats so drafts come out sounding genuinely like them, to the point they recognize themselves in the output. Use this whenever someone wants AI to write more like them, mentions capturing their writing voice/style/tone, asks for a voice guide or style profile for themselves, says AI drafts "don't sound like me," wants a reusable reference they can paste or upload into future chats, or asks to turn samples of their writing into something AI can match. Also trigger when a user references measuring or fingerprinting their writing style, stylometry, or "training" AI on how they write. Produces one self-contained Markdown document with no external dependencies. This skill builds a voice profile; it does not screen for or fix AI-sounding writing.
---

# Voice Profile Builder

## What this skill produces

One deliverable: a portable **voice profile document** (Markdown). The user attaches or pastes it into any future chat, and the AI there uses it to write in the user's voice. The document is static text — it does no measuring or computing later. All analysis happens during the build; the output is a clean, self-contained reference the user keeps and can use anywhere, forever, with no code or files required.

The bar to clear: the user should **recognize themselves** in writing produced from this document.

Hold two principles throughout:

1. **Voice, not structure.** This document captures *how the person writes* — rhythm, word choices, punctuation habits, formality — independent of *what* they are writing. It must NOT contain rhetorical or structural advice (opening hooks, persuasion frameworks like AIDA or problem-agitate-solution, where to put a call-to-action, listicle-vs-narrative). Those are task-dependent copywriting decisions. A good voice profile lets the person write a punchy hook-driven post *and* a slow reflective essay and have both sound like them. See `references/voice-components.md` for the exact line.

2. **The person's own writing only.** This builds a profile of the user from the user's own samples. It is not for imitating other people. If asked to capture someone else's distinctive voice without their involvement, decline — that is the text equivalent of a deepfake.

## The method: discover, describe, verify

The skill works in three movements, each using the right instrument for its job:

- **Discover** what makes the writing distinctive — *by measurement against published norms*, not guesswork. This establishes, objectively, where the user departs from typical English.
- **Describe** those distinctive traits as a human-readable, recognizable portrait — the close-read that turns measured deviations into a voice the user recognizes.
- **Verify** that the document actually transmits the voice — by generating a draft from it and measuring how close it lands to the user's real writing, then tightening the document where it drifts.

The full three-movement process is the **default** — it's the skill at full strength. Ask for enough samples to run it. When a user can only supply thin material, the skill falls back gracefully to a reduced version (described in Step 3), but full is the goal, not the exception.

## The build, end to end

A guided conversation, not a form. Move one decision at a time; confirm before proceeding.

1. Orient the user and name the context this voice is for.
2. Gather writing samples — aiming for enough to run the full method.
3. Assess the material and set the path (full by default; reduced only if thin).
4. **Discover** — measure the samples against norms to surface distinctive traits (full path).
5. **Describe** — close-read into a recognizable portrait, grounded in the measurements.
6. Confirm and correct with the user (human gate).
7. **Verify** — generate from the draft document, measure fidelity, tighten (full path).
8. Assemble and deliver the document.

---

### Step 1 — Orient and name the context

Briefly explain what's being built, then help the user name what this voice is *for*. Plain and warm:

> We're going to build a reusable profile of your writing voice — a short document you keep and attach to future chats so AI drafts sound like you wrote them.
>
> A voice profile works best built for **one** context. Think about where you'll mostly use it — your general work writing, a specific client, your personal/social writing, school. You *can* build separate profiles for distinct places (a LinkedIn voice and a formal-email voice), but for most people a single "work voice" covers the bulk of what they write and is the best place to start. What's this one for?

The answer is a **label**, not a logic branch. Its one downstream job: at Step 2, check the samples actually come from that context.

**One scope check here.** If the request is for a *team* or *house* voice ("so everyone's posts sound consistent"), that's a different artifact from one person's voice — it draws on many people's writing and serves consistency, not individual recognition. This skill builds *one person's* voice. You can still help, but name the distinction and confirm direction: either build a profile of one representative person's voice, or treat it as a house-style guide assembled from multiple contributors' samples (in which case the measurement movements, which assume a single author, won't apply and you'll rely on the close-read across contributors). Don't silently treat a team request as if it were one person.

### Step 2 — Gather samples

Ask for samples **from the named context**, and aim high, because the full method needs enough text:

> Now share samples of your actual writing from that context — paste them in or upload files. The more the better. For the full analysis I'd love **several pieces totaling a good amount of text** — think a handful of posts, emails, or articles. Anything you genuinely wrote works. Avoid things heavily edited by someone else, co-written, or written to a rigid template — those dilute your voice.

**Encourage a little variety within the context.** If every sample is one format (all LinkedIn posts), you capture voice *fused with* that format's conventions and can't tell them apart. A couple of samples in a different format from the same context act as a control — the traits that persist across formats are the real voice. Nudge:

> If you can, include a couple of different *kinds* of writing from this context, not just one format. It helps separate your actual voice from the habits a format forces on everyone. Not required, but it sharpens the result.

As samples arrive, read them and assess **total word count** and **number of distinct pieces**. Sanity-check they match the named context; if not, say so and ask whether to adjust the label or swap samples.

### Step 3 — Assess material and set the path

The full method (Movements 1 and 3 use code) needs both **enough text** and **a code-capable session**. Check both by simply proceeding to try the measurement when you reach it; if code can't run, fall back. For material:

- **Full path (default)** — at least **2,000 words across at least 3 distinct pieces**. Runs all three movements. This is the target; Step 2 should have aimed here. The floor exists because the measurement needs enough text to trust: function-word rates and sentence-length distributions get noisy below it, and the verify loop has to hold one piece out without starving the rest. Most people genuinely have this much writing — don't lower the bar to spare them the asking.
- **Reduced path** — less than that, or a code-capable session isn't available. Runs the Describe movement only (close-read), with an honest confidence note and an invitation to upgrade later.

Frame the full path as the normal expectation, not a premium tier. If the user is short of material, don't treat it as failure — explain plainly what changes:

> To run the full analysis I need at least 2,000 words across at least three separate pieces. You've given me about **[X] words** across **[N]**. Two honest options:
>
> **Add a bit more** to reach the full version — where I measure your writing against typical-English norms to pin down exactly what's distinctive, then test the finished profile by generating a draft and checking how close it lands to your real writing, tightening until it matches.
>
> **Proceed now with what you have** — I'll do a close, structured read and build a real profile from it. It just won't include the measurement and the generate-and-test tuning. You can upgrade later with more samples.

If material is very thin (a single short piece), be straight that only a light close-read is possible, and the result is a starting point to revisit with more writing.

### Step 4 — DISCOVER (full path): measure against norms

Read `references/measurement.md` and run the discovery measurement. This is where uniqueness is *established objectively*, the methodical way — not by impression. It measures the user's samples against bundled English norms (the `references/english_norms.json` file that ships with this skill, precomputed from a modern professional-register corpus) and computes z-scores showing exactly where the writer departs from typical: function words used far more or less than normal, sentence-length distribution, punctuation rates, plus characteristic n-grams spotted by eye.

The norms ship inside the skill — nothing is downloaded at build time, and the only dependency is Python's standard library. The measurement needs a code-capable session to run; if code can't run, fall back to the Describe movement and tell the user the measurement was skipped. Nothing from this step ships in the output document; the norms and z-scores are scaffolding.

The z-score *ranks* candidate traits; it does not decide them. Flag strong (|z|>2) and near-threshold (|z|>1.5) candidates and hand them to the close-read, which confirms which are genuine voice — never let the number alone include or exclude a trait (`measurement.md` explains why).

The output of this step is an evidence list — objective deviations — that feeds the close-read. Do not show the user raw statistics as the deliverable; they are inputs to the human-readable portrait.

### Step 5 — DESCRIBE: close-read into a recognizable portrait

Read `references/voice-components.md` and turn the samples — now with the measured deviations from Step 4 in hand — into a human-readable voice portrait. Measurement tells you *that* sentences fracture and semicolons are absent; the close-read explains *how those combine* into a recognizable voice, with the user's own excerpts as anchors.

On the full path, every descriptive claim should trace to evidence — either a measured deviation or a quotable sample. On the reduced path, this movement runs alone, using the component grid in `voice-components.md` and reading explicitly for departures from ordinary English (the file explains the method).

The cardinal rule: **specific and evidence-based, never generic.** "Professional and clear" describes nobody. Capture what is *distinctive* — what separates this writer from a generically competent one.

### Step 6 — Confirm and correct (human gate)

Show the user the portrait and invite correction. Some of their voice is invisible to them, and some of what you inferred is wrong — they'll know which. Present it in plain terms; ask what's right, what's off, what's missing. When they correct you, that's an instruction for revision — integrate the substance, don't quote it back into the document. Hold here until they confirm.

### Step 7 — VERIFY (full path): generate, measure, tighten

Skip on the reduced path. Read `references/measurement.md` for the procedure. This tests whether the document actually transmits the voice: draft the directive block, hold out one of the user's real pieces, have AI write a fresh piece to the same brief using only the draft document, and measure how close the AI piece lands to the held-out real piece (cosine similarity on function-word profiles — robust on short texts, no contrast corpus needed). Where it drifts, tighten **the document** (not the generated text) and repeat, watching the similarity improve across iterations. Because short samples make any single score noisy, require a real trend before declaring done, and if scores bounce without improving, say so and stand on the close-read rather than chasing noise.

The measurement is build-time scaffolding. Once the document is tuned, discard it.

### Step 8 — Assemble and deliver

Read `references/document-template.md` and build the document to that structure: a human-readable **profile** (reference, for the human) plus an **AI-facing directive block** (the operative section the future AI follows). Label which is which so a future AI knows what to act on.

The directive block also carries, in this skill's scope (voice fidelity, never AI-detection):
- an **anti-caricature** note (don't exaggerate an occasional habit into a tic),
- a **voice-fidelity self-check** (after drafting, re-read and ask whether it sounds like the person),
- the **apply-voice-only** guardrail (don't infer structure from the profile),
- a closing **drift note** (rebuild from fresh samples when it stops feeling right).

Include concrete excerpts from the user's **own** writing as examples. Ask once whether the document is **personal or will be shared**; if shared, strip verbatim excerpts and keep descriptions abstract.

**Close every path with an honest fidelity statement:**
- Full path: report that the profile was tuned against the user's real samples until the generated draft converged (plain language, no false precision).
- Reduced path: give a short **confidence read** instead — high / moderate / limited, based on how much material there was, how many pieces, how consistent the patterns, whether samples matched the context — and name what would raise it.

Save and present the document (use `present_files` if available; otherwise save to outputs and say where). Remind the user in one line how to use it: attach or paste it at the start of a chat, or add it as a project/reference file.

---

## Tone and judgment

- A guided conversation. One thing at a time; explain *why* each step matters in plain language. The only technical idea that should reach the user is a friendly "measuring how close it sounds" — terms like Burrows' Delta, cosine similarity, and stylometry stay in your reasoning, not in what you say.
- Resolve each decision before moving on. Name blockers (thin samples, no code) honestly rather than pap