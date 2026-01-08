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

- 新增 Claude in Chrome（Beta）功能：搭配 Chrome 擴充套件（https://claude.ai/chrome），可讓你直接從 Claude Code 控制瀏覽器
- 減少終端機閃爍
- 行動 App 提示新增可掃描 QR code，方便快速下載
- 續接對話時新增載入指示器，提供更佳回饋
- 修正非互動模式下 `/context` 指令未遵循自訂系統提示詞的問題
- 修正使用 Ctrl+Y 貼上時，連續 Ctrl+K 行的順序問題
- 改善 @ 提及檔案建議速度（在 git 儲存庫中約快 3 倍）
- 改善含 `.ignore` 或 `.rgignore` 的儲存庫之檔案建議效能
- 讓 settings 驗證錯誤更醒目
- 將思考模式切換由 Tab 改為 Alt+T，避免誤觸

## 2.0.71

- 新增 /config 開關，可啟用／停用提示詞建議
- 新增 `/settings` 作為 `/config` 指令別名
- 修正游標位於路徑中間時，@ 檔案引用建議會被誤觸發的問題
- 修正使用 `--dangerously-skip-permissions` 時 `.mcp.json` 的 MCP 伺服器不會載入的問題
- 修正權限規則會誤拒包含 shell glob 模式的合法 bash 指令（例如：`ls *.txt`、`for f in *.png`）
- Bedrock：token 計數與 inference profile 清單現在會遵循環境變數 `ANTHROPIC_BEDROCK_BASE_URL`
- 原生版本新增語法醒目提示引擎

## 2.0.70

- 新增 Enter 鍵：可立即接受並提交提示詞建議（Tab 仍用於接受後再編輯）
- MCP 工具權限新增萬用字元語法 `mcp__server__*`，可允許或拒絕某伺服器的所有工具
- 外掛市集新增自動更新開關，可針對每個市集個別控制是否自動更新
- 狀態列輸入新增 `current_usage` 欄位，可更準確計算上下文視窗百分比
- 修正在使用者打字時處理佇列指令會清空輸入內容的問題
- 修正按 Tab 時提示詞建議會覆蓋已輸入內容的問題
- 修正終端機尺寸變更時 diff 檢視未更新的問題
- 大型對話的記憶體使用量改善 3 倍
- 提升複製到剪貼簿的統計截圖（Ctrl+S）解析度，畫面更清晰
- 移除使用 # 快捷新增記憶（請改請 Claude 編輯你的 CLAUDE.md）
- 修正 /config 中的思考模式切換無法正確持久化的問題
- 改善檔案建立權限對話框的 UI

## 2.0.69

- 小幅錯誤修正

## 2.0.68

- 修正 IME（輸入法）支援：針對中文、日文、韓文等語言，將組字視窗正確定位在游標處
- 修正被禁止的 MCP 工具仍對模型可見的錯誤
- 修正子代理工作期間，引導訊息（steering messages）可能遺失的問題
- 修正 Option+Arrow 單字導覽：不再把整段 CJK（中文、日文、韓文）文字視為單一單字，而是依單字邊界導覽
- 改善退出計畫模式的 UX：當計畫為空或缺失時，改顯示簡化的 yes/no 對話框，而非拋出錯誤
- 新增支援企業受管理設定。請聯絡你的 Anthropic 客戶團隊以啟用此功能。

## 2.0.67

- Opus 4.5 現在預設啟用思考模式
- 思考模式設定已移至 /config
- `/permissions` 指令新增搜尋功能：使用 `/` 快捷鍵可依工具名稱篩選規則
- 在 `/doctor` 顯示 autoupdater 被停用的原因
- 修正誤判錯誤：當另一個實例已是最新版本時執行 `claude update`，會錯誤顯示 "Another process is currently updating Claude"
- 修正 `.mcp.json` 的 MCP 伺服器在非互動模式（`-p` 旗標或管線輸入）下卡在 pending 狀態的問題
- 修正在 `/permissions` 刪除權限規則後捲動位置會重設的問題
- 修正在西里爾文、希臘文、阿拉伯文、希伯來文、泰文與中文等非拉丁文字中，單字刪除（opt+delete）與單字導覽（opt+arrow）無法正常運作的問題
- 修正 `claude install --force` 無法略過過期 lock 檔的問題
- 修正 CLAUDE.md 中連續 @~/ 檔案引用會因 Markdown 刪除線干擾而被錯誤解析的問題
- Windows：修正外掛 MCP 伺服器因 log 目錄路徑含冒號而失敗的問題

## 2.0.65

- 新增可在撰寫提示詞時切換模型：Linux/Windows 使用 alt+p，macOS 使用 option+p
- 狀態列輸入新增上下文視窗資訊
- 新增 `fileSuggestion` 設定，用於自訂 `@` 檔案搜尋指令
- 新增 `CLAUDE_CODE_SHELL` 環境變數，可覆寫自動 shell 偵測（適用於 login shell 與實際工作 shell 不同的情況）
- 修正按 Escape 中止查詢時提示詞不會被寫入歷史紀錄的問題
- 修正 Read 工具的圖片處理：改為從位元組判定格式，而非檔案副檔名

