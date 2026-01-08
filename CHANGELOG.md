# Changelog

## 2.1.0

- Added automatic skill hot-reload - skills created or modified in `~/.claude/skills` or `.claude/skills` are now immediately available without restarting the session
- Added support for running skills and slash commands in a forked sub-agent context using `context: fork` in skill frontmatter
- Added support for `agent` field in skills to specify agent type for execution
- Added `language` setting to configure Claude's response language (e.g., language: "japanese")
- Changed Shift+Enter to work out of the box in iTerm2, WezTerm, Ghostty, and Kitty without modifying terminal configs
- Added `respectGitignore` support in `settings.json` for per-project control over @-mention file picker behavior
- Added `IS_DEMO` environment variable to hide email and organization from the UI, useful for streaming or recording sessions
- Fixed security issue where sensitive data (OAuth tokens, API keys, passwords) could be exposed in debug logs
- Fixed files and skills not being properly discovered when resuming sessions with `-c` or `--resume`
- Fixed pasted content being lost when replaying prompts from history using up arrow or Ctrl+R search
- Fixed Esc key with queued prompts to only move them to input without canceling the running task
- Reduced permission prompts for complex bash commands
- Fixed command search to prioritize exact and prefix matches on command names over fuzzy matches in descriptions
- Fixed PreToolUse hooks to allow `updatedInput` when returning `ask` permission decision, enabling hooks to act as middleware while still requesting user consent
- Fixed plugin path resolution for file-based marketplace sources
- Fixed LSP tool being incorrectly enabled when no LSP servers were configured
- Fixed background tasks failing with "git repository not found" error for repositories with dots in their names
- Fixed Claude in Chrome support for WSL environments
- Fixed Windows native installer silently failing when executable creation fails
- Improved CLI help output to display options and subcommands in alphabetical order for easier navigation
- Added wildcard pattern matching for Bash tool permissions using `*` at any position in rules (e.g., `Bash(npm *)`, `Bash(* install)`, `Bash(git * main)`)
- Added unified Ctrl+B backgrounding for both bash commands and agents - pressing Ctrl+B now backgrounds all running foreground tasks simultaneously
- Added support for MCP `list_changed` notifications, allowing MCP servers to dynamically update their available tools, prompts, and resources without requiring reconnection
- Added `/teleport` and `/remote-env` slash commands for claude.ai subscribers, allowing them to resume and configure remote sessions
- Added support for disabling specific agents using `Task(AgentName)` syntax in settings.json permissions or the `--disallowedTools` CLI flag
- Added hooks support to agent frontmatter, allowing agents to define PreToolUse, PostToolUse, and Stop hooks scoped to the agent's lifecycle
- Added hooks support for skill and slash command frontmatter
- Added new Vim motions: `;` and `,` to repeat f/F/t/T motions, `y` operator for yank with `yy`/`Y`, `p`/`P` for paste, text objects (`iw`, `aw`, `iW`, `aW`, `i"`, `a"`, `i'`, `a'`, `i(`, `a(`, `i[`, `a[`, `i{`, `a{`), `>>` and `<<` for indent/dedent, and `J` to join lines
- Added `/plan` command shortcut to enable plan mode directly from the prompt
- Added slash command autocomplete support when `/` appears anywhere in input, not just at the beginning
- Added `--tools` flag support in interactive mode to restrict which built-in tools Claude can use during interactive sessions
- Added `CLAUDE_CODE_FILE_READ_MAX_OUTPUT_TOKENS` environment variable to override the default file read token limit
- Added support for `once: true` config for hooks
- Added support for YAML-style lists in frontmatter `allowed-tools` field for cleaner skill declarations
- Added support for prompt and agent hook types from plugins (previously only command hooks were supported)
- Added Cmd+V support for image paste in iTerm2 (maps to Ctrl+V)
- Added left/right arrow key navigation for cycling through tabs in dialogs
- Added real-time thinking block display in Ctrl+O transcript mode
- Added filepath to full output in background bash task details dialog
- Added Skills as a separate category in the context visualization
- Fixed OAuth token refresh not triggering when server reports token expired but local expiration check disagrees
- Fixed session persistence getting stuck after transient server errors by recovering from 409 conflicts when the entry was actually stored
- Fixed session resume failures caused by orphaned tool results during concurrent tool execution
- Fixed a race condition where stale OAuth tokens could be read from the keychain cache during concurrent token refresh attempts
- Fixed AWS Bedrock subagents not inheriting EU/APAC cross-region inference model configuration, causing 403 errors when IAM permissions are scoped to specific regions
- Fixed API context overflow when background tasks produce large output by truncating to 30K chars with file path reference
- Fixed a hang when reading FIFO files by skipping symlink resolution for special file types
- Fixed terminal keyboard mode not being reset on exit in Ghostty, iTerm2, Kitty, and WezTerm
- Fixed Alt+B and Alt+F (word navigation) not working in iTerm2, Ghostty, Kitty, and WezTerm
- Fixed `${CLAUDE_PLUGIN_ROOT}` not being substituted in plugin `allowed-tools` frontmatter, which caused tools to incorrectly require approval
- Fixed files created by the Write tool using hardcoded 0o600 permissions instead of respecting the system umask
- Fixed commands with `$()` command substitution failing with parse errors
- Fixed multi-line bash commands with backslash continuations being incorrectly split and flagged for permissions
- Fixed bash command prefix extraction to correctly identify subcommands after global options (e.g., `git -C /path log` now correctly matches `Bash(git log:*)` rules)
- Fixed slash commands passed as CLI arguments (e.g., `claude /context`) not being executed properly
- Fixed pressing Enter after Tab-completing a slash command selecting a different command instead of submitting the completed one
- Fixed slash command argument hint flickering and inconsistent display when typing commands with arguments
- Fixed Claude sometimes redundantly invoking the Skill tool when running slash commands directly
- Fixed skill token estimates in `/context` to accurately reflect frontmatter-only loading
- Fixed subagents sometimes not inheriting the parent's model by default
- Fixed model picker showing incorrect selection for Bedrock/Vertex users using `--model haiku`
- Fixed duplicate Bash commands appearing in permission request option labels
- Fixed noisy output when background tasks complete - now shows clean completion message instead of raw output
- Fixed background task completion notifications to appear proactively with bullet point
- Fixed forked slash commands showing "AbortError" instead of "Interrupted" message when cancelled
- Fixed cursor disappearing after dismissing permission dialogs
- Fixed `/hooks` menu selecting wrong hook type when scrolling to a different option
- Fixed images in queued prompts showing as "[object Object]" when pressing Esc to cancel
- Fixed images being silently dropped when queueing messages while backgrounding a task
- Fixed large pasted images failing with "Image was too large" error
- Fixed extra blank lines in multiline prompts containing CJK characters (Japanese, Chinese, Korean)
- Fixed ultrathink keyword highlighting being applied to wrong characters when user prompt text wraps to multiple lines
- Fixed collapsed "Reading X files…" indicator incorrectly switching to past tense when thinking blocks appear mid-stream
- Fixed Bash read commands (like `ls` and `cat`) not being counted in collapsed read/search groups, causing groups to incorrectly show "Read 0 files"
- Fixed spinner token counter to properly accumulate tokens from subagents during execution
- Fixed memory leak in git diff parsing where sliced strings retained large parent strings
- Fixed race condition where LSP tool could return "no server available" during startup
- Fixed feedback submission hanging indefinitely when network requests timeout
- Fixed search mode in plugin discovery and log selector views exiting when pressing up arrow
- Fixed hook success message showing trailing colon when hook has no output
- Multiple optimizations to improve startup performance
- Improved terminal rendering performance when using native installer or Bun, especially for text with emoji, ANSI codes, and Unicode characters
- Improved performance when reading Jupyter notebooks with many cells
- Improved reliability for piped input like `cat refactor.md | claude`
- Improved reliability for AskQuestion tool
- Improved sed in-place edit commands to render as file edits with diff preview
- Improved Claude to automatically continue when response is cut off due to output token limit, instead of showing an error message
- Improved compaction reliability
- Improved subagents (Task tool) to continue working after permission denial, allowing them to try alternative approaches
- Improved skills to show progress while executing, displaying tool uses as they happen
- Improved skills from `/skills/` directories to be visible in the slash command menu by default (opt-out with `user-invocable: false` in frontmatter)
- Improved skill suggestions to prioritize recently and frequently used skills
- Improved spinner feedback when waiting for the first response token
- Improved token count display in spinner to include tokens from background agents
- Improved incremental output for async agents to give the main thread more control and visibility
- Improved permission prompt UX with Tab hint moved to footer, cleaner Yes/No input labels with contextual placeholders
- Improved the Claude in Chrome notification with shortened help text and persistent display until dismissed
- Improved macOS screenshot paste reliability with TIFF format support
- Improved `/stats` output
- Updated Atlassian MCP integration to use a more reliable default configuration (streamable HTTP)
- Changed "Interrupted" message color from red to grey for a less alarming appearance
- Removed permission prompt when entering plan mode - users can now enter plan mode without approval
- Removed underline styling from image reference links
- [SDK] Changed minimum zod peer dependency to ^4.0.0
- [VSCode] Added currently selected model name to the context menu
- [VSCode] Added descriptive labels on auto-accept permission button (e.g., "Yes, allow npm for this project" instead of "Yes, and don't ask again")
- [VSCode] Fixed paragraph breaks not rendering in markdown content
- [VSCode] Fixed scrolling in the extension inadvertently scrolling the parent iframe
- [Windows] Fixed issue with improper rendering

