# Recording decisions

## Contents
1. Terms
2. Checking earlier decisions
3. Index row
4. Who writes what
5. When the decider decides
6. Calibration

---

## 1 Terms

| Term | Meaning |
|---|---|
| Brief home | Where the brief lives: the chat, unless the profile sends a kind of brief elsewhere (for example a private document) |
| Record home | Where the decision record lives. The profile names it per kind of decision; for engineering it follows the repository's own rules (for example decision records in the repository) |
| Decision index | The one list where every brief gets a row. The profile names it and says who can see it |

Without a profile, the brief home is the chat, and the index row and record are drafts in the chat for the decider to file.

## 2 Checking earlier decisions (always first)

1. Search the decision index for the same or a related question (subject, system, vendor, team). No index named: search what is reachable (repository decision records, documents, earlier chats if available) and say what was searched under Gaps.
2. Open the record homes the matching rows link to.
3. In Higher-level context, list each relevant earlier decision with its outcome and a one-line reason. Never show hit rates or scores of past recommendations.
4. If an earlier decision on the same question has outcome *failed* or *reversed*, raise the tier to at least Standard and name this tripwire in the header.
5. In Light briefs, add a "Builds on" line under the Recommendation when a related decision is found.

## 3 Index row

Columns: date · decision in one line · category · tier · choice and whether it matched the recommendation · links to brief and record · status (open, decided, superseded, reversed, lapsed) · success signal · review date · outcome.

A profile whose index has its own format (for example a list in a repository file or a spreadsheet) maps these columns onto it and says how.

People decisions: the row shows only "People decision", role and team, never the person (people floor in `SKILL.md`), unless the people branch sets a stricter rule.

## 4 Who writes what

| Write | Who |
|---|---|
| Open row when a brief is written | Claude alone, where the profile names an index Claude can write to; otherwise Claude drafts it in the chat |
| Lapsed status when a deadline passes undecided | Claude alone, under the same condition |
| Decided row, record-home record, outcome value | Claude drafts, the decider confirms |
| Rows for people decisions | As the people branch says; without one, a draft in the chat with the people floor applied |

## 5 When the decider decides

| Step | What |
|---|---|
| 1 | Update the brief header status: the choice, success signal, review date |
| 2 | Draft the decided index row and the record-home record (for engineering, a decision record in the repository's format); show both, write after the decider confirms |
| 3 | Draft next steps for the chosen option plus the messages they need, in the profile's channels and formats. Anything that sends, books, posts, publishes or spends waits for the decider's explicit confirmation |
| 4 | If spend is involved, follow the profile's spend route; without one, draft the request and list "Spend route not set" under Gaps |

Set the success signal and review date at this point, together with the decider. The success signal is the matrix axis for the chosen option, made measurable where possible.

## 6 Calibration

At the review date, gather the evidence for the success signal and draft the outcome: held, partly held, failed, or too early to tell, with a one-line reason and register rows. The decider confirms the outcome before it enters the index.

Later briefs show relevant past decisions in Higher-level context with outcome and a one-line reason. Never hit rates.
