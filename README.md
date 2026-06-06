# Agent Skills by Kevin Luten

## Voice Profile Builder

If you're like me, you've asked AI to write in your voice and gotten back something that sounds like nobody in particular. Voice Profile Builder, an Agent Skill for Claude Cowork, Claude Code, and compatible tools, fixes this like a tailor would: it takes your measurements. The method is stylometry, the statistical analysis of writing style. In one guided session it computes z-scores for your samples against MASC, a corpus of professional American prose: function-word frequencies across the 150 most common words, sentence-length distribution, and punctuation rates (mine: parentheses, constantly). The deviations become a plain-language portrait you correct, packaged as one context file for your vault. Then it checks the fit: it generates drafts from the profile, scores them against a held-out piece of your real writing, and tightens the file until they converge. Attach it to any AI conversation. The bar? You recognize yourself.

### Install

**[Download the skill (zip)](https://github.com/kevin-buffalo/kevin-luten-skills/releases/latest/download/bdw-voice-profile-builder.zip)**

- Claude (web or desktop): Settings > Capabilities > Skills > upload the zip
- Claude Code: unzip into `~/.claude/skills/`

MIT licensed. Feedback welcome via Issues.
