---
layout: archive
title: "Research"
permalink: /research/
author_profile: true
---

## Half of What's in CLAUDE.md Isn't Even Rules: A Look at 4,664 AI Instruction Files

If you've used Claude Code or Cursor, you've probably written a `CLAUDE.md` or `.cursorrules` file. Blog posts call them "instructions," "rules," "policies." The tool vendors document them as ways to tell the AI how to behave.

We scraped 4,664 of these files from public GitHub repositories and looked at what's actually inside.

About half of it isn't rules.

---

### The data

We collected AI agent instruction files from 4,485 public GitHub repositories. CLAUDE.md, AGENTS.md, .cursorrules, copilot-instructions.md, GEMINI.md. We broke each file into 50,314 small units (a bullet point, a section, a code block) and classified each one.

We also paired 4,094 of these files with their repository's README to compare. And tracked 599 repos over time to see how documentation evolved after AI files were introduced.

---

### Finding 1: Almost half is code, not prose

The single biggest category of content is just code examples. 49.5% of all units are code-dominant — meaning they're either code blocks with minimal surrounding explanation, or have less than 30 words of prose around them.

77.8% of repositories contain at least one code-only unit in their AI instruction file.

This is consistent across tools:
- CURSOR: 56% code
- GITHUB_INSTRUCTIONS: 50%
- CLAUDE: 49%
- COPILOT: 45%
- AGENTS: 44%
- GEMINI: 44%

And consistent across repo types:
- Solo developer projects: ~50%
- Team repos: ~50%
- OSS projects: ~50%

So when blog posts tell you "write clear rules in your CLAUDE.md," that captures maybe half of what people actually do. The other half is showing, not telling. People paste in example components, configuration snippets, code patterns, and let the AI infer the rule.

This isn't a quirk of one tool or one community. It's a universal pattern in how developers write for AI agents.

---

### Finding 2: When people do write prose, it's not behavioral instructions

