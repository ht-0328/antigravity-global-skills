# Antigravity Global Skills

このリポジトリは、Antigravity IDEで使用するカスタマイズ機能（主にGlobal Skill）の保存・管理用リポジトリです。
Google Antigravityから利用するには、公式仕様で定められたGlobal Skill配置先へ配置（コピー）する必要があります。
（リポジトリをcloneしただけでは自動検出されません。詳細は「導入方法」を参照してください。）

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
| Global | `~/.gemini/GEMINI.md` |
| Workspace | `<workspace-root>/.agents/rules/` |

### Workflow

| スコープ | パス |
|----------|------|
| 未確認 | 未確認（Customizations画面から作成してください） |

### MCP設定

| スコープ | パス |
|----------|------|
| Global | `~/.gemini/config/mcp_config.json` |
| Workspace | `<workspace-root>/.agents/mcp_config.json` |

## 導入方法

このリポジトリの構成は、保存用の一元管理用ディレクトリです。
Antigravityが実際にSkillとして読み込む公式の配置先へ、リポジトリ内のディレクトリをコピーする必要があります。
（※リポジトリを任意の場所へcloneしただけでは自動検出されません。また、シンボリックリンクを使用するとAntigravityから正常に読み込めない場合があるため注意してください。）

### Global Skillとして利用する方法

すべてのワークスペースで利用したい場合は、以下の公式配置先へコピーします。

```bash
mkdir -p ~/.gemini/config/skills/
cp -r /path/to/cloned/repo/create-antigravity-customization ~/.gemini/config/skills/
```

### Workspace Skillとして利用する方法

特定のワークスペースでのみ利用したい場合は、対象プロジェクトの以下の場所へコピーします。

```bash
mkdir -p .agents/skills/
cp -r /path/to/cloned/repo/create-antigravity-customization .agents/skills/
```

## 新しいカスタマイズを追加する

`create-antigravity-customization`スキルを使えば、対話形式でカスタマイズ（Skill、Custom Subagent、Rule、Workflow、MCP、Script）を作成できます。利用者は成果物種別を選ばなくてよい設計になっています。目的を伝えるだけでAgentが自動で判定します。

また、本Skillは安全性のため、`commit`、`push`、`deploy`といった不可逆・外部影響のある操作を自動実行しないよう制限されています。


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
