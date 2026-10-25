# Public formats and channels for the decision-brief skill

Research for issue #2, "Which public formats can carry the skill?". Checked on 2026-10-25 against Anthropic's own documentation, the Agent Skills specification and Anthropic's public GitHub repositories. Every claim carries a source link, in the text or in the table. Anything marked **unconfirmed** could not be found in a primary source.

## Summary

- **One skill folder can serve every channel.** The open Agent Skills format (a folder named after the skill, holding `SKILL.md` with `name` and `description` frontmatter, plus optional `references/`, `assets/` and `scripts/`) is what Claude Code, claude.ai, Cowork, the Claude API and Claude Managed Agents all read. The stricter channels allow only the six spec fields in the frontmatter (`name`, `description`, `license`, `compatibility`, `metadata`, `allowed-tools`). Any other key makes upload fail, so the shared skill should use only those six.
- **Hard limits that apply across channels:** `name` up to 64 characters, lowercase letters, digits and single hyphens only, the same as the folder name, and without the words "anthropic" or "claude". `description` up to 1,024 characters, not empty, with no XML tags or angle brackets. `decision-brief` passes all of these.
- **A Claude Code plugin marketplace in this repository is now also a claude.ai and Cowork channel.** Users can add a GitHub repository as a marketplace under **Customize > Plugins** on claude.ai and in Cowork, as well as with `/plugin marketplace add` in Claude Code. One `.claude-plugin/marketplace.json` that lists one plugin holding `skills/decision-brief/` therefore reaches all three surfaces. The same skill folder can also be zipped by hand for a claude.ai skill upload or for the Skills API.
- **Updates differ by channel.** Claude Code marketplaces you run yourself have auto-update off by default, so users update by hand unless they or an admin turn it on. claude.ai marketplaces update with "Check for updates" or automatic sync. Zip uploads update only when someone uploads again. API callers get the new version when they use `latest`.
- **Organisation sync on claude.ai will not read a public repository.** Org-wide sync from **Organization settings** needs a private or internal repository on github.com or gitlab.com. An organisation that wants to push the public core to its members has to mirror it, upload a zip, or let members add the public marketplace themselves.
- **Private per-organisation "profile" files cannot be placed outside the skill folder and picked up by path on every surface.** Claude Code can read them from the project directory, from a plugin's persistent data folder, or from a separate private plugin that depends on the public one. On claude.ai the documented options are a second, private skill or plugin, project knowledge or uploaded files. Through the API they can be a second custom skill in the same request, or files uploaded with the Files API.

## Comparison table

