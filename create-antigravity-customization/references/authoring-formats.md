# 成果物のフォーマット仕様

Antigravity公式仕様、Agent Skills公式標準、MCP公式仕様などに基づく各成果物の定義形式と配置場所。
このファイルがリポジトリ内の唯一の正本です。

確認日: 2026-08-08
確認元: antigravity.google、Agent Skills標準（agentskills.io）、Antigravity CLI公式ブログ

## 共通する注意事項
- **未確認事項**: 公式資料で確認できない内容は「未確認」と明記し、断定しません。
- **配置先の正本**: 本ページに記載されたパスのみを正本として使用します。

---

## Agent Skill

### 基本情報

| 項目 | 内容 | 根拠 |
|------|------|------|
| 公式名称 | Agent Skill | Antigravity公式仕様 |
| Globalの配置場所 | `~/.gemini/config/skills/<skill-name>/SKILL.md` | Antigravity公式仕様 |
| Workspaceの配置場所 | `<workspace-root>/.agents/skills/<skill-name>/SKILL.md` | Antigravity公式仕様 |
| 必須ファイル | `SKILL.md` | Antigravity公式仕様 |
| 必須frontmatter | `name`, `description` | Antigravity公式仕様 |
| 任意frontmatter | なし（Agent Skills標準では拡張フィールドが存在するがAntigravityでの対応状況は未確認） | Agent Skills公式標準 / 未確認 |
| 各フィールドのデフォルト値 | なし | Antigravity公式仕様 |
| 呼び出し方法 | Progressive disclosure（Discovery → Activation → Execution） | Agent Skills公式標準 |
| 文字数などの制限 | 500行以内を推奨 | リポジトリ独自方針 |

### 制約と特記事項
- `name`はAntigravity公式仕様では必須です。小文字英数字とハイフンのみを使用し、ディレクトリ名と一致させます。
- `description`は第三者視点で記述します（Agent Skills公式標準）。

---

## Custom Subagent

### 基本情報

| 項目 | 内容 | 根拠 |
|------|------|------|
| 公式名称 | Custom Subagent (Custom Agent) | Antigravity公式仕様 |
| Globalの配置場所 | `~/.gemini/config/agents/<name>.md` または `<name>/agent.md` | Antigravity公式仕様 |
| Workspaceの配置場所 | `<workspace-root>/.agents/agents/<name>.md` または `<name>/agent.md` | Antigravity公式仕様 |
| 必須ファイル | `.md` ファイル | Antigravity公式仕様 |
| 必須frontmatter | `name`, `description` | Antigravity公式仕様 |
| 任意frontmatter | `tools`, `mainAgent`, `subagent`, `model`, `commandExecutionPolicy`, `mcpServers`, `skills`, `plugins` | Antigravity公式仕様 |
| 各フィールドのデフォルト値 | `subagent`: `true`, `mainAgent`: `true`, `model`: `inherit`, `tools`: `[]`, `skills`: `[]`, `mcpServers`: `[]` | Antigravity公式仕様 |
| 呼び出し方法 | 親Agentが`invoke_subagent`ツールで呼び出す | 実環境で確認した動作 |
| 文字数などの制限 | 制限なし | Antigravity公式仕様 |

### 未確認事項
- `inheritMcp`, `hidden`, `@agent-name`などのフィールドや呼び出し方法は公式資料で確認できないため未確認。
- `tools`に指定可能な正式なツール名は実環境に依存するため、確認済みのツール（`invoke_subagent`等）以外は未確認。

---

## Rule

### 基本情報

| 項目 | 内容 | 根拠 |
|------|------|------|
| 公式名称 | Rule | Antigravity公式仕様 |
| Globalの配置場所 | `~/.gemini/GEMINI.md` | Antigravity公式仕様 |
| Workspaceの配置場所 | `<workspace-root>/.agents/rules/` （※後方互換で `.agent/rules/` もサポート） | Antigravity公式仕様 |
| 必須ファイル | Markdownファイル (`.md`) | Antigravity公式仕様 |
| 必須frontmatter | なし（プレーンMarkdown） | Antigravity公式仕様 |
| 任意frontmatter | 未確認 | 未確認 |
| 各フィールドのデフォルト値 | なし | Antigravity公式仕様 |
| 呼び出し方法 | Manual（@メンション）, Always On, Model Decision, Glob のいずれか | Antigravity公式仕様 |
| 文字数などの制限 | 各12,000文字以内 | Antigravity公式仕様 |

### 特記事項
- ファイル内で `@filename` を用いて他ファイルを参照可能です。
- 旧仕様の `AGENTS.md` 前提の記述は現在の公式仕様に置き換わりました。

---

## Workflow

### 基本情報

| 項目 | 内容 | 根拠 |
|------|------|------|
| 公式名称 | Workflow | Antigravity公式仕様 |
| Globalの配置場所 | 公式資料では物理配置先を確認できないため、パスを断定しない。 | 未確認 |
| Workspaceの配置場所 | 公式資料では物理配置先を確認できないため、パスを断定しない。 | 未確認 |
| 必須ファイル | Markdownファイル (`.md`) | Antigravity公式仕様 |
| 必須frontmatter | 未確認 | 未確認 |
| 任意frontmatter | 未確認 | 未確認 |
| 各フィールドのデフォルト値 | 未確認 | 未確認 |
| 呼び出し方法 | スラッシュコマンド（例: `/workflow-name`）で実行 | Antigravity公式仕様 |
| 文字数などの制限 | 各12,000文字以内 | Antigravity公式仕様 |

### 特記事項
- Workflowは必ずAntigravityのCustomizations画面から作成するか、Agentに生成を依頼します。
- Workflow内から別のWorkflowをスラッシュコマンドで呼び出すことが可能です。

---

## MCP設定

### 基本情報

| 項目 | 内容 | 根拠 |
|------|------|------|
| 公式名称 | MCP Server Configuration | MCP公式仕様 |
| Globalの配置場所 | `~/.gemini/config/mcp_config.json` | Antigravity公式仕様 |
| Workspaceの配置場所 | `<workspace-root>/.agents/mcp_config.json` | Antigravity公式仕様 |
| 必須ファイル | JSONファイル | MCP公式仕様 |
| 必須frontmatter | 該当なし（JSON） | 該当なし |
| 任意frontmatter | 該当なし（JSON） | 該当なし |
| 各フィールドのデフォルト値 | なし | MCP公式仕様 |
| 呼び出し方法 | Agentが自動的に外部能力として利用 | Antigravity公式仕様 |
| 文字数などの制限 | 制限なし | MCP公式仕様 |

### 設定構造とフィールド
- ファイルは単一の `mcpServers` オブジェクトを持ちます。
- **トランスポート層（必須）**: stdio用 `command`（パス指定）、またはremote用 `serverUrl`。
- **オプション層**: `args`, `env`, `cwd`, `headers`, `authProviderType`（`google_credentials` 等）, `oauth`, `disabled`, `disabledTools`。
- レガシーな `url` や `httpUrl` は現在サポートされていません。