## 2.0.64

- 讓自動壓縮（auto-compacting）即時完成
- Agents 與 bash 指令可非同步執行，並可傳送訊息喚醒主 agent
- /stats 現在提供使用者有趣的 CC 統計資訊，例如最常用模型、用量圖表、使用連續天數
- 新增具名工作階段：使用 `/rename` 命名工作階段；在 REPL 使用 `/resume <name>`，或在終端機使用 `claude --resume <name>` 續接
- 新增支援 .claude/rules/`。詳情請見 https://code.claude.com/docs/en/memory
- 圖片縮放時新增圖片尺寸 metadata，讓大型圖片可準確對應座標
- 修正使用原生安裝程式時 .env 會自動載入的問題
- 修正使用 `--continue` 或 `--resume` 旗標時 `--system-prompt` 被忽略的問題
- 改善 `/resume` 畫面：將 forked 工作階段分組，並加入預覽（P）與重新命名（R）的鍵盤快捷鍵
- VSCode：在程式碼區塊與 bash 工具輸入新增複製到剪貼簿按鈕
- VSCode：修正擴充套件在 Windows ARM64 無法運作的問題，改以模擬方式回退使用 x64 二進位
- Bedrock：改善 token 計數效率
- Bedrock：新增支援 `aws login` 的 AWS Management Console 憑證
- 移除未發佈的 AgentOutputTool 與 BashOutputTool，改用新的統一 TaskOutputTool

## 2.0.62

- 為選擇題新增「(Recommended)」標記，並將建議選項移到清單頂端
- 新增 `attribution` 設定，可自訂 commit 與 PR 的署名（將 `includeCoAuthoredBy` 標記為已棄用）
- 修正當 ~/.claude symlink 到專案目錄時，會出現重複斜線指令的問題
- 修正多個指令同名時，斜線指令選擇無法運作的問題
- 修正 symlink 的 skill 目錄內的 skill 檔案可能變成循環 symlink 的問題
- 修正 lock 檔錯誤地判定為過期，導致執行中版本被移除的問題
- 修正拒絕檔案變更時 IDE diff 分頁未關閉的問題

## 2.0.61

- 因反應速度問題，回復（revert）VSCode 對多個終端機客戶端同時連線的支援。

## 2.0.60

- 新增背景 agent 支援：agent 可在背景執行，你可同時繼續工作
- 新增 --disable-slash-commands CLI 旗標，可停用所有斜線指令
- 在 "Co-Authored-By" 的提交訊息中新增模型名稱
- 新增 "/mcp enable [server-name]" 與 "/mcp disable [server-name]"，可快速切換所有伺服器狀態
- 更新 Fetch：對預先核准的網站略過摘要
- VSCode：新增支援多個終端機客戶端同時連線至 IDE 伺服器

## 2.0.59

- 新增 --agent CLI 旗標，可覆寫目前工作階段的 agent 設定
- 新增 `agent` 設定，可用特定 agent 的系統提示詞、工具限制與模型來設定主執行緒
- VS Code：修正 .claude.json 設定檔從錯誤位置讀取的問題

## 2.0.58

- Pro 使用者現在可在訂閱方案中使用 Opus 4.5！
- 修正計時器時間顯示為 "11m 60s" 而非 "12m 0s" 的問題
- Windows：受管理設定現在會優先使用 `C:\\Program Files\\ClaudeCode`（若存在）。未來版本將移除對 `C:\\ProgramData\\ClaudeCode` 的支援。

## 2.0.57

- 在拒絕計畫時新增回饋輸入，讓使用者可告訴 Claude 要改哪些地方
- VSCode：新增串流訊息支援，以即時顯示回應

## 2.0.56

- 新增設定，可啟用／停用終端機進度條（OSC 9;4）
- VSCode 擴充套件：新增支援 VS Code 的次側邊欄（VS Code 1.97+），可將 Claude Code 顯示在右側邊欄，同時保留檔案總管在左側。需要在設定中將 sidebar 設為 Preferred Location。

## 2.0.55

- 修正 proxy DNS 解析預設被強制啟用的問題。現在改為選用，可透過 `CLAUDE_CODE_PROXY_RESOLVES_HOSTS=true` 環境變數啟用
- 修正在記憶位置選擇器中按住方向鍵時鍵盤導覽無回應的問題
- 改善 AskUserQuestion 工具：在最後一題若為單選，會自動提交，讓簡單流程不再需要額外的檢視畫面
- 改善 `@` 檔案建議的模糊比對，結果更快更準確

## 2.0.54

- Hooks：讓 PermissionRequest hooks 可處理「一律允許」建議，並套用權限更新
- 修正 iTerm 通知過多的問題

## 2.0.52

- 修正以命令列參數啟動 Claude 時訊息重複顯示的問題
- 修正 `/usage` 進度條：用量增加時會逐步填滿（而非顯示剩餘百分比）
- 修正在執行 Wayland 的 Linux 上無法貼上圖片的問題（當 xclip 不可用時，現在會回退改用 wl-paste）
- 允許在 bash 指令中使用部分 `$!` 用法

## 2.0.51

- 新增 Opus 4.5！https://www.anthropic.com/news/claude-opus-4-5
- 推出桌面版 Claude Code：https://claude.com/download
- 為了讓你有空間試用新模型，我們已更新 Claude Code 使用者的用量限制。完整細節請參考 Claude Opus 4.5 部落格文章
- Pro 使用者現在可購買額外用量，以在 Claude Code 中使用 Opus 4.5
- 計畫模式現在可建立更精準的計畫並更徹底執行
- 用量限制通知更容易理解
- 將 `/usage` 切回顯示「已使用百分比」
- 修正思考錯誤的處理
- 修正效能回歸問題

## 2.0.50

- 修正：輸入 schema 含巢狀 reference 的 MCP 工具無法呼叫的錯誤
- 升級期間抑制一個吵雜但無害的錯誤訊息
- 改善 ultrathink 文字顯示
- 提升 5 小時工作階段限制警告訊息的清晰度

## 2.0.49

- 新增 readline 風格的 ctrl-y，可貼上被刪除的文字
- 提升用量限制警告訊息的清晰度
- 修正子代理權限處理

## 2.0.47

- 改善 `claude --teleport` 的錯誤訊息與驗證
- 改善 `/usage` 的錯誤處理
- 修正退出時歷史紀錄項目未被記錄的競態條件（race condition）
- 修正 `settings.json` 的 Vertex AI 設定未被套用的問題

## 2.0.46

- 修正當無法從 metadata 偵測格式時，圖片檔被回報為不正確的 media type 的問題

## 2.0.45

- 新增支援 Microsoft Foundry！請見 https://code.claude.com/docs/en/azure-ai-foundry
- 新增 `PermissionRequest` hook，可用自訂邏輯自動核准或拒絕工具權限請求
- 在 Web 版 Claude Code：以 `&` 開頭訊息即可將背景任務送出

## 2.0.43

- 自訂 agent 新增 `permissionMode` 欄位
- `PreToolUseHookInput` 與 `PostToolUseHookInput` 型別新增 `tool_use_id` 欄位
- skills 的 frontmatter 新增欄位，可宣告要為子代理自動載入的 skills
- 新增 `SubagentStart` hook 事件
- 修正 @ 提及檔案時巢狀 `CLAUDE.md` 檔案未載入的問題
- 修正 UI 中部分訊息重複渲染的問題
- 修正部分視覺閃爍
- 修正 NotebookEdit 工具：當 cell ID 符合 `cell-N` 模式時會將 cell 插入錯誤位置

## 2.0.42

- 在 `SubagentStop` hooks 中新增 `agent_id` 與 `agent_transcript_path` 欄位。

## 2.0.41

- 在基於提示詞的 stop hooks 中新增 `model` 參數，讓使用者可指定用於 hook 評估的自訂模型
- 修正來自使用者設定的斜線指令被載入兩次、可能造成渲染問題的錯誤
- 修正指令描述中對使用者設定與專案設定的標示不正確問題
- 修正外掛指令 hooks 執行逾時時會當機的問題
- 修正：Bedrock 使用者在使用 `--model haiku` 時，不再於 /model 選擇器看到重複的 Opus 項目
- 修正信任對話框與導覽流程中的安全性文件連結失效問題
- 修正按 ESC 關閉 diff 視窗時也會中斷模型的問題
- ctrl-r 歷史搜尋停在斜線指令時不再取消搜尋
- SDK：hooks 支援自訂逾時
- 允許更多安全的 git 指令在不需核准的情況下執行
- 外掛：新增支援分享與安裝輸出風格
- 從 Web teleport 工作階段時，會自動設定 upstream branch

## 2.0.37

- 修正通知的閒置時間計算方式
- Hooks：為 Notification hook 事件新增 matcher 值
- 輸出風格：在 frontmatter 新增 `keep-coding-instructions` 選項

## 2.0.36

- 修正：DISABLE_AUTOUPDATER 環境變數現在可正確停用套件管理器更新通知
- 修正佇列訊息被誤當成 bash 指令執行的問題
- 修正在處理佇列訊息時輸入內容遺失的問題

## 2.0.35

- 改善搜尋指令時的模糊搜尋結果
- 改善 VS Code 擴充套件：全 UI 皆遵循 `chat.fontSize` 與 `chat.fontFamily` 設定，且字型變更可立即生效，無需重新載入
- 新增 `CLAUDE_CODE_EXIT_AFTER_STOP_DELAY` 環境變數，可在指定閒置時間後自動退出 SDK 模式，適用於自動化流程與腳本
- 將 `ignorePatterns` 從專案設定遷移到 localSettings 的拒絕權限（deny permissions）
- 修正選單導覽在空字串或其他 falsy 值項目上卡住的問題（例如 `/hooks` 選單）

## 2.0.34

- VSCode 擴充套件：新增設定，可設定新對話的初始權限模式
- 透過原生 Rust 模糊搜尋器改善檔案路徑建議效能
- 修正無限權杖刷新迴圈：避免 OAuth 的 MCP 伺服器（例如 Slack）連線時卡住
- 修正讀寫大型檔案（尤其是 base64 編碼圖片）時的記憶體當機問題

## 2.0.33

- 原生二進位安裝現在啟動更快
- 正確解析 symlink，修正 `claude doctor` 會誤判 Homebrew 與 npm-global 安裝的問題
- 修正 `claude mcp serve` 會暴露 outputSchemas 不相容工具的問題

## 2.0.32

- 根據社群回饋，取消將輸出風格標記為已棄用
- 新增 `companyAnnouncements` 設定，用於在啟動時顯示公告
- 修正 PostToolUse hook 執行期間 hook 進度訊息未正確更新的問題

## 2.0.31

- Windows：原生安裝改用 shift+tab 作為模式切換快捷鍵，而非 alt+m
- Vertex：為支援的模型新增 Web Search 支援
- VSCode：新增 respectGitIgnore 設定，可在檔案搜尋中包含被 .gitignore 忽略的檔案（預設為 true）
- 修正子代理與 MCP 伺服器相關錯誤："Tool names must be unique"
- 讓 `/compact` 遵循既有 compact 邊界，修正其因 `prompt_too_long` 而失敗的問題
- 修正解除安裝外掛時未移除外掛的錯誤

## 2.0.30

- 當 macOS 鑰匙圈被鎖定而遇到 API 金鑰錯誤時，新增提示可執行 `security unlock-keychain`
- 新增 sandbox 設定 `allowUnsandboxedCommands`，可在政策層級停用 dangerouslyDisableSandbox 的逃生門
- 在自訂 agent 定義中新增 `disallowedTools` 欄位，用於明確封鎖工具
- 新增基於提示詞（prompt-based）的 stop hooks
- VSCode：新增 respectGitIgnore 設定，可在檔案搜尋中包含被 .gitignore 忽略的檔案（預設為 true）
- 在原生版本啟用 SSE MCP 伺服器
- 將輸出風格標記為已棄用。請查看 `/output-style` 的選項，並改用 --system-prompt-file、--system-prompt、--append-system-prompt、CLAUDE.md 或外掛
- 移除自訂 ripgrep 設定支援，解決 Search 無結果與設定探索失敗的問題
- 修正 Explore agent 在探索程式碼庫時會產生不必要的 .md 調查檔案
- 修正 `/context` 有時會失敗並顯示 "max_tokens must be greater than thinking.budget_tokens" 的錯誤
- 修正 `--mcp-config` 旗標可正確覆寫檔案型 MCP 設定
- 修正工作階段權限被儲存到本機設定的錯誤
- 修正子代理無法使用 MCP 工具的問題
- 修正使用 --dangerously-skip-permissions 時 hooks 與外掛不會執行的問題
- 修正使用方向鍵瀏覽 typeahead 建議時的延遲
- VSCode：恢復輸入區底部的選取指示器，顯示目前檔案或程式碼選取狀態

## 2.0.28

- 計畫模式：推出新的 Plan 子代理
- 子代理：Claude 現在可選擇續接子代理
- 子代理：Claude 現在可動態選擇子代理使用的模型
- SDK：新增 --max-budget-usd 旗標
- 自訂斜線指令、子代理與輸出風格的探索不再遵循 .gitignore
- 避免 `/terminal-setup` 在 VS Code 中對 `Shift + Enter` 加上反斜線
- Git 型外掛與市集新增支援分支與標籤，可用 fragment 語法（例如：`owner/repo#branch`）
- 修正從家目錄啟動時，macOS 權限提示會在初次啟動就出現的錯誤
- 其他多項錯誤修正

