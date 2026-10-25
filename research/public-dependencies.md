# Public availability of the skill's dependencies

Research for issue #3, "Which dependencies are available to public users?" – checked on 2026-10-25 against Anthropic's public documentation, help centre, plugin marketplaces and the connector directory.

## Summary

- **Built in, with limits by surface.** Inline visuals (beta, claude.ai web and desktop chat and Cowork; not mobile, not documented for Claude Code or the API), code execution for rendering and checking charts (all claude.ai plans), web search and fetch, Claude Docs (claude.ai, and Claude Code through a built-in connector), artifacts (Claude Code, paid plans) and subagents (Claude Code, Agent SDK and plugin agents in Cowork; not in claude.ai chat).
- **Public, but only on Claude Code.** `dataviz` ships as a bundled Claude Code skill. Claude Code also bundles `/deep-research`, but only the user can start it.
- **Public from a third party.** A popular open-source `archify` skill exists (Apache-2.0), but it is not from Anthropic and is not in Anthropic's official marketplace. Treat it as optional.
- **Not public.** No public skill named `research` and no plugin named `review-panel` is published by Anthropic or listed in the official, knowledge-work or community marketplaces. The public skill must not depend on either.
- **Connectors.** Slack, Gmail, Google Drive and Calendar, and Atlassian (Jira and Confluence) are all in the connector directory and marked "Anthropic verified". Each needs the user's own account, and on Team and Enterprise an Owner must enable it. Slack's connector is documented for Team and Enterprise only. Claude Code inherits connectors from a signed-in claude.ai account.
- **Detection.** There is no documented interface a skill can call to list its tools. Claude sees what is loaded, so the skill should name capabilities by what they do, say "if available, else", and record the fallback it used under Gaps.
- **Recommendation.** Write every dependency in the public skill as "preferred, then fallback". The plain fallback must always work: a Markdown table, a Mermaid or text diagram, sequential role passes in one context, and evidence the user pastes in.

## Dependency table

Surfaces: **Chat** = claude.ai on web, desktop and mobile; **Cowork** = Cowork tasks in Claude Desktop; **Code** = Claude Code (terminal, IDE, desktop Code tab); **API** = Claude API and Agent SDK.