Of the prose content (the half that isn't code), here's what people actually write:

- Domain knowledge: 21.8% — "In this codebase, 'transaction' means database transaction, not financial"
- Architecture/convention: 18.1% — "All handlers go in /src/handlers. Use repository pattern."
- Code style: 11.8% — naming, formatting
- Workflow: 11.5% — PR conventions, branching
- Tool usage: 8.9% — which library to use
- Build/run: 6.4% — how to test, deploy
- Validation: 5.3% — what to check before declaring done
- Boundary: 4.6% — "don't touch /legacy"
- Tone/persona: **2.5%**

That last one is striking. Tone and persona — the "Claude should respond like X" instructions that get a lot of attention in blog posts and prompt engineering threads — is the smallest category. Only 11.6% of repos have any tone/persona content at all.

What dominates is project context. Architecture, domain knowledge, conventions. The kind of thing you'd tell a new engineer joining your team. Not "Claude, be concise." More like "this is a monorepo, here's how it's structured, here's what the domain terms mean."

The framing of CLAUDE.md as a "behavior shaping" tool doesn't match the data. It's much closer to onboarding documentation.

---

### Finding 3: README doesn't shrink

Going in, I expected to see README files getting smaller as AI files took over the operational documentation. The hypothesis was simple: build commands, setup instructions, tooling info would migrate from README to CLAUDE.md, leaving README as a marketing front-page.

That didn't happen.

We tracked 599 repositories from before they had an AI instruction file (T-3 months) through 12 months after introduction. Result:

- 12% of repos: README shrunk
- 47% of repos: README grew
- 41% of repos: roughly stable

On average, README files grew by 37% over a year. Code blocks in README went from 12.9% to 17.1% of content. Imperative language increased. README didn't get cleaner or more marketing-focused. It got more technical.

So AI files aren't taking over README's job. They're adding a second layer of documentation on top.

The correlation between AI instruction file length and README length is essentially zero (r = -0.01). The two documents grow independently. A repo with a long CLAUDE.md doesn't have a shorter README, and vice versa.

The whole substitution intuition is wrong. Documentation labor in the AI era didn't get automated. It got added to.

---

### Finding 4: AI doc and README are different documents

Even though they're in the same repo, the two documents diverge sharply in form.

AI instruction files are command-heavy. They use words like "must," "always," "never," and imperatives ("run X," "use Y," "do not modify Z") at 6× the rate of README files. The effect size is r=0.83, which is about as large as it gets in this kind of comparison. AI files are short and directive.

README files are explanatory. They have more code examples per word, use "you" and "your" more, and read in flowing prose. They explain *why* and *what*. AI files instruct *what to do*.

The two documents look like they were written for different audiences — which they were. One is for a human reader scanning your repo to decide whether to use it or contribute to it. The other is for an AI agent that needs operational context to make code changes.

---

### Finding 5: Repo type matters

Same AI tools, but different documentation outcomes depending on who's running the repo.

| Repo type | % of sample | README growth over 12 months |
|---|---|---|
| Solo developer | 68% | +32% (43% grew, 12% shrunk) |
| Small team | 26% | +47% (59% grew) |
| OSS project | 6% | No significant change |

Solo and team repos keep expanding README even as they add AI files. OSS repos hold steady. Statistically, the difference between OSS and the other two is significant.

A reasonable interpretation: solo developers, who often didn't document much before AI tools, are now documenting more (for the AI, but also for themselves). Teams are using AI documentation as an addition to their existing documentation culture. OSS projects had mature README cultures before AI showed up and aren't changing them.

It's the same tool, but it lands in different organizational soil and produces different results.

---

### What this changes

A few things follow from this data.

**One: "Instructions" is a misnomer.** Half of what's in these files is code examples, not declarative rules. Most of the rest is project context, not behavioral commands. When tool vendors and bloggers call these "instructions" or "rules," they're describing maybe a quarter of the actual content.

**Two: AI documentation is additive, not substitutive.** The story isn't "AI saves us from writing docs." It's "AI made us write a new kind of doc on top of the old kind."

**Three: Onboarding documentation got revived.** Project-specific context that used to live in team members' heads or scattered across wikis is now showing up in checked-in files. AI agents made this content explicit because the agent has no shared context to draw from. A human onboard might absorb it gradually; an AI agent needs it upfront, every time.

**Four: Documentation practices vary by social context.** Solo developers, teams, and OSS projects use the same AI tools but produce different documentation patterns. There's no universal effect of AI on documentation. There's an effect that gets modulated by who's writing.

---

### Limitations

A few things I can't claim from this data.

Public GitHub only. Internal company codebases, private repos, personal notes — all invisible. The patterns might look different in production codebases.

Authorship is unclear. CLAUDE.md is often written by Claude. I can't separate human-written content from LLM-generated content in the corpus.

I don't know if any of this works. The data shows what people write. It doesn't show whether what they write actually helps the AI perform better.

And read patterns aren't visible. Maybe people write these files once and never look at them again. Maybe they iterate constantly. Git commit history hints at this, but doesn't fully answer it.

---

### What's interesting

The thing I keep coming back to is the 49.5%.

Half the content in AI instruction files is code examples. People aren't writing rules; they're showing patterns. They paste in example components and expect the AI to generalize. They show a config and expect it to be reused. They put in a function signature and expect the style to be inferred.

This is documentation by demonstration. It's not how rules are supposed to work, but it's clearly what people are doing.

And given how much of software engineering knowledge is tacit — captured in code, not in prose — maybe this isn't surprising. The AI agent enters a codebase and needs to understand patterns. Patterns live in code. So you give it code.

The thing is, no blog post about CLAUDE.md best practices says this. Every guide is about how to write good prose rules. But half the time, people aren't writing prose rules at all. They're pasting code.

That gap — between what people teach others to do and what they actually do — is where this paper sits.