## 2.0.27

- 權限提示全新 UI
- 在工作階段續接畫面新增目前分支的篩選與搜尋，方便導覽
- 修正 @ 提及目錄導致 "No assistant message found" 錯誤的問題
- VSCode 擴充套件：新增設定，可在檔案搜尋中包含被 .gitignore 忽略的檔案
- VSCode 擴充套件：修正無關的 'Warmup' 對話，以及設定偶爾被重設為預設值的問題

## 2.0.25

- 移除舊版 SDK 入口點。請遷移至 @anthropic-ai/claude-agent-sdk 以取得後續 SDK 更新：https://platform.claude.com/docs/en/agent-sdk/migration-guide

## 2.0.24

- 修正指定 --setting-sources 'project' 時，專案層級 skills 未載入的錯誤
- Claude Code Web：支援 Web -> CLI teleport
- Sandbox：在 Linux 與 Mac 上釋出 BashTool 的 sandbox 模式
- Bedrock：當需要驗證時顯示 awsAuthRefresh 輸出

## 2.0.22

- 修正瀏覽斜線指令時內容版面位移的問題
- IDE：新增開關，可啟用／停用思考模式
- 修正並行工具呼叫導致重複權限提示的錯誤
- 新增支援由企業管理的 MCP 允許清單與拒絕清單

