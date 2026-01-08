# Claude Code

![](https://img.shields.io/badge/Node.js-18%2B-brightgreen?style=flat-square) [![npm]](https://www.npmjs.com/package/@anthropic-ai/claude-code)

[npm]: https://img.shields.io/npm/v/@anthropic-ai/claude-code.svg?style=flat-square

Claude Code 是一個智慧型程式設計工具，運作於您的終端機中，能夠理解您的程式碼庫，並透過自然語言指令協助您更快速地編寫程式——包含執行例行性任務、解釋複雜程式碼以及處理 git 工作流程。您可以在終端機、整合式開發環境 (IDE) 中使用，或在 GitHub 上標記 @claude。

**深入了解請參閱[官方文件](https://code.claude.com/docs/en/overview)**。

<img src="./demo.gif" />

## 開始使用

1. 安裝 Claude Code：

**MacOS/Linux：**
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Homebrew (MacOS)：**
```bash
brew install --cask claude-code
```

**Windows：**
```powershell
irm https://claude.ai/install.ps1 | iex
```

**NPM：**
```bash
npm install -g @anthropic-ai/claude-code
```

注意：若使用 NPM 安裝，您還需要安裝 [Node.js 18+](https://nodejs.org/en/download/)

2. 導航至您的專案目錄並執行 `claude`。

## 外掛程式

此儲存庫包含數個 Claude Code 外掛程式，透過自訂指令與代理程式擴充功能。詳細的外掛程式文件請參閱[外掛程式目錄](./plugins/README.md)。

## 回報錯誤

我們歡迎您的意見回饋。請使用 `/bug` 指令直接在 Claude Code 中回報問題，或提交 [GitHub issue](https://github.com/anthropics/claude-code/issues)。

## 在 Discord 上交流

加入 [Claude Developers Discord](https://anthropic.com/discord)，與其他使用 Claude Code 的開發者交流。取得協助、分享意見回饋，並與社群討論您的專案。

## 資料收集、使用與保留

當您使用 Claude Code 時，我們會收集意見回饋資料，包括使用資料（例如程式碼的接受或拒絕）、相關對話資料，以及透過 `/bug` 指令提交的使用者意見回饋。

### 我們如何使用您的資料

請參閱我們的[資料使用政策](https://code.claude.com/docs/en/data-usage)。

### 隱私保護措施

我們實施了多項保護措施來保護您的資料，包括對敏感資訊的有限保留期限、對使用者工作階段資料的存取限制，以及明確禁止將意見回饋用於模型訓練的政策。

如需完整詳情，請參閱我們的[商業服務條款](https://www.anthropic.com/legal/commercial-terms)與[隱私權政策](https://www.anthropic.com/legal/privacy)。