| | Agent Skills folder (open spec) | Claude Code plugin marketplace (this repo) | claude.ai / Cowork custom skill (zip) | claude.ai / Cowork plugin (marketplace or zip) | Claude API Skills endpoint (also Managed Agents) |
| :- | :- | :- | :- | :- | :- |
| **Layout** | `decision-brief/SKILL.md` plus optional `scripts/`, `references/`, `assets/` [spec](https://agentskills.io/specification) | `.claude-plugin/marketplace.json` at the marketplace root, plugin folder with optional `.claude-plugin/plugin.json`, skill at `<plugin>/skills/<name>/SKILL.md` [create marketplace](https://code.claude.com/docs/en/plugins/create-marketplace.md), [manifest](https://code.claude.com/docs/en/plugins/manifest-reference.md) | Zip whose top level is the skill folder: `decision-brief.zip/decision-brief/SKILL.md` [claude.com how-to](https://claude.com/docs/skills/how-to.md) | Same plugin folder as Claude Code. A zip must hold one `.claude-plugin/plugin.json`, at the root or inside one top-level folder [plugins overview](https://claude.com/docs/plugins/overview.md), [build](https://claude.com/docs/plugins/build.md) | `SKILL.md` at the upload root or at the top of one enclosing folder. Zip or separate files [skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md) |
| **Frontmatter allowed** | `name`, `description` (required), `license`, `compatibility`, `metadata`, `allowed-tools` [spec](https://agentskills.io/specification) | All Claude Code fields (for example `when_to_use`, `argument-hint`, `disable-model-invocation`, `context`, `hooks`, `paths`) [Claude Code skills](https://code.claude.com/docs/en/skills.md) | Only the six spec fields. Other keys cause a hard error [Claude Code skills](https://code.claude.com/docs/en/skills.md) | Unconfirmed for plugin skills on claude.ai. The six spec fields are safe | Only the six spec fields [Claude Code skills](https://code.claude.com/docs/en/skills.md) |
| **`name`** | 1–64 characters, `a-z`, `0-9`, hyphen. No leading, trailing or double hyphen. Must match the folder name [spec](https://agentskills.io/specification) | Optional. Defaults to the folder name. Invoked as `/<plugin>:<name>` [Claude Code skills](https://code.claude.com/docs/en/skills.md) | Up to 64 characters, lowercase, digits, hyphens. Matches the folder [claude.com how-to](https://claude.com/docs/skills/how-to.md). No "anthropic" or "claude" [overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview.md) | As for skills. Plugin `name`: kebab-case, must not start with `claude-` or `anthropic-` [manifest](https://code.claude.com/docs/en/plugins/manifest-reference.md) | Up to 64 characters, lowercase, digits, hyphens, no XML tags, no "anthropic" or "claude" [skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md) |
| **`description`** | 1–1,024 characters [spec](https://agentskills.io/specification) | `description` and `when_to_use` together are cut to 1,536 characters in the listing [Claude Code skills](https://code.claude.com/docs/en/skills.md) | Up to 1,024 characters [claude.com how-to](https://claude.com/docs/skills/how-to.md). An older Help Center article says 200 (see Conflicts) | As for skills | Up to 1,024 characters, not empty, no XML tags [skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md) |
| **Size and count limits** | None in the spec. Recommends a `SKILL.md` under 500 lines and under 5,000 tokens [spec](https://agentskills.io/specification) | Archive sources: 256 MiB download, 100,000 entries, 1 GiB unpacked. Hosted `marketplace.json`: 5 MiB [host marketplace](https://code.claude.com/docs/en/plugins/host-marketplace.md). No account limits [platform support](https://claude.com/docs/plugins/platform-support.md) | Unconfirmed for a skill-only zip | 5,000 files and 200 MB per plugin. Upload up to 200 MB. Up to 25 marketplaces you add yourself [platform support](https://claude.com/docs/plugins/platform-support.md) | Under 30 MB uncompressed. Up to 20 skills per request [skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md) |
| **How users install** | Depends on the client | `/plugin marketplace add <owner>/<repo>`, then `/plugin install <plugin>@<marketplace>` [create marketplace](https://code.claude.com/docs/en/plugins/create-marketplace.md) | **Customize > Skills > + > Create skill > Upload a skill** [Help Center](https://support.claude.com/en/articles/12512180-using-skills-in-claude). Owners can provision org-wide [Help Center](https://support.claude.com/en/articles/13119606-provisioning-and-managing-skills-for-your-organization) | **Customize > Plugins > Add > Add marketplace** (repository URL) or **Upload plugin** [plugins overview](https://claude.com/docs/plugins/overview.md) | `POST /v1/skills`, then reference `skill_id` in `container.skills` with the code execution tool [skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md) |
| **Updates** | Depends on the client | Auto-update is off by default for third-party marketplaces. Otherwise users run `/plugin marketplace update` or `claude plugin update`. A pinned `version` holds users until it changes [install](https://code.claude.com/docs/en/plugins/install.md), [host marketplace](https://code.claude.com/docs/en/plugins/host-marketplace.md) | Re-upload. Admins use **Upload new version** [admin](https://claude.com/docs/plugins/admin.md). Shared skills update for recipients at next use [Help Center](https://support.claude.com/en/articles/12512180-using-skills-in-claude) | From the source after sync, with nothing to accept. **Check for updates** or **Sync automatically** for github.com [plugins overview](https://claude.com/docs/plugins/overview.md) | New version per upload (full snapshot). `version: "latest"` or a pinned `skver_…` ID [skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md) |
| **Sharing scope** | – | Anyone who can reach the repository | Personal. On Team and Enterprise: shared or published to the org [Help Center](https://support.claude.com/en/articles/12512180-using-skills-in-claude) | Account in each organisation. Org Owners set availability [admin](https://claude.com/docs/plugins/admin.md) | Whole workspace [skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md) |

## Details per channel

### 1. Open Agent Skills format

- **Layout:** a skill is a folder that contains at least `SKILL.md`. The optional folders are `scripts/`, `references/` and `assets/`, and any other files are allowed ([spec](https://agentskills.io/specification)).
- **Frontmatter:** YAML, followed by Markdown. The fields are listed in the table. `compatibility` takes up to 500 characters. `metadata` is a map from strings to strings. `allowed-tools` is experimental ([spec](https://agentskills.io/specification)).
- **Name rules:** 1–64 characters. Only unicode lowercase alphanumerics and hyphens. No hyphen at the start or end and no `--`. Must match the parent folder name ([spec](https://agentskills.io/specification)).
- **Progressive loading:** about 100 tokens of metadata are loaded for every skill. The body should stay under 5,000 tokens, and the spec recommends keeping `SKILL.md` under 500 lines. Files in `scripts/`, `references/` and `assets/` load only when needed ([spec](https://agentskills.io/specification)).
- **Referencing supporting files:** use relative paths from the skill root, such as `references/REFERENCE.md`, and keep references one level deep ([spec](https://agentskills.io/specification), [best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices.md)).
- **Validation:** `skills-ref validate ./my-skill` ([spec](https://agentskills.io/specification)). Anthropic's `quick_validate.py` checks the same six allowed keys and the 64, 1,024 and 500 character limits, and it rejects `<` and `>` in the description ([anthropics/skills quick_validate.py](https://github.com/anthropics/skills/blob/main/skills/skill-creator/scripts/quick_validate.py)).
- **Install and updates:** the spec defines neither. Each client decides.
- claude.ai says skills follow this open standard, so the same skill works in other tools that adopt it ([claude.com skills overview](https://claude.com/docs/skills/overview.md)).

### 2. Claude Code plugin marketplace in this repository

- **Marketplace file:** `.claude-plugin/marketplace.json` requires `name`, `owner` and a `plugins` array. Each entry needs `name` and `source` ([create marketplace](https://code.claude.com/docs/en/plugins/create-marketplace.md)).
  - A relative `source` resolves from the marketplace root (the folder that contains `.claude-plugin/`), must start with `./` and must not contain `..`. `"."` on its own means the root itself ([marketplace reference](https://code.claude.com/docs/en/plugins/marketplace-reference.md)).
  - The entry `name` should equal the plugin's `plugin.json` `name` ([create marketplace](https://code.claude.com/docs/en/plugins/create-marketplace.md)).
- **Marketplace name:** letters, digits, `.`, `_` and `-`, starting with a letter or digit, and no `..` ([marketplace reference](https://code.claude.com/docs/en/plugins/marketplace-reference.md)). Reserved names include:
  - the official names, such as `claude-plugins-official` and `agent-skills`
  - names that impersonate an official marketplace
  - names starting with `claudeai-`
  - `npm`, `github` and similar
  - `org`, `org-provisioned` and `unknown`, which Claude Desktop rejects ([marketplace reference](https://code.claude.com/docs/en/plugins/marketplace-reference.md)).
- **Plugin manifest:** `.claude-plugin/plugin.json` is optional, and `name` is its only required key. All other plugin files sit at the plugin root, not inside `.claude-plugin/` ([manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference.md)).
  - Plugin `name` rules: no spaces, `@`, `:` or path separators.
  - Names that start with `claude-`, `anthropic-`, `anthropics-` or `cc-plugin-` fail validation.
  - Having `claude` or `anthropic` as a whole word anywhere else gives a warning ([manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference.md)).
- **Skills inside a plugin:** by default the plugin loads `skills/<name>/SKILL.md`. A `skills` key adds more folders ([manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference.md)). Skills are invoked as `/plugin-name:skill-name` ([Claude Code skills](https://code.claude.com/docs/en/skills.md)).
  - Anthropic's own skills repository lists skill folders through plugin entries with `"source": "./"`, `"strict": false` and a `skills` array ([anthropics/skills marketplace.json](https://github.com/anthropics/skills/blob/main/.claude-plugin/marketplace.json)).
  - The Claude Code repository uses one subfolder per plugin under `plugins/` ([anthropics/claude-code marketplace.json](https://github.com/anthropics/claude-code/blob/main/.claude-plugin/marketplace.json)).
- **Frontmatter in Claude Code:** every Claude Code field is accepted, and unknown keys are ignored without an error ([Claude Code skills](https://code.claude.com/docs/en/skills.md)). Do not use the extra fields in a skill that must also upload to claude.ai or the API (see channel 3).
- **Path variables:**
  - `${CLAUDE_SKILL_DIR}` is the skill's folder.
  - `${CLAUDE_PLUGIN_ROOT}` is the installed plugin, which changes on each update.
  - `${CLAUDE_PLUGIN_DATA}` is a persistent folder that survives updates.
  - `${CLAUDE_PROJECT_DIR}` is the project root.
  
  All four are replaced in the skill body ([Claude Code skills](https://code.claude.com/docs/en/skills.md), [manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference.md)).
- **Install:** users run `/plugin marketplace add <owner>/<repo>` and then `/plugin install <plugin>@<marketplace>`. A repository can also register the marketplace for everyone who opens it, with `--scope project` and a committed `.claude/settings.json` ([create marketplace](https://code.claude.com/docs/en/plugins/create-marketplace.md), [host marketplace](https://code.claude.com/docs/en/plugins/host-marketplace.md)).
- **Updates:** Claude Code detects an update from a computed version. The order is the `plugin.json` `version`, then the entry `version`, then the git commit SHA ([loading reference](https://code.claude.com/docs/en/plugins/loading.md)).
  - If you pin `version`, users get nothing new until you change it. If you leave it out, users follow your commits ([host marketplace](https://code.claude.com/docs/en/plugins/host-marketplace.md)).
  - Auto-update is off by default for third-party marketplaces. A user or an admin turns it on ([install](https://code.claude.com/docs/en/plugins/install.md), [host marketplace](https://code.claude.com/docs/en/plugins/host-marketplace.md)).
- **Containment:**
  - Component paths must resolve inside the plugin root ([manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference.md)).
  - When a plugin is copied into the cache, a symlink to elsewhere in the same marketplace is copied in as content. A symlink that points outside the marketplace is skipped ([host marketplace](https://code.claude.com/docs/en/plugins/host-marketplace.md)).
- **Anthropic directory (optional):** listing needs a paid plan and a GitHub repository, which must be public before the listing goes live ([directory publish](https://claude.com/docs/directory/publish.md), [submit](https://claude.com/docs/plugins/submit.md)).
  - The plugin also needs a README of at least 40 words and a licence ([build](https://claude.com/docs/plugins/build.md)).
  - A folder without `plugin.json` is listed for Claude Code only ([build](https://claude.com/docs/plugins/build.md)).
- **Agent SDK:** skills load from the filesystem through `settingSources`, or from a path given with the `plugins` option. There is no API for registering skills in code ([Agent SDK skills](https://code.claude.com/docs/en/agent-sdk/skills.md)).

### 3. Custom skill uploaded to claude.ai (also Cowork and Desktop chat)

- **Plans and prerequisites:** "Code execution and file creation" must be on ([claude.com skills overview](https://claude.com/docs/skills/overview.md)).
  - claude.com lists Pro, Max, Team and Enterprise ([claude.com skills overview](https://claude.com/docs/skills/overview.md)).
  - The Help Center also lists Free ([Help Center: what are skills](https://support.claude.com/en/articles/12512176-what-are-skills)).
- **Package:** a zip whose top level is the skill folder. A `SKILL.md` at the root of the zip is not recognised. Build it with `zip -r my-skill.zip my-skill/` ([claude.com how-to](https://claude.com/docs/skills/how-to.md)).
- **Frontmatter:** only the six spec fields. Any other key fails with "Unexpected key(s) in SKILL.md frontmatter" ([Claude Code skills](https://code.claude.com/docs/en/skills.md)). Field limits are in the table.
- **Supporting files:** in chat, the whole skill folder, scripts included, is copied into the code execution sandbox. Reference files relative to `SKILL.md`, such as `scripts/render.py`. Claude Code and Cowork also replace `${CLAUDE_SKILL_DIR}` ([claude.com how-to](https://claude.com/docs/skills/how-to.md)).
- **Install:** **Customize > Skills > + > Create skill > Upload a skill** ([Help Center: use skills](https://support.claude.com/en/articles/12512180-using-skills-in-claude)).
- **Sharing on Team and Enterprise:**
  - Members can share a skill with people or groups, or publish it to the organisation library, which may require review ([Help Center: use skills](https://support.claude.com/en/articles/12512180-using-skills-in-claude)).
  - Owners can provision a skill zip for everyone under **Organization settings > Plugins & skills > Add > Upload a skill** ([Help Center: provision](https://support.claude.com/en/articles/13119606-provisioning-and-managing-skills-for-your-organization)).
  - Provisioned skills also sync to Claude Code for users signed in with that account ([Help Center: provision](https://support.claude.com/en/articles/13119606-provisioning-and-managing-skills-for-your-organization), [Claude Code skills](https://code.claude.com/docs/en/skills.md)).
- **Updates:**
  - Shared skills reach recipients "at next use" ([Help Center: use skills](https://support.claude.com/en/articles/12512180-using-skills-in-claude)).
  - A skill published to the organisation updates when it is published again, through the same review ([Help Center: use skills](https://support.claude.com/en/articles/12512180-using-skills-in-claude)).
  - Org-uploaded items have **Upload new version** and a version history with revert ([admin](https://claude.com/docs/plugins/admin.md)).
  - How a member replaces their own personal upload is **unconfirmed**.
- **Size limit for a skill-only zip:** **unconfirmed**. The 200 MB and 5,000-file limits are documented for plugins ([platform support](https://claude.com/docs/plugins/platform-support.md)).
- **Claude Code sync:** claude.ai skills sync to `~/.claude/skills/synced/` and run as `/anthropic-skills:<name>` ([Claude Code skills](https://code.claude.com/docs/en/skills.md)).
  - On the user's own computer, a synced skill does not run `!` commands or attach `@` files, and `${CLAUDE_PROJECT_DIR}` is not replaced ([Claude Code skills](https://code.claude.com/docs/en/skills.md)).
- **Microsoft 365 add-ins:** skills enabled in Claude settings are also available in the Claude for Excel, PowerPoint, Word and Outlook add-ins ([Help Center: use skills](https://support.claude.com/en/articles/12512180-using-skills-in-claude)).

### 4. Plugin on claude.ai and in Cowork

- **Adding a plugin:** on the **Plugins** page, users add a plugin from **Discover**, **Add > Add marketplace** or **Add > Upload plugin** ([plugins overview](https://claude.com/docs/plugins/overview.md)).
  - **Add marketplace** accepts a GitHub repository, as a URL or `owner/repo`. It also accepts GitHub Enterprise repositories and public GitLab and Bitbucket repositories ([plugins overview](https://claude.com/docs/plugins/overview.md), [Cowork plugins](https://claude.com/docs/cowork/guide/plugins.md)).
- **One install, three surfaces:** an install is saved to the account. Its skills are available in chat and Cowork, and they reach Claude Code as a synced plugin ([plugins overview](https://claude.com/docs/plugins/overview.md), [platform support](https://claude.com/docs/plugins/platform-support.md)).
- **What loads where:**
  - Skills load in chat, Cowork and Claude Code.
  - Agents and hooks are ignored in chat.
  - Claude Code-only parts, such as LSP servers and output styles, are ignored elsewhere.
  - A top-level `bin/` folder makes claude.ai and Cowork refuse the whole plugin ([platform support](https://claude.com/docs/plugins/platform-support.md)).
- **Updates:** they arrive automatically after a sync from the source. Users can also select **Check for updates** or turn on **Sync automatically** for github.com marketplaces ([plugins overview](https://claude.com/docs/plugins/overview.md)).
  - For a marketplace hosted on claude.ai, claude.ai records the version, and the manifest `version` is not read ([loading reference](https://code.claude.com/docs/en/plugins/loading.md)).
- **Organisation distribution:** Owners on Team and Enterprise can sync a marketplace repository and set each plugin to **Not available**, **Available to install**, **Installed by default** or **Required** ([admin](https://claude.com/docs/plugins/admin.md)).
  - On github.com and gitlab.com the synced repository must be private or internal ([org sync](https://claude.com/docs/plugins/org-sync.md)).
  - Plugin sources must be `github`, `url`, `git-subdir` or a `./` relative path ([org sync](https://claude.com/docs/plugins/org-sync.md)).
  - Enterprise can limit a plugin, and so the skills inside it, to user groups ([Help Center: provision](https://support.claude.com/en/articles/13119606-provisioning-and-managing-skills-for-your-organization)).
- **Entries without `plugin.json`:** whether claude.ai **Add marketplace** accepts entries with `"strict": false` and no `plugin.json`, in the style of anthropics/skills, is **unconfirmed**. Anthropic's directory lists such a folder for Claude Code only ([build](https://claude.com/docs/plugins/build.md)). A real `plugin.json` is the safe choice.

### 5. Claude API Skills endpoint, Managed Agents and cloud platforms

- **Upload:** send a zip or separate files to `POST /v1/skills`.
  - `SKILL.md` must sit at the upload root or at the top of one enclosing folder.
  - The total must be under 30 MB uncompressed.
  - `display_name` is optional, up to 255 characters ([skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md)).
- **Status:** the Skills API is out of beta and needs no beta header ([skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md)).
- **Use:** pass `container.skills` with `type: "custom"`, the `skill_id` and an optional `version`, together with the code execution tool. Up to 20 skills per request ([skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md)).
- **Versions:** each version is a full snapshot, and its `name` must match the existing skill. Requests use `latest` or a pinned `skver_…` ID ([skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md)).
- **Scope:** a custom skill is visible to the whole workspace. Use one workspace per tenant for isolation ([skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md)).
- **Runtime:** no network access and no installing packages at runtime ([overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview.md)).
- **Input files:** files for a skill to work on are uploaded with the Files API and passed in a container upload block ([skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md)).
- **Claude Managed Agents** (beta) attach skills uploaded the same way ([Managed Agents skills](https://platform.claude.com/docs/en/managed-agents/skills.md)).
- **Claude Platform on AWS and Microsoft Foundry** use the Skills API too. On Foundry, the version download endpoint is not supported ([overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview.md)).

### Other first-party channels found

- **Claude Code without a marketplace:**
  - personal `~/.claude/skills/`
  - project `.claude/skills/`, committed to a repository
  - enterprise managed-settings folder
  - `--add-dir` folders
  - a one-session plugin with `--plugin-dir` ([Claude Code skills](https://code.claude.com/docs/en/skills.md), [create marketplace](https://code.claude.com/docs/en/plugins/create-marketplace.md))
- **Claude Desktop:** the **Code** tab is Claude Code, with the same `/plugin` flow. Chat and Cowork read the claude.ai account ([install](https://code.claude.com/docs/en/plugins/install.md), [platform support](https://claude.com/docs/plugins/platform-support.md)).
- **Cowork and cloud sessions** do not read `~/.claude/skills/` on the user's computer. They load the skills enabled for the claude.ai account ([Claude Code skills](https://code.claude.com/docs/en/skills.md)).
- **Desktop extensions (`.mcpb`)** package local MCP servers, not skills ([claude.com docs index](https://claude.com/docs/llms.txt)). They are not a carrier for this skill.

### Conflicts between sources

1. **Org-wide skills on claude.ai.** The platform overview says claude.ai custom skills are individual only and cannot be managed centrally ([overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview.md)). The current Help Center and claude.com pages document Owner provisioning, sharing and publishing on Team and Enterprise ([Help Center: provision](https://support.claude.com/en/articles/13119606-provisioning-and-managing-skills-for-your-organization), [admin](https://claude.com/docs/plugins/admin.md)). The newer product pages are taken as current.
2. **Description length on claude.ai.** The Help Center article on creating skills says `description` is limited to 200 characters and shows a `name` with capitals and spaces ([Help Center: create](https://support.claude.com/en/articles/12512198-creating-custom-skills)). The claude.com guide, the API and the spec say 1,024 characters and lowercase with hyphens ([claude.com how-to](https://claude.com/docs/skills/how-to.md)). Which limit claude.ai enforces today is **unconfirmed**. A lowercase-hyphen `name` meets both texts. A description of up to 200 characters would also meet both.
3. **Zip shape.** claude.ai needs the skill folder as the top level of the zip ([claude.com how-to](https://claude.com/docs/skills/how-to.md)). The API accepts either shape ([skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md)). Zipping the folder itself works for both.

## Implications for one repository serving several channels

**One layout can serve every channel.** The pattern follows Anthropic's own repositories ([anthropics/claude-code marketplace.json](https://github.com/anthropics/claude-code/blob/main/.claude-plugin/marketplace.json), [anthropics/skills marketplace.json](https://github.com/anthropics/skills/blob/main/.claude-plugin/marketplace.json)):

```text
.claude-plugin/marketplace.json          # marketplace root = repository root
plugins/decision-brief/
├── .claude-plugin/plugin.json           # name "decision-brief", version, description, license
├── README.md                            # 40+ words if it should ever go to the directory
└── skills/decision-brief/               # the portable Agent Skills folder
    ├── SKILL.md                         # only the six spec frontmatter fields
    └── references/…                     # relative links, one level deep
```

- **Claude Code:** `/plugin marketplace add <owner>/decision-brief`, then `/plugin install decision-brief@<marketplace-name>` ([create marketplace](https://code.claude.com/docs/en/plugins/create-marketplace.md)).
- **claude.ai and Cowork, per user:** **Customize > Plugins > Add > Add marketplace** with the same repository URL. From there it also reaches Claude Code as a synced plugin ([plugins overview](https://claude.com/docs/plugins/overview.md)).
- **claude.ai skill upload or provisioning:** run `zip -r decision-brief.zip decision-brief/` from `plugins/decision-brief/skills/`. A release workflow could attach this zip to each GitHub release ([claude.com how-to](https://claude.com/docs/skills/how-to.md)).
- **Claude API:** upload the same zip, or run `ant apply` on the folder ([skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md)).
- **Other Agent Skills clients:** copy the folder ([spec](https://agentskills.io/specification)).

**Rules that keep every channel working:**

- Use only `name`, `description`, `license`, `compatibility`, `metadata` and `allowed-tools` in `SKILL.md`. Claude Code-only fields would break the zip and API uploads ([Claude Code skills](https://code.claude.com/docs/en/skills.md)). Put Claude Code-only behaviour in the plugin, not in the shared `SKILL.md`.
- Keep the folder name and `name` both `decision-brief` ([spec](https://agentskills.io/specification)).
- Reference supporting files with paths relative to `SKILL.md`, which work in the chat sandbox. `${CLAUDE_SKILL_DIR}` is only a Claude Code and Cowork convenience ([claude.com how-to](https://claude.com/docs/skills/how-to.md)).
- Do not depend on `!` commands, network access or packages installed at runtime. These do not work on claude.ai chat, on the API, or for skills synced to the user's computer ([Claude Code skills](https://code.claude.com/docs/en/skills.md), [overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview.md)).
- Do not add a top-level `bin/` to the plugin ([platform support](https://claude.com/docs/plugins/platform-support.md)).
- Choose a versioning policy:
  - Bump `version` in `plugin.json` on every release. Set it in `plugin.json` only, not also in the marketplace entry.
  - Or leave `version` out so users follow commits ([host marketplace](https://code.claude.com/docs/en/plugins/host-marketplace.md)).
- Tell users that auto-update has to be turned on for a third-party marketplace in Claude Code ([install](https://code.claude.com/docs/en/plugins/install.md)).
- Avoid `claude-` or `anthropic-` prefixes in the plugin and marketplace names, and the reserved marketplace names ([manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference.md), [marketplace reference](https://code.claude.com/docs/en/plugins/marketplace-reference.md)).
- A public repository cannot be synced through claude.ai **Organization settings**, which needs private or internal ([org sync](https://claude.com/docs/plugins/org-sync.md)). Organisations either:
  - mirror the repository privately,
  - upload the plugin or skill zip, or
  - let members add the public marketplace themselves, if the org policy allows it ([admin](https://claude.com/docs/plugins/admin.md)).

### Private "profile" files next to a public core

What the sources say:

- **Inside the skill folder** (relative paths) works on every channel ([spec](https://agentskills.io/specification)). That rules out keeping private data there in a public repository.
- **Outside the plugin folder by path:** plugin component paths must stay inside the plugin root. A symlink that points outside the marketplace is skipped when the plugin is cached ([manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference.md), [host marketplace](https://code.claude.com/docs/en/plugins/host-marketplace.md)). A profile cannot be "plugged in" by placing it next to the installed plugin.
- **Claude Code options:**
  - The skill body can name a file under `${CLAUDE_PROJECT_DIR}` or `${CLAUDE_PLUGIN_DATA}`. Claude then reads it with its file tools, subject to the normal permission rules ([Claude Code skills](https://code.claude.com/docs/en/skills.md), [manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference.md), [permissions](https://code.claude.com/docs/en/permissions.md)).
  - A private profile plugin in a private marketplace can declare the public core as a dependency across marketplaces. This needs `allowCrossMarketplaceDependenciesOn` in the private marketplace's `marketplace.json` ([dependencies](https://code.claude.com/docs/en/plugins/dependencies.md)). Plugin dependencies are Claude Code only ([build](https://claude.com/docs/plugins/build.md)).
  - `userConfig` prompts work in Claude Code only ([platform support](https://claude.com/docs/plugins/platform-support.md)).
- **claude.ai chat and Cowork options:** no documented way for a skill to load files from outside its own folder was found (**unconfirmed**).
  - The Help Center says skills "can't explicitly reference other skills", but Claude can use several skills together ([Help Center: create](https://support.claude.com/en/articles/12512198-creating-custom-skills)). A private profile skill or plugin, provisioned or shared inside the organisation, would sit beside the public core, but the core cannot call it directly.
  - Other documented places for organisation context:
    - projects, which hold always-loaded knowledge ([claude.com skills overview](https://claude.com/docs/skills/overview.md))
    - Cowork projects, with local folders and standing instructions ([Cowork projects](https://claude.com/docs/cowork/guide/projects.md))
  - How a skill would find a file the user attached is not documented (**unconfirmed**).
- **API options:**
  - attach a private profile skill from the organisation's own workspace together with the public core in one request (up to 20 skills), or
  - upload profile files with the Files API as container inputs ([skills guide](https://platform.claude.com/docs/en/build-with-claude/skills-guide.md)).

Design consequence: the core `SKILL.md` should treat the profile as optional and describe where to look in order. The order would be:

1. a profile skill or plugin, if one is loaded
2. a file in the project or working folder
3. files the user attached
4. a fallback, which is to ask the user

It should not assume one fixed path.

## Sources

- Agent Skills specification – https://agentskills.io/specification
- Agent Skills documentation index – https://agentskills.io/llms.txt
- Agent Skills overview (Claude Platform docs) – https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview
- Skill authoring best practices – https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices
- Using Agent Skills with the API – https://platform.claude.com/docs/en/build-with-claude/skills-guide
- Claude Managed Agents skills – https://platform.claude.com/docs/en/managed-agents/skills
- Claude Code: Extend Claude with skills – https://code.claude.com/docs/en/skills
- Claude Code: Agent SDK skills – https://code.claude.com/docs/en/agent-sdk/skills
- Claude Code: Permissions – https://code.claude.com/docs/en/permissions
- Claude Code: Create a marketplace – https://code.claude.com/docs/en/plugins/create-marketplace
- Claude Code: Host and maintain a marketplace – https://code.claude.com/docs/en/plugins/host-marketplace
- Claude Code: Marketplace reference – https://code.claude.com/docs/en/plugins/marketplace-reference
- Claude Code: Plugin manifest reference – https://code.claude.com/docs/en/plugins/manifest-reference
- Claude Code: Plugin loading reference – https://code.claude.com/docs/en/plugins/loading
- Claude Code: Install and manage plugins – https://code.claude.com/docs/en/plugins/install
- Claude Code: Plugin dependencies – https://code.claude.com/docs/en/plugins/dependencies
- claude.com: Skills overview – https://claude.com/docs/skills/overview
- claude.com: Create custom skills – https://claude.com/docs/skills/how-to
- claude.com: Plugins – https://claude.com/docs/plugins/overview
- claude.com: Plugin structure and testing – https://claude.com/docs/plugins/build
- claude.com: Plugin feature support across platforms – https://claude.com/docs/plugins/platform-support
- claude.com: Manage plugins for your organization – https://claude.com/docs/plugins/admin
- claude.com: Sync your organization's plugins from a repository – https://claude.com/docs/plugins/org-sync
- claude.com: Install plugins (Cowork) – https://claude.com/docs/cowork/guide/plugins
- claude.com: Organize work with projects (Cowork) – https://claude.com/docs/cowork/guide/projects
- claude.com: Publish to the directory – https://claude.com/docs/directory/publish
- claude.com: Submit your plugin – https://claude.com/docs/plugins/submit
- claude.com documentation index – https://claude.com/docs/llms.txt
- Help Center: What are skills? – https://support.claude.com/en/articles/12512176-what-are-skills
- Help Center: Use skills in Claude – https://support.claude.com/en/articles/12512180-using-skills-in-claude
- Help Center: How to create custom skills – https://support.claude.com/en/articles/12512198-creating-custom-skills
- Help Center: Provision and manage skills for your organization – https://support.claude.com/en/articles/13119606-provisioning-and-managing-skills-for-your-organization
- anthropics/skills marketplace.json – https://github.com/anthropics/skills/blob/main/.claude-plugin/marketplace.json
- anthropics/skills quick_validate.py – https://github.com/anthropics/skills/blob/main/skills/skill-creator/scripts/quick_validate.py
- anthropics/claude-code marketplace.json – https://github.com/anthropics/claude-code/blob/main/.claude-plugin/marketplace.json