| Dependency | How the skill uses it | Availability per surface | Public? | Fallback when missing |
|---|---|---|---|---|
| Inline visuals in chat ("custom visuals", the Visualizer) | Draws the brief's visual inline, inside the Visualisation section | Chat: built in, beta, "available to all Claude users on web and desktop" [S1]; not rendered on iOS or Android [S1]. Cowork: built in, without sharing or click-to-follow-up [S1]. Code: not documented (unconfirmed); artifacts are the documented route there [S9]. API: not documented (unconfirmed) | Yes, built in | Mermaid in a fenced `mermaid` code block, which GitHub renders [S27]; or inline SVG; in Code an artifact page or a local HTML file [S9]; text diagram in a code block as the last resort |
| Native chart tool | Plain line and bar charts in chat | Help centre documents no separate chart tool. Charts in chat are made through custom visuals ("chart this data") [S1]. Unconfirmed whether a distinct chart tool exists | Covered by custom visuals | Same as inline visuals; or a PNG made with code execution [S12]; always include the numbers as a Markdown table under the chart |
| `dataviz` skill | Design rules for a standalone chart file when the brief lives in a terminal | Code: bundled skill `/dataviz` [S7]. Chat, Cowork, API: not documented (unconfirmed). Not in the public skills repository [S16] | Yes on Code; otherwise no | Put a short chart checklist in the skill's own references (chart form, metric units on every axis, colour-blind-safe palette, direct labels); use any plotting library |
| `archify` skill | Standalone diagram file (component, sequence, flow) when the brief lives in a terminal | Not from Anthropic. A third-party open-source skill of that name exists, Apache-2.0 [S18]. Not in Anthropic's official marketplace [S17]. Installable as a personal or project skill in Code [S6], or uploaded as a zip custom skill in Chat [S5] | Yes, third party, unvetted by Anthropic | Mermaid or inline SVG; in Code the bundled `/artifact-diagramming` skill for diagrams inside artifacts (v2.1.221 or later) [S7] |
| Code execution to render and look at a visual | Renders the visual to an image to check overlaps, clipped labels and colours before sending | Chat and Cowork: "Code execution and file creation", Free, Pro, Max, Team and Enterprise, web, desktop and mobile; Team and Enterprise Owners turn it on; makes PNG charts with Python [S12]. Code: runs locally. API: code execution tool, no network, no package installs [S4] | Yes, built in | Skip the render check and write "visual not rendered and checked" under Gaps |
| `research` skill | One background research pass per source or question, at Standard and Deep tier, writing graded research records | No Anthropic skill of that name: not in the public skills repository [S16], not in the official, knowledge-work or community marketplaces [S17]. Related built-ins: Code bundles `/deep-research`, which "runs only when you invoke it" [S7]; Chat has Research mode on paid plans with web search on [S14] | No (the private one) | In Code, brief a built-in `general-purpose` or `Explore` subagent per source [S8]; in Chat, ask the user to switch on Research, else gather directly; note "gathered directly, no background agents" under Gaps (the skill already does this) |
| Background agents and subagents | Parallel gathering per source; a separate fresh-eyes review pass | Code: built in, background by default, can nest three levels [S8]. API: Agent SDK subagents, can run in parallel [S10]; the plain Messages API has none. Cowork: plugin agents load. Chat: plugin agents are "Ignored" [S20] | Yes, built in on Code, Cowork and Agent SDK | Sequential passes in one context: one pass per source, then one re-read "as the decider would", each labelled. Same output, slower, no parallelism |
| `review-panel` plugin (multi-agent review panel with moderator) | Runs the expert panel as parallel reviewer agents with a moderator verdict, for Standard and Deep briefs | No plugin of that name from Anthropic, and none in the official, knowledge-work or community marketplaces [S17]. Several unrelated third-party GitHub repositories use the name [S19] | No | The core panel rule already works without it: up to four role viewpoints written by Claude, labelled as Claude's analysis, dissenter last. Where subagents exist (Code, Cowork) each role may run as a subagent; a public plugin can ship its own agents (Cowork and Code only) [S20] |
| Claude Docs | Delivery for briefs that should not stay in the chat; charts and diagrams inside the Doc | Chat: web, desktop and mobile; "Free, Pro, Max, Team, and Enterprise plans, and it's on by default", Enterprise via Owner setting [S2]. Code: arrives as the built-in connector `claude.ai Claude Docs` when signed in with a claude.ai account [S9][S11]. Not with customer-managed keys, zero data retention or HIPAA [S2][S9]. Cowork and API: not documented | Yes, built in | A Markdown file in the chat or repository; in Code an artifact page [S9]. Note: an older version of the help article excluded Free; the live page includes it |
| Slack connector | Searches channels, direct messages and threads; drafts messages for the user to send | Directory: "Slack", Anthropic verified [S21]. Chat: Team and Enterprise only, needs the Claude in Slack app installed and an Owner to enable it [S22]. A newer statement that Pro and Max have it is unconfirmed. Code: inherited from claude.ai when signed in [S11]. API: MCP connector for remote servers, beta [S23] | Yes, with plan limits | User pastes or exports the thread; list "Slack not searched" under Gaps |
| Gmail connector | Searches and reads vendor and partner mail; drafts replies | Chat: "all users on Claude and Claude Desktop"; Team and Enterprise need an Owner to enable; attachment content not readable; send needs approval by default [S24]. Code: inherited from claude.ai [S11]. API: MCP connector [S23] | Yes | User pastes or forwards the text; Gaps |
| Google Drive connector (with Calendar and Sheets) | Reads meeting notes, decks and sheets; checks who met when; can hold a decision index in a Sheet | Chat: same terms as Gmail; Drive reads text but "Images embedded in documents are not processed"; separate Docs, Sheets and Slides connectors create and edit files [S24]. Directory lists Drive and Calendar as Anthropic verified [S21]. Code: inherited [S11] | Yes | Decision index as a Markdown or CSV file in the repository; documents uploaded or pasted by the user |
| Atlassian connector (Jira, Confluence) | Searches tickets, reads descriptions, comments and status history | Atlassian's own Rovo MCP server, generally available since 4 February 2026, Claude listed as a supported client [S25]. Directory: "Atlassian MCP", Anthropic verified [S21]. Needs an Atlassian Cloud site. Code: inherited from claude.ai or added as a remote MCP server [S11] | Yes, from Atlassian | User pastes ticket text or exports; GitHub issues for repository-based work |
| Web search and web fetch | Reads primary vendor documentation at every tier; broader research at Deep tier | Chat: web search with URL fetch; Team and Enterprise Owner enables [S13]. Code: built-in web tools. API: web search tool, admin can disable, charged per search [S15] | Yes, built in | Ask the user for the document; grade unread vendor claims as statements and list under Gaps |
| Artifacts (a fallback carrier, not a hard dependency) | Interactive page for a visual or matrix when inline visuals are missing | Code: Pro, Max, Team, Enterprise; signed in with a claude.ai account; Anthropic API only (not Amazon Bedrock, Google Cloud's Agent Platform or Microsoft Foundry); off by default in the Agent SDK and `-p` [S9] | Yes, paid plans | Local HTML file, which Code writes when artifacts are not available [S9] |
| Local files and the code repository | Reads code, decision records and tickets as evidence | Code: native. Desktop: desktop extensions "open to all users on Claude Desktop" [S3]. Chat: user uploads files | Yes | User uploads or pastes the files; missing history under Gaps |
| Claude Code memory | Reads earlier guidance from project memory, graded as a statement | Code: auto memory per repository [S26]. Other surfaces: not used | Yes, Code only | Ask the user; skip |
| Profile-only sources (a company-internal analytics connector, an applicant tracking system connector, private ticket-workflow skills, a people-directory skill not yet built, a private publishing script) | Company-specific evidence and record keeping, named in private profiles only | Private or not built | No | Leave them out of the public skill. Ship a profile template where users list their own sources, record homes and decision index |

## Notes

### How a skill can tell whether a tool is present

| Finding | Source |
|---|---|
| No documented interface lets a skill query its available tools. Skills are instructions plus optional scripts; Claude reads them and acts with the tools already loaded in the session. This is an inference from the absence of such an interface in the skills docs | [S4][S6] |
| Claude Code lists MCP tool names to Claude at session start and loads their definitions on demand (tool search), so Claude can see whether a named tool exists before calling it | [S11] |
| A plugin's skills load "whether or not you connect anything"; a skill that uses a connector's service "can't reach that service until the connector shows Connected" | [S20] |
| Name MCP tools fully qualified (`ServerName:tool_name`) to avoid "tool not found" errors | [S28] |
| Do not assume packages are installed; state them, and check in a script | [S28] |
| `allowed-tools` in a skill's frontmatter pre-approves tools; it "does not restrict which tools are available" and cannot be used as a presence check | [S6] |
| In Claude Code, a plugin can declare `dependencies` on other plugins, which then start enabled. Unconfirmed whether claude.ai or Cowork honour this field | [S29] |
| Claude Code's own artifact feature shows the intended pattern: when a requirement is not met, "Claude writes a local HTML file or says it cannot publish instead" | [S9] |

Recommended wording for the public skill:

| Pattern | Example |
|---|---|
| Name the capability, not the private tool id | "If you can draw inline visuals in this chat, use that. Otherwise write a Mermaid diagram in a `mermaid` code block." |
| One table of capability, preferred route, fallback | Keep it in `references/visuals.md` and in the evidence reference, not spread across files |
| Record the route taken | Under Gaps: "Visual as Mermaid – inline visuals not available on this surface"; "Gathered directly – no background agents" |
| Scripts check for themselves | A bundled script tests for its library and prints a clear message, so Claude can switch to the fallback |
| Never block a brief on a missing tool | Same rule the skill already applies to open configuration items |

### Other points

- The skill's current text names a private multi-agent panel plugin and a private research skill. The public version should describe the panel and the research step as methods, with subagents as an optional speed-up.
- Plugin agents and hooks are ignored in claude.ai chat [S20]. If the public skill later ships as a plugin with agents, chat users still get only the skill.
- Custom skills do not sync automatically between the API and the apps [S4]. Skills enabled on a claude.ai account load in Claude Code v2.1.273 or later when signed in with that account (one-way) [S5]. The README should give install steps for each surface.
- Connector names differ by surface (for example `claude.ai Slack` in Claude Code [S11]). Another reason to describe the capability ("a Slack search tool") rather than a tool id.
- Cowork is limited to paid plans (Pro, Max, Team, Enterprise) [S30]. Help-centre versions disagree on whether Cowork runs on the web and on mobile, so this is unconfirmed.

## Sources

| # | Source | What it supports |
|---|---|---|
| S1 | https://support.claude.com/en/articles/13979539 | Custom visuals in chat and Cowork: beta, web and desktop, not mobile, export options |
| S2 | https://support.claude.com/en/articles/16923645 | Claude Docs: plans, surfaces, charts and diagrams, export, restrictions |
| S3 | https://support.claude.com/en/articles/11176164 | Connectors overview, directory and custom connectors, desktop extensions, admin control |
| S4 | https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview | Where skills work, no cross-surface sync, runtime constraints per surface |
| S5 | https://support.claude.com/en/articles/12512180 | Skills on claude.ai: plans, code execution required, upload, sync to Claude Code |
| S6 | https://code.claude.com/docs/en/skills | Skill locations, frontmatter (`allowed-tools`, `context: fork`) |
| S7 | https://code.claude.com/docs/en/commands | Bundled `/dataviz`, `/artifact-diagramming`, `/deep-research` |
| S8 | https://code.claude.com/docs/en/sub-agents | Built-in subagents, background runs, nesting |
| S9 | https://code.claude.com/docs/en/artifacts | Artifacts: availability and fallback; Claude Docs connector in Claude Code |
| S10 | https://code.claude.com/docs/en/agent-sdk/subagents | Agent SDK subagents and parallel runs |
| S11 | https://code.claude.com/docs/en/mcp | claude.ai connectors in Claude Code; Claude Docs connector; tool search |
| S12 | https://support.claude.com/en/articles/12111783 | Code execution and file creation: plans, surfaces, PNG charts |
| S13 | https://support.claude.com/en/articles/10684626 | Web search and fetch on claude.ai |
| S14 | https://support.claude.com/en/articles/11088861 | Research mode: paid plans, web search required, connectors |
| S15 | https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool | API web search tool |
| S16 | https://github.com/anthropics/skills/tree/main/skills | Anthropic's public skills list (no `research`, `dataviz`, `archify`) |
| S17 | https://github.com/anthropics/claude-plugins-official/blob/main/.claude-plugin/marketplace.json, https://github.com/anthropics/knowledge-work-plugins/blob/main/.claude-plugin/marketplace.json, https://github.com/anthropics/claude-plugins-community/blob/main/.claude-plugin/marketplace.json | Marketplace listings searched by name on 2026-10-25 (315, 123 and 2,283 entries) |
| S18 | https://github.com/tt-a1i/archify | Third-party `archify` skill, Apache-2.0 |
| S19 | https://github.com/search?q=%22review-panel%22+filename%3Aplugin.json&type=code | Unrelated third-party plugins using the name `review-panel` |
| S20 | https://claude.com/docs/plugins/platform-support.md, https://claude.com/docs/plugins/overview.md | Which plugin components load in chat, Cowork and Claude Code |
| S21 | https://claude.com/connectors | Connector directory: Slack, Gmail, Google Drive, Google Calendar, Atlassian MCP, all Anthropic verified |
| S22 | https://claude.com/docs/connectors/slack.md | Slack connector: Team and Enterprise, app install, Owner enablement |
| S23 | https://platform.claude.com/docs/en/agents-and-tools/mcp-connector | MCP connector on the Messages API (beta) |
| S24 | https://support.claude.com/en/articles/10166901 | Google Workspace connectors: Gmail, Calendar, Drive, Docs, Sheets, Slides |
| S25 | https://www.atlassian.com/blog/announcements/atlassian-rovo-mcp-ga | Atlassian Rovo MCP server generally available, Claude supported |
| S26 | https://code.claude.com/docs/en/memory | Claude Code auto memory |
| S27 | https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-diagrams | GitHub renders Mermaid code blocks |
| S28 | https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices | Fully qualified MCP tool names; do not assume packages |
| S29 | https://code.claude.com/docs/en/plugins-reference | Plugin `dependencies` field |
| S30 | https://support.claude.com/en/articles/13345190 | Cowork plans |
