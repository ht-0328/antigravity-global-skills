# 成果物のフォーマット仕様

Antigravity公式仕様とAgent Skills標準に基づく各成果物の定義形式。

確認日: 2026-08-07
確認元: antigravity.google、Agent Skills標準（agentskills.io）、Antigravity CLI公式ブログ

## Agent Skill

### 基本情報

| 項目 | 内容 |
|------|------|
| 公式名称 | Agent Skill |
| 必須ファイル | `SKILL.md` |
| frontmatter必須フィールド | `name`, `description` |
| frontmatter任意フィールド | なし（Agent Skills標準で定義される追加フィールドが今後拡張される可能性あり） |
| 読み込み方法 | Progressive disclosure（Discovery → Activation → Execution） |
| 推奨上限 | 500行（Agent Skills標準。超える場合は`references/`へ分離） |

### Workspace Skill

| 項目 | 内容 |
|------|------|
| 配置場所 | `<workspace-root>/.agents/skills/<skill-name>/SKILL.md` |
| スコープ | 特定ワークスペース内のみ |
| 制約 | プロジェクト固有のパスやルールに依存してよい |

### Global Skill

| 項目 | 内容 |
|------|------|
| 配置場所 | `~/.gemini/config/skills/<skill-name>/SKILL.md` |
| スコープ | すべてのワークスペースで使用可能 |
| 制約 | 特定プロジェクトのパスや認証情報をハードコードしない |

### frontmatterの例

```yaml
---
name: pr-description-writer
description: Pull Requestの説明文を自動生成するSkillです。git diffの内容を分析し、変更の概要、影響範囲、レビューポイントを含むPR説明文を作成します。PRを作成する際やdiffを確認した後に使用します。
---
```

### ディレクトリ構成

```text
<skill-name>/
├── SKILL.md          # 必須: frontmatter + 実行手順
├── references/       # 任意: 条件付きで読む詳細資料
├── scripts/          # 任意: 実行可能なヘルパースクリプト
└── assets/           # 任意: テンプレート、データファイル
```

### `name`の制約

- 小文字英数字とハイフンのみ（例: `code-review-helper`）
- 親ディレクトリ名と一致させる
- 根拠: Agent Skills標準

### `description`のガイドライン

- 第三者視点で記述する（「〜を分析する」「〜を生成する」）
- 「何をするか」と「いつ使うか」を含める
- 曖昧な「機能を作る」のような記述を避ける
- 根拠: Agent Skills標準の推奨

## Custom Agent / Custom Subagent

### 基本情報

| 項目 | 内容 |
|------|------|
| 公式名称 | Custom Agent / Custom Subagent |
| ファイル形式 | Markdown (`.md`) |
| frontmatter必須フィールド | `name`, `description` |
| 本文の役割 | System promptとして機能する |

### frontmatterフィールド一覧

| フィールド | 型 | 必須 | デフォルト | 説明 | 確認状況 |
|-----------|-----|------|-----------|------|----------|
| `name` | string | ✓ | - | 一意な識別子 | 公式仕様で確認 |
| `description` | string | ✓ | - | 親Agentが委譲判断に使う説明 | 公式仕様で確認 |
| `subagent` | boolean | | `true` | subagentとして呼び出し可能にするか | 公式仕様で確認 |
| `mainAgent` | boolean | | `true` | メインAgentとして機能するか | 公式仕様で確認 |
| `model` | string | | `inherit` | 使用するAIモデル | 公式仕様で確認 |
| `tools` | array | | `[]` | 使用可能なツール名のリスト | 公式仕様で確認 |
| `commandExecutionPolicy` | string | | - | コマンド実行のセキュリティポリシー | 公式仕様で確認（値の詳細は要確認） |
| `skills` | array | | `[]` | 依存するSkillのパス | 公式仕様で確認 |
| `mcpServers` | array | | `[]` | MCP server設定 | 公式仕様で確認 |
| `inheritMcp` | boolean | | - | 親AgentのMCP設定を継承するか | 公式仕様で確認 |
| `hidden` | boolean | | `false` | 選択メニューに表示しないか | 公式仕様で確認 |