## 2.0.21

- 支援 MCP 工具回應中的 `structuredContent` 欄位
- 新增互動式提問工具
- Claude 在計畫模式中現在會更常向你提問
- 為 Pro 使用者新增 Haiku 4.5 模型選項
- 修正佇列指令無法存取前一則訊息輸出的問題

## 2.0.20

- 新增支援 Claude Skills

## 2.0.19

- 自動將長時間執行的 bash 指令轉到背景，而不是直接終止；可用 BASH_DEFAULT_TIMEOUT_MS 自訂
- 修正列印模式下不必要呼叫 Haiku 的錯誤

## 2.0.17

- 在模型選擇器新增 Haiku 4.5！
- Haiku 4.5 在計畫模式會自動使用 Sonnet、執行時使用 Haiku（也就是預設為 SonnetPlan）
- 第三方供應商（Bedrock 與 Vertex）尚未自動升級；可透過設定 `ANTHROPIC_DEFAULT_HAIKU_MODEL` 手動升級
- 推出 Explore 子代理：由 Haiku 驅動，可高效率搜尋你的程式碼庫以節省上下文！
- OTEL：支援 HTTP_PROXY 與 HTTPS_PROXY
- `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` 現在會停用發行說明抓取

## 2.0.15

- 修正續接（resume）時的錯誤：先前建立的檔案在寫入前需要再次讀取
- 修正 `-p` 模式的錯誤：@ 提及的檔案在寫入前需要再次讀取

