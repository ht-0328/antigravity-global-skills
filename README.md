# Antigravity Global Skills

Antigravity IDEで使用するカスタマイズ機能のコレクションです。
ここに配置されたSkillは、すべてのワークスペースで自動的に検出・利用可能になります。

## 収録スキル

| スキル名 | 説明 |
|----------|------|
| [create-antigravity-customization](create-antigravity-customization/) | Antigravityのカスタマイズ機能（Skill、Custom Subagent、Rule、Workflow、MCP設定、補助スクリプト）を新規作成または改善する。ユーザーが種類を理解していなくても、目的から適切な成果物を判定する。 |

## ディレクトリ構成

```text
create-antigravity-customization/
├── SKILL.md                          # メインの指示ファイル（frontmatter + 手順）
├── references/                       # 条件付きで参照する詳細資料
│   ├── artifact-selection.md         # 成果物の選択基準（決定表）
│   ├── quality-criteria.md           # 品質基準（品質ゲート）
│   ├── authoring-formats.md          # 各成果物のフォーマット仕様
│   └── evaluation-cases.md           # 評価ケース
└── assets/                           # 出力テンプレート
    ├── skill-template.md             # Skillテンプレート
    ├── custom-subagent-template.md   # Custom Subagentテンプレート
    ├── rule-template.md              # Ruleテンプレート
    └── workflow-template.md          # Workflowテンプレート
```

## 使い方

チャットで以下のように話しかけると、対応するSkillが自動で起動します。

```text
「Skillを作りたい」
「Ruleを追加したい」
「Workflowを定義したい」
「既存Skillを改善したい」
「カスタマイズを整理したい」
```

種類が分からない場合も、目的を伝えるだけでAgentが適切な成果物を判定します。

```text
「コードレビューの手順を再利用できるようにしたい」
「コミット前にテストを必ず実行するようにしたい」
```

## Antigravityカスタマイズの配置場所

### Skill

| スコープ | パス |
|----------|------|
| Global | `~/.gemini/config/skills/<skill-name>/` |
| Workspace | `<workspace-root>/.agents/skills/<skill-name>/` |

### Custom Agent / Custom Subagent

| スコープ | パス |
|----------|------|
| Global | `~/.gemini/config/agents/<name>.md` |
| Workspace | `<workspace-root>/.agents/agents/<name>.md` |

SkillとCustom Subagentは異なる成果物です。Skillは`skills/`ディレクトリに`SKILL.md`として配置し、Custom Subagentは`agents/`ディレクトリに独自の`.md`ファイルとして配置します。

### Rule

| スコープ | パス |
|----------|------|
| Global | `~/.gemini/config/AGENTS.md` |
| Workspace | `<workspace-root>/.agents/AGENTS.md` |

### Workflow

| スコープ | パス |
|----------|------|
| Workspace | `<workspace-root>/.agents/workflows/<name>.md` |

## 新しいSkillを追加する

1. `~/.gemini/config/skills/`（Global）または`<workspace-root>/.agents/skills/`（Workspace）にディレクトリを作成する。
2. ディレクトリ内に`SKILL.md`を作成する（YAML frontmatterに`name`と`description`を記載）。
3. 必要に応じて`references/`、`scripts/`、`assets/`を追加する。

`create-antigravity-customization`スキルを使えば、対話形式でカスタマイズを作成できます。

## 旧名称からの移行

このスキルは`create-skill`から`create-antigravity-customization`に名称変更されました。

変更点:
- SkillとAgentの2択から、Skill / Custom Subagent / Rule / Workflow / MCP / Scriptの6種類に対応範囲を拡大
- AgentをSKILL.mdとして生成する誤りを修正（Custom Subagentは`agents/`ディレクトリに配置）
- 複数の事前承認ステップを廃止し、目的から直接判定して作成する方式に変更
- 品質基準による自己評価を追加

旧`create-skill`ディレクトリを参照しているカスタマイズがある場合は、パスを`create-antigravity-customization`に更新してください。

## ライセンス

Private use.