## 2.0.76

- Fixed issue with macOS code-sign warning when using Claude in Chrome integration

## 2.0.75

- Minor bugfixes

## 2.0.74

- Added LSP (Language Server Protocol) tool for code intelligence features like go-to-definition, find references, and hover documentation
- Added `/terminal-setup` support for Kitty, Alacritty, Zed, and Warp terminals
- Added ctrl+t shortcut in `/theme` to toggle syntax highlighting on/off
- Added syntax highlighting info to theme picker
- Added guidance for macOS users when Alt shortcuts fail due to terminal configuration
- Fixed skill `allowed-tools` not being applied to tools invoked by the skill
- Fixed Opus 4.5 tip incorrectly showing when user was already using Opus
- Fixed a potential crash when syntax highlighting isn't initialized correctly
- Fixed visual bug in `/plugins discover` where list selection indicator showed while search box was focused
- Fixed macOS keyboard shortcuts to display 'opt' instead of 'alt'
- Improved `/context` command visualization with grouped skills and agents by source, slash commands, and sorted token count
- [Windows] Fixed issue with improper rendering
- [VSCode] Added gift tag pictogram for year-end promotion message

## 2.0.73

- Added clickable `[Image #N]` links that open attached images in the default viewer
- Added alt-y yank-pop to cycle through kill ring history after ctrl-y yank
- Added search filtering to the plugin discover screen (type to filter by name, description, or marketplace)
- Added support for custom session IDs when forking sessions with `--session-id` combined with `--resume` or `--continue` and `--fork-session`
- Fixed slow input history cycling and race condition that could overwrite text after message submission
- Improved `/theme` command to open theme picker directly
- Improved theme picker UI
- Improved search UX across resume session, permissions, and plugins screens with a unified SearchBox component
- [VSCode] Added tab icon badges showing pending permissions (blue) and unread completions (orange)

## 2.0.72