## 2.0.14

- 修正透過 @ 提及 MCP 伺服器以切換啟用／停用的功能
- 改善 bash 指令包含行內 env 變數時的權限檢查
- 修正 ultrathink 與思考模式切換
- 減少不必要的登入
- 補充 --system-prompt 文件
- 多項渲染改進
- 外掛 UI 微調

## 2.0.13

- 修正 `/plugin` 在原生版本無法運作的問題

## 2.0.12

- **外掛系統正式推出**：可從市集以自訂指令、代理、hooks 與 MCP 伺服器擴充 Claude Code
- 以 `/plugin install`、`/plugin enable/disable`、`/plugin marketplace` 管理外掛
- 透過 `extraKnownMarketplaces` 支援儲存庫層級的外掛設定，方便團隊協作
- 使用 `/plugin validate` 驗證外掛結構與設定
- 外掛公告文章：https://www.anthropic.com/news/claude-code-plugins
- 外掛文件：https://code.claude.com/docs/en/plugins
- 透過 `/doctor` 提供完整的錯誤訊息與診斷
- 避免 `/model` 選擇器閃爍
- 改善 `/help`
- 在 `/resume` 摘要中避免提及 hooks
- `/config` 的 "verbose" 設定變更現在可跨工作階段保留

## 2.0.11

- 將系統提示詞縮小 1.4k tokens
- IDE：修正鍵盤快捷鍵與焦點問題，讓互動更順暢
- 修正 Opus 回退（fallback）的速率限制錯誤被誤顯示的問題
- 修正 /add-dir 指令選到錯誤預設分頁的問題

## 2.0.10

- 重寫終端機渲染器，帶來更順暢的 UI
- 可透過 @ 提及或在 /mcp 中啟用／停用 MCP 伺服器
- bash 模式新增 shell 指令的 Tab 自動補完
- PreToolUse hooks 現在可修改工具輸入
- 按 Ctrl-G 可使用系統設定的文字編輯器編輯提示詞
- 修正 bash 權限檢查在指令包含環境變數時的問題

## 2.0.9

- 修正 bash 背景執行停止運作的回歸問題

## 2.0.8

- 將 Bedrock 預設 Sonnet 模型更新為 `global.anthropic.claude-sonnet-4-5-20250929-v1:0`
- IDE：在聊天中新增支援拖放檔案與資料夾
- /context：修正思考區塊的計數
- 改善在深色終端機上使用淺色主題時的訊息渲染
- 移除已棄用的 .claude.json 設定選項 allowedTools、ignorePatterns、env 與 todoFeatureEnabled（請改在 settings.json 設定）

## 2.0.5

- IDE：修正使用 Enter 與 Tab 時，IME（輸入法）導致非預期提交訊息的問題
- IDE：在登入畫面新增 "Open in Terminal" 連結
- 修正未處理 OAuth 過期導致的 401 API 錯誤
- SDK：新增 SDKUserMessageReplay.isReplay，避免重複訊息

## 2.0.1

- 略過 Bedrock 與 Vertex 的 Sonnet 4.5 預設模型設定變更
- 多項錯誤修正與呈現改善

## 2.0.0

- 全新的原生 VS Code 擴充套件
- 全應用程式 UI 全面換新
- 使用 /rewind 回溯對話以復原程式碼變更
- 使用 /usage 指令查看方案限制
- 按 Tab 切換思考模式（跨工作階段保留）
- 使用 Ctrl-R 搜尋歷史紀錄
- 移除尚未發佈的 claude config 指令
- Hooks：減少 PostToolUse 出現「找到沒有 'tool_result' 區塊的 'tool_use' id」的錯誤
- SDK：Claude Code SDK 現已更名為 Claude Agent SDK
- 使用 `--agents` 旗標動態加入子代理

## 1.0.126

- 在 Bedrock 與 Vertex 上啟用 /context 指令
- 為基於 HTTP 的 OpenTelemetry exporter 新增 mTLS 支援

## 1.0.124

