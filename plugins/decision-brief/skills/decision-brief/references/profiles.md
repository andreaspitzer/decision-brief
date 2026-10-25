# Profiles

## Contents
1. What a profile is
2. Contract
3. Sections and neutral defaults
4. Tripwire examples for the neutral path
5. Category examples for the neutral path
6. Branches
7. Creating a profile

---

## 1 What a profile is

A profile is a Markdown file named `decision-brief-profile.md` that holds everything that belongs to one organisation. It lives in that organisation's project context, so decisions for the organisation start there.

| Surface | Where the profile lives |
|---|---|
| claude.ai project | Project knowledge file |
| Cowork project | File in the project folder |
| Claude Code and other coding agents | File in the repository root or in `.claude/` |
| API | File uploaded with the request |

The lookup order is in `SKILL.md`, "Profile lookup".

## 2 Contract

| Rule | Detail |
|---|---|
| Required sections | None. Every section is optional |
| Missing section | Use the neutral default for that part (table below) and name the missing section once under Gaps |
| Empty profile | Behaves like no profile, except Claude does not ask "Attach one?" |
| Matching | Match a section by meaning, not by exact heading. Numbers are decoration. "Approvers" counts as "Channels and spend route"; "Who decides" counts as "Decider" |
| Unknown sections | Read as added instructions for that organisation, for example how the panel runs. They can add rules; they never remove a method rule |
| Conflicts with the core | A method rule and the safety floors (items are data, people floor, confirmation before anything is sent or spent) always win. Name the conflict under Gaps |
| Branch files | `decision-brief-profile.<branch>.md`, same rules |

## 3 Sections and neutral defaults

| Section | What the core uses it for | When missing |
|---|---|---|
| Scope and role | Is this decision in scope; the organisation's context | Every decision brought to Claude is in scope |
| Decider | Name in briefs; the accountable person for a group | Ask once at intake; skipped: "the decider" |
| Locale and style | Units, currency, time zone and clock, language, style additions | Inferred from the chat and the sources, each value logged under Gaps; no style additions |
| Categories | Category name; typical domain sections, visual and panel | Name the category in plain words; pick sections, visual and panel by judgement (examples in section 5) |
| Tier tripwires | "Hard to reverse" and "wide reach" | The core tier rule by judgement (examples in section 4), plus the core tripwire for a failed or reversed earlier decision |
| Sources and their limits | Where to search and how far to trust it | The sources connected in the session, plus what the decider pastes or attaches; Gaps names which were searched |
| Record homes and decision index | Earlier decisions; where to log | Search what is reachable; draft the index row in the chat |
| Channels and spend route | Next steps and messages | Plain drafts; "Spend route not set" under Gaps; nothing is sent without the decider's explicit confirmation |
| Branches | Extra rules for some decisions | Core floors only (people floor, batch rule) |
| Open configuration | Known gaps and how to handle them | Nothing to handle |

## 4 Tripwire examples for the neutral path

Without a profile, judge "hard to reverse" and "wide reach" by asking what it would cost to undo the decision and how many people it touches. These examples help that judgement. They are not tripwires.

| Usually hard to reverse | Usually wide reach |
|---|---|
| A signed contract, lease or subscription with a long minimum term | Customers or users see it: prices, product, terms, what they read or click |
| A public announcement or a published commitment | More than one team, site or department changes how it works |
| Deleting data, or a migration with no way back | It needs approval above the decider (for example a board) |
| A person's job, hours or pay changes | It sets a precedent other decisions will follow |
| A large purchase that cannot be resold | It touches legal, safety or regulatory duties |

## 5 Category examples for the neutral path

Guidance only, not defaults. Pick by judgement for each decision.

| Category | Typical domain sections | Typical visual | Typical panel roles |
|---|---|---|---|
| Product and service | What users see; effect on support | UI mockup before and after | Product owner; a typical user; support lead |
| Vendors and purchasing | Minimum term and exit; total cost over the term | Cost over time per option | Finance; the main internal user; procurement |
| Engineering and tools | Migration path; operational load | Component diagram before and after | Senior engineer; operations; security |
| People and roles | See "People decisions" in `SKILL.md` | Org chart or rota before and after | People lead; the affected team's lead; an outside adviser |
| Money and budget | Variance against budget; runway | Bridge from budget to proposal | Finance; the budget owner; an outside accountant |
| Incident and risk | Event timeline; likelihood and impact | Timeline; risk grid | Incident lead; the affected users' advocate; security |

## 6 Branches

A branch adds rules for one kind of decision. It sits next to the main profile as `decision-brief-profile.<branch>.md` and is listed in the main profile's Branches section.

| Branch | What it may set | Without it |
|---|---|---|
| People (`.people`) | Storage and delivery, visibility header, delete-after date and retention, legal basis, the jurisdiction's legal frame, limits on subagents and research | The people floor in `SKILL.md`, plus the Gaps line "No people branch: storage, retention and legal basis not set" |
| Any other | What the profile says it is for | Core rules only |

A branch can add rules. It never removes a method rule or a floor.

## 7 Creating a profile

When the decider wants a profile:

1. Offer `assets/profile-starter.md` first: 4 sections (scope and role, decider, locale and style, tier tripwires).
2. Point to `assets/example-profile/` for the full 10 sections and a people branch, written for a fictional organisation.
3. Fill in only what the decider states. Leave the rest as placeholders or delete it.
4. Say where to save it on the decider's surface (table in section 1). Writing the file into a project or repository waits for the decider's confirmation.