### 配置場所

| スコープ | パス |
|----------|------|
| Workspace | `<workspace-root>/.agents/agents/<name>.md` |
| Workspace（ディレクトリ形式） | `<workspace-root>/.agents/agents/<name>/agent.md` |
| Global | `~/.gemini/config/agents/<name>.md` |
| Global（ディレクトリ形式） | `~/.gemini/config/agents/<name>/agent.md` |

### 重要な注意事項

- **SkillとSubagentは異なる成果物**: SubagentはSKILL.mdではなく独自の`.md`ファイルで定義する。`skills/`ディレクトリには配置しない。
- **toolsフィールド**: 実環境に存在する正確なツール名のみ記載する。推測しない。不明な場合は省略するか「未確認」と明記する。
- **本文 = system prompt**: frontmatter以降のMarkdown本文がsubagentのsystem promptとして使われる。

### 不明点

- `commandExecutionPolicy`の取りうる値の完全なリスト（`sandbox`以外の値は公式ドキュメントで要確認）
- `tools`フィールドに指定可能なツール名の完全なリスト（実環境依存のため、実行時に確認が必要）

## Rule

### 基本情報

| 項目 | 内容 |
|------|------|
| 公式名称 | Rule / Context File |
| ファイル名 | `AGENTS.md`（推奨）または`GEMINI.md` |
| frontmatter | なし（プレーンMarkdown） |
| 読み込み方法 | セッション開始時に自動読み込み |

### 配置場所

| スコープ | パス |
|----------|------|
| Workspace | `<workspace-root>/.agents/AGENTS.md` |
| Global | `~/.gemini/config/AGENTS.md` |

### 制約

- タスク手順ではなくAgentの振る舞いの制約を記述する
- Skillの手順と重複させない

## Workflow

### 基本情報

| 項目 | 内容 |
|------|------|
| 公式名称 | Workflow / Slash Command |
| ファイル形式 | Markdown (`.md`) |
| frontmatter必須フィールド | `description` |
| 呼び出し方法 | `/filename`（ファイル名がコマンド名になる） |
| 文字数制限 | 12,000文字 |

### 配置場所

| スコープ | パス |
|----------|------|
| Workspace | `<workspace-root>/.agents/workflows/<name>.md` |

### 注意事項

- `description`をシンボル（`[`、`]`等）で始めない（登録に失敗する）
- ファイル名 = スラッシュコマンド名のため、名前を慎重に選ぶ

## MCP設定

### 基本情報

| 項目 | 内容 |
|------|------|
| プロトコル仕様 | MCP 2026-07-28（Stateless） |
| 設定場所 | `.gemini/settings.json`の`mcpServers`フィールド |
| またはSubagent内 | frontmatterの`mcpServers`フィールド |

### 不明点

- Workspace単位のMCP設定ファイルパスの詳細（settings.json内で設定、実装依存）

## 公式仕様とAgent Skills標準の差異

| 項目 | Antigravity公式 | Agent Skills標準 | 採用方針 |
|------|----------------|-----------------|----------|
| Skillの配置パス | `~/.gemini/config/skills/`、`.agents/skills/` | `~/.agents/skills/`、`.agents/skills/` | Antigravity公式を優先 |
| `SKILL.md`推奨上限 | 明示なし | 500行 | 互換性を高めるためのリポジトリ方針として500行を目安にする |
| `description`の視点 | 明示なし | 第三者視点を推奨 | 互換性を高めるためのリポジトリ方針として第三者視点を採用 |
| SubagentのファイルパスにSKILL.mdを使うか | 使わない（独自の`.md`ファイル） | Subagentの概念なし | Antigravity公式を優先 |
