# Brief format

## Contents
1. Header
2. Section order
3. Domain sections
4. Light skeleton
5. Decision matrix
6. Expert panel
7. Gaps and Conflicts
8. Templates (Standard and Deep, Light)

---

## 1 Header

A short block above the Ask, holding facts about the whole brief only.

| Item | When |
|---|---|
| Who may see it; delete-after date | When a profile branch requires it (for example a people branch), first lines |
| "Support for a decision by [decider]; not legal advice. …" | Every people brief, per the people floor in `SKILL.md` |
| Tier with its reason and any tripwire that fired | Always |
| Status: open with deadline → once decided, the choice, success signal and review date → later lapsed, superseded or reversed | Always |
| Related briefs, with why split or coupled | Only when there are any |

Light briefs carry the same header on one line, empty fields left out.

## 2 Section order

Answer first. Sections 3–6 are called **Setting**; sections 7–9 **Analysis**.

| # | Section | Holds | In Light |
|---|---|---|---|
| 1 | Ask | What is needed from the decider, by when | Yes |
| 2 | Recommendation | Two or three lines; the dissenter's strongest objection in one line; the recommendation's one-line reply | Yes, plus a "Builds on" line when an earlier related decision is found |
| 3 | Higher-level context | Where the decision fits; earlier decisions it builds on or reverses, each with outcome and a one-line reason; never hit rates | No |
| 4 | Default outcome | What happens if nothing is decided, and when that becomes the decision | No |
| 5 | Stakeholders | Who is affected or has a say, with recorded positions cited to the register | No |
| 6 | Scenario | The concrete situation the decision plays out in: who does what, when, with which numbers | No |
| | Domain sections | Up to two, see below | Incident timeline only |
| 7 | Visualisation | See `visuals.md` | Only when the crux has a clear shape |
| 8 | Decision matrix | See below | Six core axes, one line each |
| 9 | Expert panel | See below | No |
| 10 | Communication | Who must be told, in what order; only when relevant (for example people, incidents, vendor exits, anything sent outside the organisation). For people decisions always, with the disclosure reminder | No |
| 11 | Gaps and Conflicts | See below | Gaps only |
| 12 | Evidence register | See `evidence.md` | Links only |

Stakeholder positions come only from what real people said or wrote, cited as register rows. If no position is recorded, write "no recorded position" rather than guessing.

Without an index Claude can write to, the draft index row follows the evidence register, below a rule, under the heading "Draft index row". It is not a numbered section.

## 3 Domain sections

Chosen per decision, not per category: at most two, each with a one-line reason directly under its heading, placed after Setting and before Analysis. Nothing is mandatory.

In Light, domain sections are left out except an incident's event timeline (an incident or risk decision): a sourced table (time · event · register row), oldest first, between the Recommendation and the Matrix.

The profile's category table gives typical domain sections and visuals per category, as guidance only, not defaults. Without one, see `profiles.md`, section 5.

## 4 Light skeleton

Header (one line) · Ask · Recommendation with dissenter line and "Builds on" line · incident timeline (incidents only) · visual (shaped crux only) · Matrix (six axes, one line each) · Gaps · Register links.

## 5 Decision matrix

Options as columns, axes as rows. Always include "do nothing / keep as is" as an option when it is viable; it usually matches the Default outcome.

