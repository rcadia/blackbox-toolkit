# Skill template spec

## Goal

Every skill in this repo (`skills/bb-*/SKILL.md`) follows one structure. This document is that structure's spec: it defines every field and section a skill must have, which of them are `REQUIRED`, and what `REQUIRED` means.

There is no separate boilerplate file to copy — this document is both the spec and the reference. Scaffold a new skill by writing `skills/bb-<name>/SKILL.md` directly against the Structure section below, then run it through the Validating checklist before shipping.

## Voice

Write every `SKILL.md` like a senior engineer handing off a runbook to someone who will follow it literally and will not fill in gaps correctly on their own. Assume nothing gets inferred that wasn't spelled out.

- **Imperative, not descriptive.** "Navigate to $url and capture the network log," not "the skill navigates to $url and captures the network log." Tell it what to do, don't narrate what it does.
- **Opinionated, not encyclopedic.** State the one right way to do the step. Don't list every possible approach and let the reader pick — that's a design doc, not an instruction. If a real judgment call exists, say what to decide and how, not "consider your options."
- **No filler.** Cut hedges, throat-clearing, and restatement ("basically," "it's worth noting," "as mentioned above"). If a sentence doesn't change what the reader does, delete it.
- **Concise over complete-sounding.** Shorter and exact beats longer and thorough-sounding. This is what the 500-line and 50-line budgets in Rules are for — they're a symptom of this same standard, not a separate one.
- **Every instruction is executable as written.** No step should require the reader to guess intent, resolve an ambiguity, or already know something the doc didn't say.

## Rules

- A field or section marked `REQUIRED` below must have a real, filled-in value in the shipped skill. An empty `REQUIRED` field — a leftover placeholder, a bracketed `<...>`, a `TODO`, or a blank — is a failure. The skill is not done, not reviewable, and must not be merged or shipped in that state.
- Fields marked `OPTIONAL` may be omitted or deleted entirely if they don't apply to a given skill, but only if the skill explicitly doesn't need them — silently dropping a section because it's inconvenient is not the same as it being inapplicable.
- A generated `SKILL.md` should be around 500 lines, in general. Treat that as a budget, not a target to fill — if the skill is done in fewer lines, stop. Going meaningfully over means the skill is doing too much and should be split.
- Any code example inside a `SKILL.md` should be around 50 lines. Longer than that, trim it to the part that illustrates the point, or move it to a separate file the skill reads/references instead of inlining it.

## Structure

### Frontmatter

| Field | Status | Notes |
|---|---|---|
| `name` | REQUIRED | `bb-<verb-or-noun>`, matches the folder name. |
| `description` | REQUIRED | What it does, what it consumes, what it produces, where it saves output, trigger phrases, `Use for ...` examples. |
| `argument-hint` | REQUIRED | Even if the only argument is `<url>`. |
| `arguments` | REQUIRED | Must list every named argument the skill body references. |
| `disable-model-invocation` | REQUIRED | Set `true` for skills that act against a live target. |

### Body

| Section | Status | Notes |
|---|---|---|
| Opening paragraph | REQUIRED | States the target, the no-source-access constraint, that this is the only file the skill touches, and the passivity boundary (or its explicit, narrower replacement — see template). |
| Ground rule: observed vs. inferred | REQUIRED | Every skill restates this in its own terms — do not just reference `bb-recon`'s copy. |
| Discovery questions | REQUIRED (except `bb-recon` itself) | Non-negotiable, first thing the skill does: check `./bb-recon.md` exists at the project root. If it's missing, stop and prompt the user to run `bb-recon` first — do not proceed on a guess at context it would have provided. |
| Prior-skill input section | OPTIONAL | Required only if this skill consumes another skill's output (e.g. `bb-recon`'s snapshot). Delete if this skill takes a URL directly and nothing else. |
| Definition of done | REQUIRED | Checklist, not prose. Must include: every section present or explicitly marked n/a; every claim traceable to an observation; every inferred claim tagged; no unintended state-changing action; only the one output file touched. |
| What's next | REQUIRED | Name the downstream skill(s) that consume this output, or state explicitly that this is a terminal skill with nothing downstream. |

## Validating a skill against this spec

Before a skill ships, confirm:

1. Every `REQUIRED` row above has a real value in the file — no brackets, no `TODO`, no blanks, and no verbatim copy-paste from `bb-recon` where the skill's own content should be.
2. Every `OPTIONAL` row is either filled in or cleanly removed, not left half-done.
3. The skill still reads as this toolkit's voice — black-box, observed-vs-inferred, one-file-touched — not just structurally compliant.