- 將 `CLAUDE_BASH_NO_LOGIN` 環境變數設為 1 或 true，可讓 BashTool 略過 login shell
- 修正 Bedrock 與 Vertex 環境變數：不再把所有字串都判定為 truthy
- 當權限被拒絕時，不再告知 Claude 允許使用的工具清單
- 修正 Bash 工具權限檢查的安全性漏洞
- 改善 VSCode 擴充套件在大型檔案下的效能

## 1.0.123

- Bash 權限規則在比對時，現在支援輸出重新導向（例如：`Bash(python:*)` 可比對 `python script.py > output.txt`）
- 修正像 "don't think" 這類否定語句會觸發思考模式的問題
- 修正 token 串流期間渲染效能逐步下降的問題
- 新增 SlashCommand 工具，讓 Claude 能呼叫你的斜線指令：https://code.claude.com/docs/en/slash-commands#SlashCommand-tool
- 強化 BashTool 的環境快照記錄
- 修正無頭模式續接對話時，有時會不必要啟用思考模式的錯誤
- 將 --debug 記錄遷移至檔案，方便 tail 與篩選

## 1.0.120

- 修正打字輸入延遲問題，特別是在大型提示詞下更明顯
- 改善 VSCode 擴充套件指令登錄與工作階段對話框的使用體驗
- 強化工作階段對話框的反應速度與視覺回饋
- 移除 worktree 支援檢查，修正 IDE 相容性問題
- 修正安全性漏洞：Bash 工具的權限檢查可能被前綴比對（prefix matching）繞過

## 1.0.119

- 修正 Windows 問題：進入互動模式時行程畫面看起來會卡住
- 透過 headersHelper 設定新增支援 MCP 伺服器動態標頭
- 修正在無頭（headless）工作階段中思考模式無法運作的問題
- 修正斜線指令：現在會正確更新允許工具清單，而不是直接取代

## 1.0.117

- 新增 Ctrl-R 歷史搜尋，可像 bash/zsh 一樣回想先前指令
- 修正打字輸入延遲問題，特別是在 Windows 上
- 在 acceptEdits 模式中，將 sed 指令加入自動允許清單
- 修正 Windows PATH 比對：磁碟代號改為不區分大小寫
- 在 /add-dir 輸出中新增權限管理提示

## 1.0.115

- 透過更佳的視覺效果改善思考模式顯示
- 在提示詞中輸入 /t 可暫時停用思考模式
- 改善 glob 與 grep 工具的路徑驗證
- 針對 post-tool hooks 顯示精簡輸出，減少視覺雜訊
- 修正載入狀態完成時的視覺回饋
- 改善權限請求對話框的 UI 一致性

## 1.0.113

- 將互動模式的管線輸入（piped input）標記為已棄用
- 將切換 transcript 的快捷鍵由 Ctrl+R 改為 Ctrl+O

## 1.0.112

- Transcript 模式（Ctrl+R）：新增顯示產生每則助理訊息所使用的模型
- 修正部分 Claude Max 使用者被錯誤辨識為 Claude Pro 使用者的問題
- Hooks：SessionEnd hooks 新增 systemMessage 支援
- 新增 `spinnerTipsEnabled` 設定，可停用 spinner 提示
- IDE：多項改進與錯誤修正

## 1.0.111

- /model 現在會驗證提供的模型名稱
- 修正因 shell 語法解析不正確而導致 Bash 工具當機的問題

## 1.0.110

- /terminal-setup 指令現在支援 WezTerm
- MCP：OAuth 權杖現在會在到期前主動重新整理
- 修正背景 Bash 行程的可靠性問題

## 1.0.109

- SDK：透過 `--include-partial-messages` CLI 旗標新增支援部分訊息（partial message）串流

## 1.0.106

- Windows：修正路徑權限比對，統一使用 POSIX 格式（例如：`Read(//c/Users/...)`）

## 1.0.97

- Settings：/doctor 現在會驗證權限規則語法並提供修正建議

## 1.0.94

- Vertex：為支援的模型新增 global endpoint 支援
- /memory 指令現在允許直接編輯所有已匯入的記憶檔案
- SDK：新增以 callback 形式提供自訂工具
- 新增 /todos 指令，可列出目前的待辦事項

## 1.0.93

- Windows：新增 alt + v 快捷鍵，可從剪貼簿貼上圖片
- 支援 NO_PROXY 環境變數，可對指定主機名稱與 IP 略過 proxy

## 1.0.90

- Settings 檔案變更會立即生效——不需要重新啟動

## 1.0.88

- 修正導致出現 "OAuth authentication is currently not supported" 的問題
- 狀態列輸入現在包含 `exceeds_200k_tokens`
- 修正 /cost 的用量追蹤不正確問題
- 新增 `ANTHROPIC_DEFAULT_SONNET_MODEL` 與 `ANTHROPIC_DEFAULT_OPUS_MODEL`，用於控制模型別名 opusplan、opus 與 sonnet
- Bedrock：將預設 Sonnet 模型更新為 Sonnet 4

## 1.0.86

- 新增 /context，協助使用者自助除錯上下文問題
- SDK：所有 SDK 訊息新增 UUID 支援
- SDK：新增 `--replay-user-messages`，可將使用者訊息重新輸出到 stdout

## 1.0.85

- 狀態列輸入現在包含工作階段成本資訊
- Hooks：新增 SessionEnd hook