| Axis | Standard and Deep | Light |
|---|---|---|
| Pros | Yes | One line |
| Cons | Yes | One line |
| Cost in money (the brief's currency) and people-time | Yes | One line |
| Risk with fallback | Yes | One line |
| Reversibility (easy or hard, point of no return) | Yes | One line |
| Success signal (how the decider would know it worked) | Yes | One line |
| Decision-specific axes | As many as needed, each with a one-line reason | None |

Every claim in the matrix cites a register row ([E3]) or an inference row ([I2]). Mark the recommended option in the column header.

Coupled decisions: columns are the viable combinations, not the single options.

## 6 Expert panel

| Rule | Detail |
|---|---|
| Members | Up to four role archetypes plus the dissenter, chosen per decision, each with a one-line reason; labelled as Claude's analysis from that role's viewpoint |
| How it runs | Preferred: one subagent per role, then a merge. Fallback: sequential role passes in one context, each labelled. A profile may say how its panel runs; the other rules still bind |
| Real people | Never simulated. What real people said goes in the register as a graded statement |
| Dissenter | Always present; argues against the recommendation |
| Dissenter evidence | Cites register rows; an argument without evidence is allowed but marked inference |
| Conceding | Never concedes; if nothing strong remains, says so and names the residual risk to watch |
| Last word | Its strongest objection sits under the recommendation; the recommendation replies in one line |
| Missing voice | May name one viewpoint the panel left out and argue from it |

Present the panel as a table: role · why on the panel · view · cited rows. The dissenter is the last row.

The profile's category table gives typical roles per category, as guidance only.

Write the dissenter honestly. Its job is to find the strongest case against the recommendation, so the recommendation can be revised if the case wins. If the dissent is stronger than the recommendation, change the recommendation.

## 7 Gaps and Conflicts

| Part | Holds |
|---|---|
| Gaps | Sources not searched (with reason) or not reachable; evidence that should exist but was not found; truncated messages without a full-text route; missing profile or profile sections; inferred locale values; fallbacks used (visual route, no subagents, no render check); open configuration items that affected this brief |
| Conflicts | Sources that disagree, side by side with their register rows. Never resolve silently; say which one the recommendation relies on and why |

Sources the profile excludes are never listed under Gaps.

---

## 8 Templates

Headings and labels follow the brief's language.

### Standard and Deep

```markdown
# [Decision in one line]

**Tier:** Standard – [reason]; tripwire: [which] · **Status:** open, deadline [date and time] · **Related:** [brief links, split or coupled reason]

## 1 Ask
[What is needed from the decider, by when]

## 2 Recommendation
[Two or three lines, cited]
**Dissenter:** [strongest objection in one line] [E#]
**Reply:** [one line]

## 3 Higher-level context
| Earlier decision | Date | Outcome | Why it matters here |
|---|---|---|---|

## 4 Default outcome
[What happens if nothing is decided; when that becomes the decision]

## 5 Stakeholders
| Person or group | Role in this decision | Recorded position | Source |
|---|---|---|---|

## 6 Scenario
[Concrete situation]

## [Domain section]
*Why here: [one line]*

## 7 Visualisation
**Crux:** [one line]
[visual, per visuals.md]

## 8 Decision matrix
| Axis | Option A (recommended) | Option B | Keep as is |
|---|---|---|---|
| Pros | | | |
| Cons | | | |
| Cost (money, people-time) | | | |
| Risk with fallback | | | |
| Reversibility | | | |
| Success signal | | | |
| [Specific axis] – [why] | | | |

## 9 Expert panel
*Claude's analysis from each role's viewpoint; not real people.*
| Role | Why on the panel | View | Evidence |
|---|---|---|---|
| Dissenter | Argues against the recommendation | | |

## 10 Communication
| Order | Who | What | Channel |
|---|---|---|---|

## 11 Gaps and Conflicts
| Gap | Reason |
|---|---|

| Conflict | Source A | Source B | Relied on and why |
|---|---|---|---|

## 12 Evidence register
| # | Source | Date (age in days) | Author | What it shows | Grade | Flags |
|---|---|---|---|---|---|---|
```

### Light

```markdown
# [Decision in one line]
**Tier:** Light – [reason] · **Status:** open, deadline [date]

**Ask:** [what, by when]
**Recommendation:** [one or two lines] [E#]
**Dissenter:** [one line] · **Reply:** [one line]
**Builds on:** [earlier decision, outcome] (only when found)

| Axis | Option A (recommended) | Option B |
|---|---|---|
| Pros | | |
| Cons | | |
| Cost | | |
| Risk with fallback | | |
| Reversibility | | |
| Success signal | | |

**Gaps:** [sources not searched, fallbacks used, one line each]
**Register:** [E1](link) · [E2](link)
```
