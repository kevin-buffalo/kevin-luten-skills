# Measurement: Discover and Verify (Full Path)

Two movements use code: **Discover** (Step 4 — find distinctive traits by measuring the user's samples against bundled English norms) and **Verify** (Step 7 — test that the finished document transmits the voice). Both need a code-capable session. If code can't run, skip measurement, tell the user plainly, and run the close-read (Describe) alone.

The norms ship *with this skill* as `references/english_norms.json` — nothing is downloaded at build time. They were precomputed once from MASC (the Manually Annotated Sub-Corpus of the Open American National Corpus), restricted to professional/business **written** registers (email, letters, newspaper, technical, essays, journal, non-fiction, govt-docs, travel-guides, blog) — modern American English produced from 1990 onward, public-domain/unrestricted. The file holds, for the 150 most common words, their mean rate and standard deviation across documents, plus the same mean/SD for structural features (sentence length, share of short/long sentences, semicolons and dashes per 1k words). Mean + SD is what lets us compute real z-scores. The only dependency is the Python standard library.

The output document carries none of this — the norms are reference scaffolding used during the build.

---

## MOVEMENT 1 — DISCOVER (Step 4)

Goal: establish *objectively* where this writer departs from typical professional English. This is the methodical alternative to guessing what is distinctive. The output is a ranked list of candidate traits that feeds the close-read — never shown to the user as the deliverable.

For each measured feature, compute a **z-score**: how many standard deviations the user's value sits from the norm. The z-score is the actual statistic underneath Burrows' Delta, and the bands are standard inferential-statistics conventions, not invented cutoffs:

- **|z| > 3** — strongly distinctive.
- **|z| > 2** — notably distinctive.
- **1.5 < |z| < 2** — worth examining; flag as a *candidate*, do not assert on the number alone.

Crucial rule, learned from testing: **the z-score ranks candidates; it does not decide.** A trait at z = 1.5 that recurs visibly in every sample (e.g. a writer's habitual short fragments) is genuine voice even though it sits below the strict bar — because plenty of professional writing also runs short, so the statistic understates a trait that reading plainly confirms. Likewise, a single high z on a rare word may be noise. So: measurement surfaces and ranks; the close-read (Step 5) confirms which candidates are real and how they combine. Never let the number alone include or exclude a trait.

Working pattern (tested end-to-end):

```python
import json, glob, re, statistics as st
from collections import Counter

norms = json.load(open("references/english_norms.json"))
W = norms["word_norms_mean_sd"]      # word -> [mean_rate, sd]
S = norms["structural"]              # feature -> [mean, sd]

def z(val, pair):
    m, sd = pair
    return (val - m) / sd if sd else 0.0

texts = [open(p, encoding="utf-8", errors="ignore").read()
         for p in glob.glob("SAMPLES_DIR/*.txt")]
full = "\n\n".join(texts)
wl = re.findall(r"\b[\w']+\b", full.lower()); n = len(wl); c = Counter(wl)
sents = [s for s in re.split(r'(?<=[.!?])\s+', full) if s.strip()]
slens = [len(re.findall(r"\b[\w']+\b", s)) for s in sents]

# word-level candidates
for w, pair in W.items():
    if c[w] >= 3:
        zz = z(c[w]/n, pair)
        if abs(zz) > 1.5:
            tier = "STRONG" if abs(zz) > 3 else ("notable" if abs(zz) > 2 else "candidate")
            print(f"{w}: z={zz:+.1f} {tier} ({'over' if zz>0 else 'under'}-used, n={c[w]})")

# structural candidates
sm = st.mean(slens)
short = sum(1 for l in slens if l < 10)/len(slens)
semi = 1000*full.count(';')/n
print("sentence_len", round(sm,1), "z=", round(z(sm, S["sentence_length_mean"]),1))
print("frac_short",   round(short,2), "z=", round(z(short, S["frac_short_under10"]),1))
print("semicolons/1k",round(semi,1), "z=", round(z(semi, S["semicolons_per1k"]),1))
```

Also surface **characteristic n-grams** by eye from the samples — repeated multi-word habits ("here's the thing," a recurring concessive close). The frequency pass won't flag these; reading will.

Interpretation, from real testing: a terse writer surfaced "just"/"it" overused (z≈2), a conversational "what/they/because" cluster, ~52% short sentences (z≈1.5, a candidate the close-read confirmed as central), and zero semicolons. Note the short-sentence trait sat just below |z|>2 yet was clearly defining — exactly the case the close-read must catch. Hand the ranked candidates to Step 5.

Caveat: on thin samples these rates are noisy; this is why the full path requires 2,000+ words. Treat a deviation as real only when the feature occurs enough to trust, and confirm against your own reading.

---

## MOVEMENT 3 — VERIFY (Step 7)

Goal: test whether the *document* transmits the voice, and tighten it where it doesn't. This is direct text-to-text similarity between the user's real writing and an AI draft built from the document — NOT authorship attribution, so no contrast corpus is needed.

Procedure:

1. **Hold out** one real user piece as the target. Build the draft directive block (Step 8) from the *remaining* samples only, so the target is unseen.
2. **Generate** a fresh AI piece to the same brief the held-out piece answers (same topic/length/context), using only the draft document as voice guidance.
3. **Measure** similarity between the AI piece and the held-out real piece on their function-word profiles, using cosine similarity (robust on short texts). Higher = closer.

```python
import re, math, json
from collections import Counter

# Reuse the 150 common words already in the bundled norms file — no external dependency.
FUNC = set(json.load(open("references/english_norms.json"))["word_norms_mean_sd"].keys())

def profile(text):
    toks = re.findall(r"\b[\w']+\b", text.lower())
    c = Counter(t for t in toks if t in FUNC)
    n = sum(c.values()) or 1
    return {w: c[w]/n for w in FUNC}

def cosine(a, b):
    keys = FUNC
    dot = sum(a[k]*b[k] for k in keys)
    na = math.sqrt(sum(a[k]**2 for k in keys))
    nb = math.sqrt(sum(b[k]**2 for k in keys))
    return dot/(na*nb) if na and nb else 0.0

real = profile(open('held_out.txt').read())
ai   = profile(ai_generated_piece)
print("similarity to real:", round(cosine(real, ai), 3))
```

4. **Tighten the document, not the text.** Read the AI piece against the real one: where did it drift? Add the missed trait or sharpen a vague directive *in the document*. Regenerate from the revised document, re-measure.
5. **Require a trend.** A single score means little on short text. Conclude only when similarity improves across at least two iterations in response to your edits. If it bounces without trending up, stop and say the measurement isn't discriminating at this sample size — stand on the close-read. That is an honest outcome, not a failure.
6. **Fold wins back in.** The document edits *are* the tuning; they ship. Scripts and scores are discarded.

A sanity check from testing: a held-out real piece, an AI piece written in-voice, and a generic AI piece scored in clearly separated bands against the real writing — the in-voice draft landing much closer than the generic one. That separation is what makes the loop's signal trustworthy when there's enough text.

## Framing to the user (both movements)

Keep it plain: "I measured your writing against typical English to pin down what's distinctive, then tested the profile by writing something and checking how close it came to the real you." Never expose Burrows' Delta, cosine, z-scores, or package names. And never claim the document keeps measuring anything after the build — it doesn't. The measurement happened during the build; its results live in the words of the document.