- Added Claude in Chrome (Beta) feature that works with the Chrome extension (https://claude.ai/chrome) to let you control your browser directly from Claude Code
- Reduced terminal flickering
- Added scannable QR code to mobile app tip for quick app downloads
- Added loading indicator when resuming conversations for better feedback
- Fixed `/context` command not respecting custom system prompts in non-interactive mode
- Fixed order of consecutive Ctrl+K lines when pasting with Ctrl+Y
- Improved @ mention file suggestion speed (~3x faster in git repositories)
- Improved file suggestion performance in repos with `.ignore` or `.rgignore` files
- Improved settings validation errors to be more prominent
- Changed thinking toggle from Tab to Alt+T to avoid accidental triggers

## 2.0.71

- Added /config toggle to enable/disable prompt suggestions
- Added `/settings` as an alias for the `/config` command
- Fixed @ file reference suggestions incorrectly triggering when cursor is in the middle of a path
- Fixed MCP servers from `.mcp.json` not loading when using `--dangerously-skip-permissions`
- Fixed permission rules incorrectly rejecting valid bash commands containing shell glob patterns (e.g., `ls *.txt`, `for f in *.png`)
- Bedrock: Environment variable `ANTHROPIC_BEDROCK_BASE_URL` is now respected for token counting and inference profile listing
- New syntax highlighting engine for native build

## 2.0.70

- Added Enter key to accept and submit prompt suggestions immediately (tab still accepts for editing)
- Added wildcard syntax `mcp__server__*` for MCP tool permissions to allow or deny all tools from a server
- Added auto-update toggle for plugin marketplaces, allowing per-marketplace control over automatic updates
- Added `current_usage` field to status line input, enabling accurate context window percentage calculations
- Fixed input being cleared when processing queued commands while the user was typing
- Fixed prompt suggestions replacing typed input when pressing Tab
- Fixed diff view not updating when terminal is resized
- Improved memory usage by 3x for large conversations
- Improved resolution of stats screenshots copied to clipboard (Ctrl+S) for crisper images
- Removed # shortcut for quick memory entry (tell Claude to edit your CLAUDE.md instead)
- Fix thinking mode toggle in /config not persisting correctly
- Improve UI for file creation permission dialog

## 2.0.69

- Minor bugfixes

## 2.0.68

- Fixed IME (Input Method Editor) support for languages like Chinese, Japanese, and Korean by correctly positioning the composition window at the cursor
- Fixed a bug where disallowed MCP tools were visible to the model
- Fixed an issue where steering messages could be lost while a subagent is working
- Fixed Option+Arrow word navigation treating entire CJK (Chinese, Japanese, Korean) text sequences as a single word instead of navigating by word boundaries
- Improved plan mode exit UX: show simplified yes/no dialog when exiting with empty or missing plan instead of throwing an error
- Add support for enterprise managed settings. Contact your Anthropic account team to enable this feature.

## 2.0.67

- Thinking mode is now enabled by default for Opus 4.5
- Thinking mode configuration has moved to /config
- Added search functionality to `/permissions` command with `/` keyboard shortcut for filtering rules by tool name
- Show reason why autoupdater is disabled in `/doctor`
- Fixed false "Another process is currently updating Claude" error when running `claude update` while another instance is already on the latest version
- Fixed MCP servers from `.mcp.json` being stuck in pending state when running in non-interactive mode (`-p` flag or piped input)
- Fixed scroll position resetting after deleting a permission rule in `/permissions`
- Fixed word deletion (opt+delete) and word navigation (opt+arrow) not working correctly with non-Latin text such as Cyrillic, Greek, Arabic, Hebrew, Thai, and Chinese
- Fixed `claude install --force` not bypassing stale lock files
- Fixed consecutive @~/ file references in CLAUDE.md being incorrectly parsed due to markdown strikethrough interference
- Windows: Fixed plugin MCP servers failing due to colons in log directory paths

## 2.0.65

- Added ability to switch models while writing a prompt using alt+p (linux, windows), option+p (macos).
- Added context window information to status line input
- Added `fileSuggestion` setting for custom `@` file search commands
- Added `CLAUDE_CODE_SHELL` environment variable to override automatic shell detection (useful when login shell differs from actual working shell)
- Fixed prompt not being saved to history when aborting a query with Escape
- Fixed Read tool image handling to identify format from bytes instead of file extension

## 2.0.64

- Made auto-compacting instant
- Agents and bash commands can run asynchronously and send messages to wake up the main agent
- /stats now provides users with interesting CC stats, such as favorite model, usage graph, usage streak
- Added named session support: use `/rename` to name sessions, `/resume <name>` in REPL or `claude --resume <name>` from the terminal to resume them
- Added support for .claude/rules/`.  See https://code.claude.com/docs/en/memory for details.
- Added image dimension metadata when images are resized, enabling accurate coordinate mappings for large images
- Fixed auto-loading .env when using native installer
- Fixed `--system-prompt` being ignored when using `--continue` or `--resume` flags
- Improved `/resume` screen with grouped forked sessions and keyboard shortcuts for preview (P) and rename (R)
- VSCode: Added copy-to-clipboard button on code blocks and bash tool inputs
- VSCode: Fixed extension not working on Windows ARM64 by falling back to x64 binary via emulation
- Bedrock: Improve efficiency of token counting
- Bedrock: Add support for `aws login` AWS Management Console credentials
- Unshipped AgentOutputTool and BashOutputTool, in favor of a new unified TaskOutputTool

## 2.0.62

- Added "(Recommended)" indicator for multiple-choice questions, with the recommended option moved to the top of the list
- Added `attribution` setting to customize commit and PR bylines (deprecates `includeCoAuthoredBy`)
- Fixed duplicate slash commands appearing when ~/.claude is symlinked to a project directory
- Fixed slash command selection not working when multiple commands share the same name
- Fixed an issue where skill files inside symlinked skill directories could become circular symlinks
- Fixed running versions getting removed because lock file incorrectly going stale
- Fixed IDE diff tab not closing when rejecting file changes

## 2.0.61

- Reverted VSCode support for multiple terminal clients due to responsiveness issues.

## 2.0.60

- Added background agent support. Agents run in the background while you work
- Added --disable-slash-commands CLI flag to disable all slash commands
- Added model name to "Co-Authored-By" commit messages
- Enabled "/mcp enable [server-name]" or "/mcp disable [server-name]" to quickly toggle all servers
- Updated Fetch to skip summarization for pre-approved websites
- VSCode: Added support for multiple terminal clients connecting to the IDE server simultaneously

## 2.0.59

- Added --agent CLI flag to override the agent setting for the current session
- Added `agent` setting to configure main thread with a specific agent's system prompt, tool restrictions, and model
- VS Code: Fixed .claude.json config file being read from incorrect location

## 2.0.58

- Pro users now have access to Opus 4.5 as part of their subscription!
- Fixed timer duration showing "11m 60s" instead of "12m 0s"
- Windows: Managed settings now prefer `C:\Program Files\ClaudeCode` if it exists. Support for `C:\ProgramData\ClaudeCode` will be removed in a future version.

## 2.0.57

- Added feedback input when rejecting plans, allowing users to tell Claude what to change
- VSCode: Added streaming message support for real-time response display

## 2.0.56

- Added setting to enable/disable terminal progress bar (OSC 9;4)
- VSCode Extension: Added support for VS Code's secondary sidebar (VS Code 1.97+), allowing Claude Code to be displayed in the right sidebar while keeping the file explorer on the left. Requires setting sidebar as Preferred Location in the config.

## 2.0.55

- Fixed proxy DNS resolution being forced on by default. Now opt-in via `CLAUDE_CODE_PROXY_RESOLVES_HOSTS=true` environment variable
- Fixed keyboard navigation becoming unresponsive when holding down arrow keys in memory location selector
- Improved AskUserQuestion tool to auto-submit single-select questions on the last question, eliminating the extra review screen for simple question flows
- Improved fuzzy matching for `@` file suggestions with faster, more accurate results

## 2.0.54

- Hooks: Enable PermissionRequest hooks to process 'always allow' suggestions and apply permission updates
- Fix issue with excessive iTerm notifications

## 2.0.52

- Fixed duplicate message display when starting Claude with a command line argument
- Fixed `/usage` command progress bars to fill up as usage increases (instead of showing remaining percentage)
- Fixed image pasting not working on Linux systems running Wayland (now falls back to wl-paste when xclip is unavailable)
- Permit some uses of `$!` in bash commands

## 2.0.51

- Added Opus 4.5! https://www.anthropic.com/news/claude-opus-4-5
- Introducing Claude Code for Desktop: https://claude.com/download
- To give you room to try out our new model, we've updated usage limits for Claude Code users. See the Claude Opus 4.5 blog for full details
- Pro users can now purchase extra usage for access to Opus 4.5 in Claude Code
- Plan Mode now builds more precise plans and executes more thoroughly
- Usage limit notifications now easier to understand
- Switched `/usage` back to "% used"
- Fixed handling of thinking errors
- Fixed performance regression

## 2.0.50

- Fixed bug preventing calling MCP tools that have nested references in their input schemas
- Silenced a noisy but harmless error during upgrades
- Improved ultrathink text display
- Improved clarity of 5-hour session limit warning message

## 2.0.49

- Added readline-style ctrl-y for pasting deleted text
- Improved clarity of usage limit warning message
- Fixed handling of subagent permissions

## 2.0.47

- Improved error messages and validation for `claude --teleport`
- Improved error handling in `/usage`
- Fixed race condition with history entry not getting logged at exit
- Fixed Vertex AI configuration not being applied from `settings.json`

## 2.0.46

- Fixed image files being reported with incorrect media type when format cannot be detected from metadata

## 2.0.45

- Added support for Microsoft Foundry! See https://code.claude.com/docs/en/azure-ai-foundry
- Added `PermissionRequest` hook to automatically approve or deny tool permission requests with custom logic
- Send background tasks to Claude Code on the web by starting a message with `&`

## 2.0.43

- Added `permissionMode` field for custom agents
- Added `tool_use_id` field to `PreToolUseHookInput` and `PostToolUseHookInput` types
- Added skills frontmatter field to declare skills to auto-load for subagents
- Added the `SubagentStart` hook event
- Fixed nested `CLAUDE.md` files not loading when @-mentioning files
- Fixed duplicate rendering of some messages in the UI
- Fixed some visual flickers
- Fixed NotebookEdit tool inserting cells at incorrect positions when cell IDs matched the pattern `cell-N`

## 2.0.42

- Added `agent_id` and `agent_transcript_path` fields to `SubagentStop` hooks.

## 2.0.41

- Added `model` parameter to prompt-based stop hooks, allowing users to specify a custom model for hook evaluation
- Fixed slash commands from user settings being loaded twice, which could cause rendering issues
- Fixed incorrect labeling of user settings vs project settings in command descriptions
- Fixed crash when plugin command hooks timeout during execution
- Fixed: Bedrock users no longer see duplicate Opus entries in the /model picker when using `--model haiku`
- Fixed broken security documentation links in trust dialogs and onboarding
- Fixed issue where pressing ESC to close the diff modal would also interrupt the model
- ctrl-r history search landing on a slash command no longer cancels the search
- SDK: Support custom timeouts for hooks
- Allow more safe git commands to run without approval
- Plugins: Added support for sharing and installing output styles
- Teleporting a session from web will automatically set the upstream branch

## 2.0.37

- Fixed how idleness is computed for notifications
- Hooks: Added matcher values for Notification hook events
- Output Styles: Added `keep-coding-instructions` option to frontmatter

## 2.0.36

- Fixed: DISABLE_AUTOUPDATER environment variable now properly disables package manager update notifications
- Fixed queued messages being incorrectly executed as bash commands
- Fixed input being lost when typing while a queued message is processed

## 2.0.35

- Improve fuzzy search results when searching commands
- Improved VS Code extension to respect `chat.fontSize` and `chat.fontFamily` settings throughout the entire UI, and apply font changes immediately without requiring reload
- Added `CLAUDE_CODE_EXIT_AFTER_STOP_DELAY` environment variable to automatically exit SDK mode after a specified idle duration, useful for automated workflows and scripts
- Migrated `ignorePatterns` from project config to deny permissions in the localSettings.
- Fixed menu navigation getting stuck on items with empty string or other falsy values (e.g., in the `/hooks` menu)

## 2.0.34

- VSCode Extension: Added setting to configure the initial permission mode for new conversations
- Improved file path suggestion performance with native Rust-based fuzzy finder
- Fixed infinite token refresh loop that caused MCP servers with OAuth (e.g., Slack) to hang during connection
- Fixed memory crash when reading or writing large files (especially base64-encoded images)

## 2.0.33

- Native binary installs now launch quicker.
- Fixed `claude doctor` incorrectly detecting Homebrew vs npm-global installations by properly resolving symlinks
- Fixed `claude mcp serve` exposing tools with incompatible outputSchemas

## 2.0.32

- Un-deprecate output styles based on community feedback
- Added `companyAnnouncements` setting for displaying announcements on startup
- Fixed hook progress messages not updating correctly during PostToolUse hook execution

## 2.0.31

- Windows: native installation uses shift+tab as shortcut for mode switching, instead of alt+m
- Vertex: add support for Web Search on supported models
- VSCode: Adding the respectGitIgnore configuration to include .gitignored files in file searches (defaults to true)
- Fixed a bug with subagents and MCP servers related to "Tool names must be unique" error
- Fixed issue causing `/compact` to fail with `prompt_too_long` by making it respect existing compact boundaries
- Fixed plugin uninstall not removing plugins

## 2.0.30

- Added helpful hint to run `security unlock-keychain` when encountering API key errors on macOS with locked keychain
- Added `allowUnsandboxedCommands` sandbox setting to disable the dangerouslyDisableSandbox escape hatch at policy level
- Added `disallowedTools` field to custom agent definitions for explicit tool blocking
- Added prompt-based stop hooks
- VSCode: Added respectGitIgnore configuration to include .gitignored files in file searches (defaults to true)
- Enabled SSE MCP servers on native build
- Deprecated output styles. Review options in `/output-style` and use --system-prompt-file, --system-prompt, --append-system-prompt, CLAUDE.md, or plugins instead
- Removed support for custom ripgrep configuration, resolving an issue where Search returns no results and config discovery fails
- Fixed Explore agent creating unwanted .md investigation files during codebase exploration
- Fixed a bug where `/context` would sometimes fail with "max_tokens must be greater than thinking.budget_tokens" error message
- Fixed `--mcp-config` flag to correctly override file-based MCP configurations
- Fixed bug that saved session permissions to local settings
- Fixed MCP tools not being available to sub-agents
- Fixed hooks and plugins not executing when using --dangerously-skip-permissions flag
- Fixed delay when navigating through typeahead suggestions with arrow keys
- VSCode: Restored selection indicator in input footer showing current file or code selection status

## 2.0.28

- Plan mode: introduced new Plan subagent
- Subagents: claude can now choose to resume subagents
- Subagents: claude can dynamically choose the model used by its subagents
- SDK: added --max-budget-usd flag
- Discovery of custom slash commands, subagents, and output styles no longer respects .gitignore
- Stop `/terminal-setup` from adding backslash to `Shift + Enter` in VS Code
- Add branch and tag support for git-based plugins and marketplaces using fragment syntax (e.g., `owner/repo#branch`)
- Fixed a bug where macOS permission prompts would show up upon initial launch when launching from home directory
- Various other bug fixes

## 2.0.27

- New UI for permission prompts
- Added current branch filtering and search to session resume screen for easier navigation
- Fixed directory @-mention causing "No assistant message found" error
- VSCode Extension: Add config setting to include .gitignored files in file searches
- VSCode Extension: Bug fixes for unrelated 'Warmup' conversations, and configuration/settings occasionally being reset to defaults

## 2.0.25

- Removed legacy SDK entrypoint. Please migrate to @anthropic-ai/claude-agent-sdk for future SDK updates: https://platform.claude.com/docs/en/agent-sdk/migration-guide

## 2.0.24

- Fixed a bug where project-level skills were not loading when --setting-sources 'project' was specified
- Claude Code Web: Support for Web -> CLI teleport
- Sandbox: Releasing a sandbox mode for the BashTool on Linux & Mac
- Bedrock: Display awsAuthRefresh output when auth is required

## 2.0.22

- Fixed content layout shift when scrolling through slash commands
- IDE: Add toggle to enable/disable thinking.
- Fix bug causing duplicate permission prompts with parallel tool calls
- Add support for enterprise managed MCP allowlist and denylist

## 2.0.21

- Support MCP `structuredContent` field in tool responses
- Added an interactive question tool
- Claude will now ask you questions more often in plan mode
- Added Haiku 4.5 as a model option for Pro users
- Fixed an issue where queued commands don't have access to previous messages' output

## 2.0.20

- Added support for Claude Skills

## 2.0.19

- Auto-background long-running bash commands instead of killing them. Customize with BASH_DEFAULT_TIMEOUT_MS
- Fixed a bug where Haiku was unnecessarily called in print mode

## 2.0.17

- Added Haiku 4.5 to model selector!
- Haiku 4.5 automatically uses Sonnet in plan mode, and Haiku for execution (i.e. SonnetPlan by default)
- 3P (Bedrock and Vertex) are not automatically upgraded yet. Manual upgrading can be done through setting `ANTHROPIC_DEFAULT_HAIKU_MODEL`
- Introducing the Explore subagent. Powered by Haiku it'll search through your codebase efficiently to save context!
- OTEL: support HTTP_PROXY and HTTPS_PROXY
- `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` now disables release notes fetching

## 2.0.15

- Fixed bug with resuming where previously created files needed to be read again before writing
- Fixed bug with `-p` mode where @-mentioned files needed to be read again before writing

## 2.0.14

- Fix @-mentioning MCP servers to toggle them on/off
- Improve permission checks for bash with inline env vars
- Fix ultrathink + thinking toggle
- Reduce unnecessary logins
- Document --system-prompt
- Several improvements to rendering
- Plugins UI polish

## 2.0.13

- Fixed `/plugin` not working on native build

## 2.0.12

- **Plugin System Released**: Extend Claude Code with custom commands, agents, hooks, and MCP servers from marketplaces
- `/plugin install`, `/plugin enable/disable`, `/plugin marketplace` commands for plugin management
- Repository-level plugin configuration via `extraKnownMarketplaces` for team collaboration
- `/plugin validate` command for validating plugin structure and configuration
- Plugin announcement blog post at https://www.anthropic.com/news/claude-code-plugins
- Plugin documentation available at https://code.claude.com/docs/en/plugins
- Comprehensive error messages and diagnostics via `/doctor` command
- Avoid flickering in `/model` selector
- Improvements to `/help`
- Avoid mentioning hooks in `/resume` summaries
- Changes to the "verbose" setting in `/config` now persist across sessions

## 2.0.11

- Reduced system prompt size by 1.4k tokens
- IDE: Fixed keyboard shortcuts and focus issues for smoother interaction
- Fixed Opus fallback rate limit errors appearing incorrectly
- Fixed /add-dir command selecting wrong default tab

## 2.0.10

- Rewrote terminal renderer for buttery smooth UI
- Enable/disable MCP servers by @mentioning, or in /mcp
- Added tab completion for shell commands in bash mode
- PreToolUse hooks can now modify tool inputs
- Press Ctrl-G to edit your prompt in your system's configured text editor
- Fixes for bash permission checks with environment variables in the command

## 2.0.9

- Fix regression where bash backgrounding stopped working

## 2.0.8

- Update Bedrock default Sonnet model to `global.anthropic.claude-sonnet-4-5-20250929-v1:0`
- IDE: Add drag-and-drop support for files and folders in chat
- /context: Fix counting for thinking blocks
- Improve message rendering for users with light themes on dark terminals
- Remove deprecated .claude.json allowedTools, ignorePatterns, env, and todoFeatureEnabled config options (instead, configure these in your settings.json)

## 2.0.5

- IDE: Fix IME unintended message submission with Enter and Tab
- IDE: Add "Open in Terminal" link in login screen
- Fix unhandled OAuth expiration 401 API errors
- SDK: Added SDKUserMessageReplay.isReplay to prevent duplicate messages

## 2.0.1

- Skip Sonnet 4.5 default model setting change for Bedrock and Vertex
- Various bug fixes and presentation improvements

## 2.0.0

- New native VS Code extension
- Fresh coat of paint throughout the whole app
- /rewind a conversation to undo code changes
- /usage command to see plan limits
- Tab to toggle thinking (sticky across sessions)
- Ctrl-R to search history
- Unshipped claude config command
- Hooks: Reduced PostToolUse 'tool_use' ids were found without 'tool_result' blocks errors
- SDK: The Claude Code SDK is now the Claude Agent SDK
- Add subagents dynamically with `--agents` flag

## 1.0.126

- Enable /context command for Bedrock and Vertex
- Add mTLS support for HTTP-based OpenTelemetry exporters

## 1.0.124

- Set `CLAUDE_BASH_NO_LOGIN` environment variable to 1 or true to to skip login shell for BashTool
- Fix Bedrock and Vertex environment variables evaluating all strings as truthy
- No longer inform Claude of the list of allowed tools when permission is denied
- Fixed security vulnerability in Bash tool permission checks
- Improved VSCode extension performance for large files

## 1.0.123

- Bash permission rules now support output redirections when matching (e.g., `Bash(python:*)` matches `python script.py > output.txt`)
- Fixed thinking mode triggering on negation phrases like "don't think"
- Fixed rendering performance degradation during token streaming
- Added SlashCommand tool, which enables Claude to invoke your slash commands. https://code.claude.com/docs/en/slash-commands#SlashCommand-tool
- Enhanced BashTool environment snapshot logging
- Fixed a bug where resuming a conversation in headless mode would sometimes enable thinking unnecessarily
- Migrated --debug logging to a file, to enable easy tailing & filtering

## 1.0.120

- Fix input lag during typing, especially noticeable with large prompts
- Improved VSCode extension command registry and sessions dialog user experience
- Enhanced sessions dialog responsiveness and visual feedback
- Fixed IDE compatibility issue by removing worktree support check
- Fixed security vulnerability where Bash tool permission checks could be bypassed using prefix matching

## 1.0.119

- Fix Windows issue where process visually freezes on entering interactive mode
- Support dynamic headers for MCP servers via headersHelper configuration
- Fix thinking mode not working in headless sessions
- Fix slash commands now properly update allowed tools instead of replacing them

## 1.0.117

- Add Ctrl-R history search to recall previous commands like bash/zsh
- Fix input lag while typing, especially on Windows
- Add sed command to auto-allowed commands in acceptEdits mode
- Fix Windows PATH comparison to be case-insensitive for drive letters
- Add permissions management hint to /add-dir output

## 1.0.115

- Improve thinking mode display with enhanced visual effects
- Type /t to temporarily disable thinking mode in your prompt
- Improve path validation for glob and grep tools
- Show condensed output for post-tool hooks to reduce visual clutter
- Fix visual feedback when loading state completes
- Improve UI consistency for permission request dialogs

## 1.0.113

- Deprecated piped input in interactive mode
- Move Ctrl+R keybinding for toggling transcript to Ctrl+O

## 1.0.112

- Transcript mode (Ctrl+R): Added the model used to generate each assistant message
- Addressed issue where some Claude Max users were incorrectly recognized as Claude Pro users
- Hooks: Added systemMessage support for SessionEnd hooks
- Added `spinnerTipsEnabled` setting to disable spinner tips
- IDE: Various improvements and bug fixes

## 1.0.111

- /model now validates provided model names
- Fixed Bash tool crashes caused by malformed shell syntax parsing

## 1.0.110

- /terminal-setup command now supports WezTerm
- MCP: OAuth tokens now proactively refresh before expiration
- Fixed reliability issues with background Bash processes

## 1.0.109

- SDK: Added partial message streaming support via `--include-partial-messages` CLI flag

## 1.0.106

- Windows: Fixed path permission matching to consistently use POSIX format (e.g., `Read(//c/Users/...)`)

## 1.0.97

- Settings: /doctor now validates permission rule syntax and suggests corrections

## 1.0.94

- Vertex: add support for global endpoints for supported models
- /memory command now allows direct editing of all imported memory files
- SDK: Add custom tools as callbacks
- Added /todos command to list current todo items

## 1.0.93

- Windows: Add alt + v shortcut for pasting images from clipboard
- Support NO_PROXY environment variable to bypass proxy for specified hostnames and IPs

## 1.0.90

- Settings file changes take effect immediately - no restart required

## 1.0.88

- Fixed issue causing "OAuth authentication is currently not supported"
- Status line input now includes `exceeds_200k_tokens`
- Fixed incorrect usage tracking in /cost.
- Introduced `ANTHROPIC_DEFAULT_SONNET_MODEL` and `ANTHROPIC_DEFAULT_OPUS_MODEL` for controlling model aliases opusplan, opus, and sonnet.
- Bedrock: Updated default Sonnet model to Sonnet 4

## 1.0.86

- Added /context to help users self-serve debug context issues
- SDK: Added UUID support for all SDK messages
- SDK: Added `--replay-user-messages` to replay user messages back to stdout

## 1.0.85

- Status line input now includes session cost info
- Hooks: Introduced SessionEnd hook

## 1.0.84

- Fix tool_use/tool_result id mismatch error when network is unstable
- Fix Claude sometimes ignoring real-time steering when wrapping up a task
- @-mention: Add ~/.claude/\* files to suggestions for easier agent, output style, and slash command editing
- Use built-in ripgrep by default; to opt out of this behavior, set USE_BUILTIN_RIPGREP=0

## 1.0.83

- @-mention: Support files with spaces in path
- New shimmering spinner

## 1.0.82

- SDK: Add request cancellation support
- SDK: New additionalDirectories option to search custom paths, improved slash command processing
- Settings: Validation prevents invalid fields in .claude/settings.json files
- MCP: Improve tool name consistency
- Bash: Fix crash when Claude tries to automatically read large files

## 1.0.81

- Released output styles, including new built-in educational output styles "Explanatory" and "Learning". Docs: https://code.claude.com/docs/en/output-styles
- Agents: Fix custom agent loading when agent files are unparsable

## 1.0.80

- UI improvements: Fix text contrast for custom subagent colors and spinner rendering issues

## 1.0.77

- Bash tool: Fix heredoc and multiline string escaping, improve stderr redirection handling
- SDK: Add session support and permission denial tracking
- Fix token limit errors in conversation summarization
- Opus Plan Mode: New setting in `/model` to run Opus only in plan mode, Sonnet otherwise

## 1.0.73

- MCP: Support multiple config files with `--mcp-config file1.json file2.json`
- MCP: Press Esc to cancel OAuth authentication flows
- Bash: Improved command validation and reduced false security warnings
- UI: Enhanced spinner animations and status line visual hierarchy
- Linux: Added support for Alpine and musl-based distributions (requires separate ripgrep installation)

## 1.0.72

- Ask permissions: have Claude Code always ask for confirmation to use specific tools with /permissions

## 1.0.71

- Background commands: (Ctrl-b) to run any Bash command in the background so Claude can keep working (great for dev servers, tailing logs, etc.)
- Customizable status line: add your terminal prompt to Claude Code with /statusline

## 1.0.70

- Performance: Optimized message rendering for better performance with large contexts
- Windows: Fixed native file search, ripgrep, and subagent functionality
- Added support for @-mentions in slash command arguments

## 1.0.69

- Upgraded Opus to version 4.1

## 1.0.68

- Fix incorrect model names being used for certain commands like `/pr-comments`
- Windows: improve permissions checks for allow / deny tools and project trust. This may create a new project entry in `.claude.json` - manually merge the history field if desired.
- Windows: improve sub-process spawning to eliminate "No such file or directory" when running commands like pnpm
- Enhanced /doctor command with CLAUDE.md and MCP tool context for self-serve debugging
- SDK: Added canUseTool callback support for tool confirmation
- Added `disableAllHooks` setting
- Improved file suggestions performance in large repos

## 1.0.65

- IDE: Fixed connection stability issues and error handling for diagnostics
- Windows: Fixed shell environment setup for users without .bashrc files

## 1.0.64

- Agents: Added model customization support - you can now specify which model an agent should use
- Agents: Fixed unintended access to the recursive agent tool
- Hooks: Added systemMessage field to hook JSON output for displaying warnings and context
- SDK: Fixed user input tracking across multi-turn conversations
- Added hidden files to file search and @-mention suggestions

## 1.0.63

- Windows: Fixed file search, @agent mentions, and custom slash commands functionality

## 1.0.62

- Added @-mention support with typeahead for custom agents. @<your-custom-agent> to invoke it
- Hooks: Added SessionStart hook for new session initialization
- /add-dir command now supports typeahead for directory paths
- Improved network connectivity check reliability

## 1.0.61

- Transcript mode (Ctrl+R): Changed Esc to exit transcript mode rather than interrupt
- Settings: Added `--settings` flag to load settings from a JSON file
- Settings: Fixed resolution of settings files paths that are symlinks
- OTEL: Fixed reporting of wrong organization after authentication changes
- Slash commands: Fixed permissions checking for allowed-tools with Bash
- IDE: Added support for pasting images in VSCode MacOS using ⌘+V
- IDE: Added `CLAUDE_CODE_AUTO_CONNECT_IDE=false` for disabling IDE auto-connection
- Added `CLAUDE_CODE_SHELL_PREFIX` for wrapping Claude and user-provided shell commands run by Claude Code

## 1.0.60

- You can now create custom subagents for specialized tasks! Run /agents to get started

## 1.0.59

- SDK: Added tool confirmation support with canUseTool callback
- SDK: Allow specifying env for spawned process
- Hooks: Exposed PermissionDecision to hooks (including "ask")
- Hooks: UserPromptSubmit now supports additionalContext in advanced JSON output
- Fixed issue where some Max users that specified Opus would still see fallback to Sonnet

## 1.0.58

- Added support for reading PDFs
- MCP: Improved server health status display in 'claude mcp list'
- Hooks: Added CLAUDE_PROJECT_DIR env var for hook commands

## 1.0.57

- Added support for specifying a model in slash commands
- Improved permission messages to help Claude understand allowed tools
- Fix: Remove trailing newlines from bash output in terminal wrapping

## 1.0.56

- Windows: Enabled shift+tab for mode switching on versions of Node.js that support terminal VT mode
- Fixes for WSL IDE detection
- Fix an issue causing awsRefreshHelper changes to .aws directory not to be picked up

## 1.0.55

- Clarified knowledge cutoff for Opus 4 and Sonnet 4 models
- Windows: fixed Ctrl+Z crash
- SDK: Added ability to capture error logging
- Add --system-prompt-file option to override system prompt in print mode

## 1.0.54

- Hooks: Added UserPromptSubmit hook and the current working directory to hook inputs
- Custom slash commands: Added argument-hint to frontmatter
- Windows: OAuth uses port 45454 and properly constructs browser URL
- Windows: mode switching now uses alt + m, and plan mode renders properly
- Shell: Switch to in-memory shell snapshot to fix file-related errors

## 1.0.53

- Updated @-mention file truncation from 100 lines to 2000 lines
- Add helper script settings for AWS token refresh: awsAuthRefresh (for foreground operations like aws sso login) and awsCredentialExport (for background operation with STS-like response).

## 1.0.52

- Added support for MCP server instructions

## 1.0.51

- Added support for native Windows (requires Git for Windows)
- Added support for Bedrock API keys through environment variable AWS_BEARER_TOKEN_BEDROCK
- Settings: /doctor can now help you identify and fix invalid setting files
- `--append-system-prompt` can now be used in interactive mode, not just --print/-p.
- Increased auto-compact warning threshold from 60% to 80%
- Fixed an issue with handling user directories with spaces for shell snapshots
- OTEL resource now includes os.type, os.version, host.arch, and wsl.version (if running on Windows Subsystem for Linux)
- Custom slash commands: Fixed user-level commands in subdirectories
- Plan mode: Fixed issue where rejected plan from sub-task would get discarded

## 1.0.48

- Fixed a bug in v1.0.45 where the app would sometimes freeze on launch
- Added progress messages to Bash tool based on the last 5 lines of command output
- Added expanding variables support for MCP server configuration
- Moved shell snapshots from /tmp to ~/.claude for more reliable Bash tool calls
- Improved IDE extension path handling when Claude Code runs in WSL
- Hooks: Added a PreCompact hook
- Vim mode: Added c, f/F, t/T

## 1.0.45

- Redesigned Search (Grep) tool with new tool input parameters and features
- Disabled IDE diffs for notebook files, fixing "Timeout waiting after 1000ms" error
- Fixed config file corruption issue by enforcing atomic writes
- Updated prompt input undo to Ctrl+\_ to avoid breaking existing Ctrl+U behavior, matching zsh's undo shortcut
- Stop Hooks: Fixed transcript path after /clear and fixed triggering when loop ends with tool call
- Custom slash commands: Restored namespacing in command names based on subdirectories. For example, .claude/commands/frontend/component.md is now /frontend:component, not /component.

## 1.0.44

- New /export command lets you quickly export a conversation for sharing
- MCP: resource_link tool results are now supported
- MCP: tool annotations and tool titles now display in /mcp view
- Changed Ctrl+Z to suspend Claude Code. Resume by running `fg`. Prompt input undo is now Ctrl+U.

## 1.0.43

- Fixed a bug where the theme selector was saving excessively
- Hooks: Added EPIPE system error handling

## 1.0.42

- `/add-dir` 指令新增支援波浪符（`~`）展開

## 1.0.41

- Hooks：將 Stop hook 的觸發拆分為 Stop 與 SubagentStop
- Hooks：支援為每個指令選用逾時設定
- Hooks：在 hook 輸入中新增 "hook_event_name"
- 修正 MCP 工具在工具清單中重複顯示兩次的錯誤
- 在 `tool_decision` 事件中新增 Bash 工具的工具參數 JSON

## 1.0.40

- 修正當設定 `NODE_EXTRA_CA_CERTS` 時，可能導致出現 UNABLE_TO_GET_ISSUER_CERT_LOCALLY 的 API 連線錯誤

## 1.0.39

- OpenTelemetry 記錄新增 Active Time 指標

## 1.0.38

- 發布 hooks。特別感謝社群在 https://github.com/anthropics/claude-code/issues/712 的回饋。文件：https://code.claude.com/docs/en/hooks

## 1.0.37

- 移除透過 ANTHROPIC_AUTH_TOKEN 或 apiKeyHelper 設定 `Proxy-Authorization` 標頭的能力

## 1.0.36

- 網路搜尋現在會將今日日期納入上下文
- 修正退出時 stdio MCP 伺服器未正確終止的錯誤

## 1.0.35

- 新增支援 MCP OAuth 授權伺服器探索（discovery）

## 1.0.34

- 修正記憶體洩漏，避免出現 MaxListenersExceededWarning 訊息

## 1.0.33

- 改善記錄功能，新增工作階段 ID 支援
- 提示詞輸入新增復原功能（Ctrl+Z 與 Vim 的 'u' 指令）
- 改善計畫模式（plan mode）

## 1.0.32

- 更新 litellm 的 loopback 設定
- 新增 forceLoginMethod 設定，可略過登入方式選擇畫面

## 1.0.31

- 修正 ~/.claude.json 內容包含無效 JSON 時檔案會被重設的錯誤

## 1.0.30

- 自訂斜線指令：可執行 bash 輸出、@ 提及檔案，並用思考關鍵字啟用思考模式
- 以檔名比對改善檔案路徑自動補完
- 在 Ctrl-r 模式新增時間戳，並修正 Ctrl-c 的處理
- 強化 jq regex 支援，改善包含 pipe 與 select 的複雜篩選條件

## 1.0.29

- 改善游標導覽與渲染時對 CJK 字元的支援

## 1.0.28

- 斜線指令：修正瀏覽歷史紀錄時選擇器的顯示問題
- 上傳前會先調整圖片大小，以避免 API 尺寸上限錯誤
- 設定目錄新增支援 XDG_CONFIG_HOME
- 記憶體使用效能最佳化
- OpenTelemetry 記錄新增屬性（terminal.type、language）

## 1.0.27

- 現在支援可串流的 HTTP MCP 伺服器
- 遠端 MCP 伺服器（SSE 與 HTTP）現在支援 OAuth
- MCP 資源現在可用 @ 提及
- 新增 /resume 斜線指令，可在 Claude Code 中切換對話

## 1.0.25

- 斜線指令：將 "project" 與 "user" 前綴移至描述中
- 斜線指令：提升指令探索的可靠性
- 改善對 Ghostty 的支援
- 改善網路搜尋的可靠性

## 1.0.24

- 改善 /mcp 輸出
- 修正 settings 陣列被覆寫而非合併的錯誤

## 1.0.23

- 發布 TypeScript SDK：使用 import @anthropic-ai/claude-code 開始使用
- 發布 Python SDK：使用 pip install claude-code-sdk 開始使用

## 1.0.22

- SDK：將 `total_cost` 重新命名為 `total_cost_usd`

## 1.0.21

- 改善以 Tab 縮排檔案的編輯體驗
- 修正 tool_use 沒有對應 tool_result 時的錯誤
- 修正在退出 Claude Code 後 stdio MCP 伺服器行程仍會殘留的錯誤

## 1.0.18

- 新增 --add-dir CLI 參數，用於指定額外的工作目錄
- 新增串流輸入支援，不需 -p 旗標
- 改善啟動效能與工作階段儲存效能
- 新增 CLAUDE_BASH_MAINTAIN_PROJECT_WORKING_DIR 環境變數，用於固定 bash 指令的工作目錄
- 新增更詳細的 MCP 伺服器工具顯示（/mcp）
- 改善 MCP 驗證與權限機制
- 新增 MCP SSE 連線中斷後的自動重新連線
- 修正在出現對話框時貼上內容遺失的問題

## 1.0.17

- 在 -p 模式下，我們現在會輸出子任務的訊息（請查看 parent_tool_use_id 屬性）
- 修正在短時間內多次呼叫 VS Code diff 工具時可能當機的問題
- 改善 MCP 伺服器清單 UI
- 更新 Claude Code 的行程標題，顯示為 "claude" 而非 "node"

## 1.0.11

- Claude Code 現在也可搭配 Claude Pro 訂閱使用
- 新增 /upgrade，讓切換至 Claude Max 方案更順暢
- 改善使用 API 金鑰與 Bedrock/Vertex/外部驗證權杖登入的 UI
- 改善 Shell 設定的錯誤處理
- 改善壓縮（compaction）期間的待辦清單處理

## 1.0.10

- 新增 Markdown 表格支援
- 改善串流效能

## 1.0.8

- 修正在使用 CLOUD_ML_REGION 時 Vertex AI 區域回退（fallback）的問題
- 將預設 otel 間隔由 1s 提高至 5s
- 修正 MCP_TIMEOUT 與 MCP_TOOL_TIMEOUT 在部分邊界情況下未被遵循的問題
- 修正搜尋工具不必要詢問權限的回歸問題
- 新增支援以非英文觸發思考模式
- 改善壓縮（compacting）UI

## 1.0.7

- 將 /allowed-tools 重新命名為 /permissions
- 將 .claude.json 的 allowedTools 與 ignorePatterns 遷移至 settings.json
- 將 claude config 指令標記為已棄用，改為直接編輯 settings.json
- 修正在 --print 模式下 --dangerously-skip-permissions 偶爾無法運作的錯誤
- 改善 /install-github-app 的錯誤處理
- 另包含錯誤修正、UI 微調與工具可靠性改進

## 1.0.6

- 改善以 Tab 縮排檔案的編輯可靠性
- 全面遵循 CLAUDE_CONFIG_DIR 設定
- 減少不必要的工具權限提示
- @file 自動完成現在支援符號連結（symlink）
- 另包含錯誤修正、UI 微調與工具可靠性改進

## 1.0.4

- 修正 MCP 工具錯誤未被正確解析的問題

## 1.0.1

- 新增 `DISABLE_INTERLEAVED_THINKING`，讓使用者可選擇停用交錯式思考（interleaved thinking）
- 改善模型名稱顯示，以呈現各供應商的特定名稱（Bedrock 為 Sonnet 3.7，Console 為 Sonnet 4）
- 更新文件連結與 OAuth 流程說明

## 1.0.0

- Claude Code 現已正式推出（GA）
- 推出 Sonnet 4 與 Opus 4 模型

## 0.2.125

- 重大變更：傳入 `ANTHROPIC_MODEL` 或 `ANTHROPIC_SMALL_FAST_MODEL` 的 Bedrock ARN 不應再包含逸出斜線（請用 `/` 取代 `%2F`）
- 移除 `DEBUG=true`，改用 `ANTHROPIC_LOG=debug` 以記錄所有請求

## 0.2.117

- 重大變更：--print 的 JSON 輸出現在會回傳巢狀的 message 物件，以便在新增 metadata 欄位時保持向前相容
- 新增 settings.cleanupPeriodDays
- 新增 CLAUDE_CODE_API_KEY_HELPER_TTL_MS 環境變數
- 新增 --debug 模式

## 0.2.108

- 現在可在 Claude 工作時傳送訊息，即時引導 Claude
- 新增 BASH_DEFAULT_TIMEOUT_MS 與 BASH_MAX_TIMEOUT_MS 環境變數
- 修正在 -p 模式下思考功能無法運作的錯誤
- 修正 /cost 報告的回歸問題
- 以其他 MCP 指令取代 MCP 精靈介面，並將其標記為已棄用
- 另包含多項錯誤修正與改進

## 0.2.107

- CLAUDE.md 檔現在可匯入其他檔案：在 ./CLAUDE.md 加入 @path/to/file.md，即可在啟動時載入額外檔案

## 0.2.106

- MCP SSE 伺服器設定現在可指定自訂標頭
- 修正 MCP 權限提示不一定會正確顯示的錯誤

## 0.2.105

- Claude 現在可以搜尋網路
- 將系統與帳戶狀態移至 /status
- 新增 Vim 的單字移動快捷鍵
- 改善啟動、Todo 工具與檔案編輯的延遲表現

## 0.2.102

- 改善思考模式觸發的可靠性
- 改善圖片與資料夾的 @ 提及可靠性
- 現在可在單一提示詞中貼上多個大型片段

## 0.2.100

- 修正因堆疊溢位（stack overflow）錯誤導致的當機
- 將 db 儲存改為選用；若缺少 db 支援，將停用 --continue 與 --resume

## 0.2.98

- 修正自動壓縮（auto-compact）執行兩次的問題

## 0.2.96

- Claude Code 現在也可搭配 Claude Max 訂閱使用（https://claude.ai/upgrade）

## 0.2.93

- 使用 "claude --continue" 與 "claude --resume" 可從上次中斷處續接對話
- Claude 現在可使用待辦清單（Todo list），幫助保持進度並更有條理

## 0.2.82

- 新增對 --disallowedTools 的支援
- 為一致性調整工具名稱：LSTool -> LS、View -> Read 等

## 0.2.75

- Claude 工作時可按 Enter 將後續訊息加入佇列
- 直接將圖片檔拖曳進提示詞，或複製貼上
- 使用 @ 提及檔案，可直接加入上下文
- 使用 `claude --mcp-config <path-to-file>` 執行單次 MCP 伺服器
- 改善檔名自動補完的效能

## 0.2.74

- 新增支援重新整理動態產生的 API 金鑰（透過 apiKeyHelper），存活時間（TTL）為 5 分鐘
- Task 工具現在可進行寫入並執行 bash 指令

## 0.2.72

- 更新旋轉指示器，顯示已載入的 token 與工具使用狀態

## 0.2.70

- Claude 現在可使用像 curl 這類網路指令
- Claude 現在可並行執行多個網頁查詢
- 在 Auto-accept 模式下，按一次 ESC 即可立即中斷 Claude

## 0.2.69

- 改善 Select 元件行為，修正 UI 小瑕疵
- 改善文字截斷邏輯，強化終端機輸出顯示

## 0.2.67

- 可將共享的專案權限規則儲存在 .claude/settings.json

## 0.2.66

- 列印模式（-p）現在支援透過 --output-format=stream-json 串流輸出
- 修正貼上內容時可能意外觸發記憶或 bash 模式的問題

## 0.2.63

- 修正 MCP 工具被載入兩次而導致工具呼叫錯誤的問題

## 0.2.61

- 可使用 Vim 風格按鍵（j/k）或 bash/emacs 快捷鍵（Ctrl+n/p）瀏覽選單，加快操作速度
- 強化圖片偵測，讓剪貼簿貼上更可靠
- 修正按下 ESC 可能導致對話歷史選擇器當機的問題

## 0.2.59

- 直接將圖片複製貼上到提示詞中
- 改善 bash 與 fetch 工具的進度顯示
- 修正非互動模式（-p）的錯誤

## 0.2.54

- 以 '#' 開頭即可快速新增至記憶（Memory）
- 按 ctrl+r 查看較長工具結果的完整輸出
- 新增 MCP SSE 傳輸支援

## 0.2.53

- 新增網頁抓取工具，讓 Claude 能查看你貼上的 URL
- 修正 JPEG 偵測的錯誤

## 0.2.50

- 新增 MCP "project" 範圍：現在可將 MCP 伺服器加入 .mcp.json 檔案並提交到你的儲存庫

## 0.2.49

- 既有 MCP 伺服器範圍已重新命名：原本的 "project" 現為 "local"，"global" 現為 "user"

## 0.2.47

- 按 Tab 自動補完檔案與資料夾名稱
- 按 Shift + Tab 切換是否自動接受檔案編輯
- 自動壓縮對話以支援無限對話長度（可用 /config 切換）

## 0.2.44

- 在思考模式下請 Claude 擬定計畫：只要說 'think'、'think harder'，甚至 'ultrathink'

## 0.2.41

- MCP 伺服器啟動逾時時間現在可透過 MCP_TIMEOUT 環境變數設定
- MCP 伺服器啟動不再阻塞應用程式啟動

## 0.2.37

- 新增 /release-notes 指令，讓你隨時查看發行說明
- `claude config add/remove` 指令現在可接受以逗號或空白分隔的多個值

## 0.2.36

- 使用 `claude mcp add-from-claude-desktop` 從 Claude Desktop 匯入 MCP 伺服器
- 使用 `claude mcp add-json <n> <json>` 以 JSON 字串新增 MCP 伺服器

## 0.2.34

- 文字輸入的 Vim 鍵位綁定：使用 /vim 或 /config 啟用

## 0.2.32

- 互動式 MCP 設定精靈：執行 "claude mcp add"，以逐步介面新增 MCP 伺服器
- 修正部分 PersistentShell 問題

## 0.2.31

- 自訂斜線指令：.claude/commands/ 目錄中的 Markdown 檔現在會顯示為自訂斜線指令，用於將提示詞插入你的對話
- MCP 偵錯模式：使用 --mcp-debug 旗標執行，可取得更多 MCP 伺服器錯誤資訊

## 0.2.30

- 新增 ANSI 色彩主題，以提升終端機相容性
- 修正斜線指令參數未正確送出的問題
- （僅限 Mac）API 金鑰現在會儲存在 macOS 鑰匙圈

## 0.2.26

- 新增 /approved-tools 指令，用於管理工具權限
- 提供字詞層級的差異（diff）顯示，提升程式碼可讀性
- 支援斜線指令的模糊比對

## 0.2.21

- 支援 /commands 的模糊比對