## 1.0.84

- 修正網路不穩時 tool_use/tool_result id 不一致的錯誤
- 修正 Claude 在收尾任務時有時會忽略即時引導（real-time steering）的問題
- @ 提及：將 ~/.claude/* 檔案加入建議清單，讓編輯 agent、輸出風格與斜線指令更容易
- 預設使用內建 ripgrep；如要停用此行為，請設定 USE_BUILTIN_RIPGREP=0

## 1.0.83

- @ 提及：支援路徑包含空白的檔案
- 新增閃爍旋轉指示器

## 1.0.82

- SDK：新增支援取消請求
- SDK：新增 additionalDirectories 選項以搜尋自訂路徑，並改善斜線指令處理
- Settings：驗證機制可防止 .claude/settings.json 出現無效欄位
- MCP：改善工具名稱的一致性
- Bash：修正 Claude 嘗試自動讀取大型檔案時可能當機的問題

## 1.0.81

- 發布輸出風格（output styles），包含新的內建教學風格 "Explanatory" 與 "Learning"。文件：https://code.claude.com/docs/en/output-styles
- Agents：修正當 agent 檔案無法解析時，自訂 agent 無法載入的問題

## 1.0.80

- UI 改進：修正自訂子代理色彩的文字對比與旋轉指示器渲染問題

## 1.0.77

- Bash 工具：修正 heredoc 與多行字串的逸出處理，並改善 stderr 重新導向處理
- SDK：新增工作階段（session）支援與權限拒絕追蹤
- 修正對話摘要時的 token 限制錯誤
- Opus 計畫模式：在 `/model` 新增設定，讓 Opus 僅在計畫模式下執行，其餘使用 Sonnet

## 1.0.73

- MCP：支援多個設定檔，可使用 `--mcp-config file1.json file2.json`
- MCP：按 Esc 可取消 OAuth 驗證流程
- Bash：改善指令驗證並減少誤判的安全警告
- UI：強化旋轉指示器動畫與狀態列視覺層級
- Linux：新增支援 Alpine 與基於 musl 的發行版（需另外安裝 ripgrep）

## 1.0.72

- 詢問權限：使用 /permissions 設定讓 Claude Code 在使用特定工具前一律先詢問確認

## 1.0.71

- 背景指令：（Ctrl-b）可將任何 Bash 指令放到背景執行，讓 Claude 能繼續工作（很適合跑開發伺服器、tail log 等）
- 可自訂狀態列：使用 /statusline 將你的終端機提示符（prompt）加入 Claude Code

## 1.0.70

- 效能：最佳化訊息渲染，在大型上下文下有更好的效能
- Windows：修正原生檔案搜尋、ripgrep 與子代理功能
- 新增支援在斜線指令參數中使用 @ 提及

## 1.0.69

- 將 Opus 升級至 4.1 版

## 1.0.68

- 修正像 `/pr-comments` 等特定指令使用了不正確的模型名稱
- Windows：改善允許／拒絕工具與專案信任的權限檢查。這可能會在 `.claude.json` 建立新的專案項目——如有需要，請手動合併 history 欄位。
- Windows：改善子行程（sub-process）啟動，避免執行 pnpm 等指令時出現 "No such file or directory"
- 強化 /doctor 指令：加入 CLAUDE.md 與 MCP 工具上下文，方便自助除錯
- SDK：新增 canUseTool callback 支援，用於工具確認
- 新增 `disableAllHooks` 設定
- 改善大型儲存庫中的檔案建議效能

## 1.0.65

- IDE：修正連線穩定性問題，並改善診斷資訊的錯誤處理
- Windows：修正沒有 .bashrc 檔案的使用者之 Shell 環境設定

## 1.0.64

- Agents：新增模型自訂支援——現在可指定代理要使用的模型
- Agents：修正非預期存取 recursive agent tool 的問題
- Hooks：在 hook JSON 輸出中新增 systemMessage 欄位，用於顯示警告與上下文
- SDK：修正多輪對話中的使用者輸入追蹤
- 檔案搜尋與 @ 提及建議新增包含隱藏檔

## 1.0.63

- Windows：修正檔案搜尋、@agent 提及與自訂斜線指令功能

## 1.0.62

- 自訂代理新增支援 @ 提及與自動完成：使用 @<your-custom-agent> 呼叫
- Hooks：新增 SessionStart hook，用於新工作階段初始化
- /add-dir 指令現在支援目錄路徑的自動完成
- 改善網路連線檢查的可靠性

## 1.0.61

- Transcript 模式（Ctrl+R）：將 Esc 改為退出 Transcript 模式，而非中斷
- Settings：新增 `--settings` 旗標，可從 JSON 檔載入設定
- Settings：修正設定檔路徑為符號連結（symlink）時的解析問題
- OTEL：修正驗證變更後回報到錯誤組織的問題
- 斜線指令：修正 allowed-tools 搭配 Bash 時的權限檢查
- IDE：macOS 的 VSCode 新增支援使用 ⌘+V 貼上圖片
- IDE：新增 `CLAUDE_CODE_AUTO_CONNECT_IDE=false`，可停用 IDE 自動連線
- 新增 `CLAUDE_CODE_SHELL_PREFIX`，用於包裝 Claude 與使用者提供、由 Claude Code 執行的 shell 指令

## 1.0.60

- 現在可以建立自訂子代理（subagent）來處理特定任務！執行 /agents 即可開始

## 1.0.59

- SDK：新增工具確認支援，可透過 canUseTool callback
- SDK：允許為衍生（spawned）行程指定 env
- Hooks：將 PermissionDecision（包含 "ask"）暴露給 hooks
- Hooks：UserPromptSubmit 現在在進階 JSON 輸出中支援 additionalContext
- 修正部分 Max 使用者指定 Opus 時仍會回退（fallback）到 Sonnet 的問題

## 1.0.58

- 新增支援讀取 PDF
- MCP：改善 'claude mcp list' 中的伺服器健康狀態顯示
- Hooks：為 hook 指令新增 CLAUDE_PROJECT_DIR 環境變數

## 1.0.57

- 新增支援在斜線指令中指定模型
- 改善權限訊息，協助 Claude 理解允許使用的工具
- 修正：終端機換行處理時移除 bash 輸出末尾的換行字元

## 1.0.56

- Windows：在支援終端機 VT 模式的 Node.js 版本上，啟用 shift+tab 用於模式切換
- 修正 WSL 的 IDE 偵測問題
- 修正 awsRefreshHelper 對 .aws 目錄的變更未被偵測到的問題

## 1.0.55

- 釐清 Opus 4 與 Sonnet 4 模型的知識截止日期（knowledge cutoff）
- Windows：修正 Ctrl+Z 當機
- SDK：新增可擷取錯誤記錄（error logging）的能力
- 新增 --system-prompt-file 選項，可在列印模式下覆寫系統提示詞

## 1.0.54

- Hooks：新增 UserPromptSubmit hook，並在 hook 輸入中加入目前工作目錄
- 自訂斜線指令：在 frontmatter 新增 argument-hint
- Windows：OAuth 使用 45454 連接埠，並正確組合瀏覽器 URL
- Windows：模式切換改用 alt + m，且計畫模式可正確渲染
- Shell：改用記憶體中的 shell 快照，以修正檔案相關錯誤

## 1.0.53

- 將 @ 提及檔案的截斷上限由 100 行提高到 2000 行
- 新增用於 AWS 權杖更新的輔助腳本設定：awsAuthRefresh（用於 aws sso login 等前景操作）與 awsCredentialExport（用於回傳類 STS 回應的背景操作）。

## 1.0.52

- 新增支援 MCP 伺服器指令（instructions）

## 1.0.51

- 新增支援原生 Windows（需要 Git for Windows）
- 新增支援透過環境變數 AWS_BEARER_TOKEN_BEDROCK 使用 Bedrock API 金鑰
- Settings：/doctor 現在可協助你辨識並修正無效的設定檔
- `--append-system-prompt` 現在可用於互動模式，不再僅限於 --print/-p
- 將 auto-compact 警告門檻由 60% 提高至 80%
- 修正 shell 快照在處理含空白的使用者目錄時的問題
- OTEL resource 現在包含 os.type、os.version、host.arch，以及 wsl.version（若在 Windows Subsystem for Linux 上執行）
- 自訂斜線指令：修正子目錄中的使用者層級指令
- 計畫模式：修正子任務遭拒的計畫會被丟棄的問題

## 1.0.48

- 修正 v1.0.45 的錯誤：應用程式有時會在啟動時卡住
- Bash 工具新增進度訊息，依據指令輸出的最後 5 行顯示
- MCP 伺服器設定新增支援變數展開（variable expansion）
- 將 shell 快照由 /tmp 移至 ~/.claude，提升 Bash 工具呼叫的可靠性
- 改善 Claude Code 在 WSL 執行時的 IDE 擴充套件路徑處理
- Hooks：新增 PreCompact hook
- Vim 模式：新增 c、f/F、t/T

## 1.0.45

- 重新設計 Search（Grep）工具，加入新的工具輸入參數與功能
- 對 notebook 檔停用 IDE diff，修正 "Timeout waiting after 1000ms" 錯誤
- 強制採用原子寫入（atomic writes），修正設定檔損毀問題
- 將提示詞輸入的復原更新為 Ctrl+\_，避免破壞既有 Ctrl+U 行為，並與 zsh 的復原快捷鍵一致
- Stop Hooks：修正 /clear 後的逐字稿路徑，並修正當迴圈以工具呼叫結束時的觸發問題
- 自訂斜線指令：依子目錄恢復指令名稱命名空間。例如：.claude/commands/frontend/component.md 現為 /frontend:component，而非 /component。

## 1.0.44

- 新增 /export 指令，讓你快速匯出對話以便分享
- MCP：現在支援 resource_link 工具結果
- MCP：工具註解與工具標題現在會顯示於 /mcp 檢視中
- 將 Ctrl+Z 改為暫停 Claude Code；可執行 `fg` 繼續。提示詞輸入的復原改為 Ctrl+U。

## 1.0.43

- 修正主題選擇器過度儲存的錯誤
- Hooks：新增 EPIPE 系統錯誤處理

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
