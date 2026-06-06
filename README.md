# Agent Skills by Kevin Luten

## Voice Profile Builder

Voice Profile Builder creates a context file: a My Writing Voice document for your vault that you attach to any AI conversation, so when you ask for a draft, the output sounds like you. It's an Agent Skill for Claude Cowork, Claude Code, and other tools that support skills, built around one guided session. You supply samples of your real writing. The skill measures them against stylometric norms bundled inside it, precomputed from MASC, a corpus of professional and business written American English: z-scores on your rates for the 150 most common words, sentence-length distribution, and punctuation habits, to find where you measurably depart from typical. Those findings become a plain-language portrait you review and correct. It then verifies the result by generating a fresh draft from the profile and tightening the file until that draft converges on a held-out piece of your real writing. 

### Install

**[Download the skill (zip)](https://github.com/kevin-buffalo/kevin-luten-skills/releases/latest/download/bdw-voice-profile-builder.zip)**

- Claude (web or desktop): Settings > Capabilities > Skills > upload the zip
- Claude Code: unzip into `~/.claude/skills/`

MIT licensed. Feedback welcome via Issues.
