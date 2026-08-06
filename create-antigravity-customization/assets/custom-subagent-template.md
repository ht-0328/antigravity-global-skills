---
name: <subagent-name>
description: <親Agentが委譲判断に使う説明。このSubagentが何を専門とし、どのような依頼で呼び出されるべきかを記述する>
subagent: true
mainAgent: false
tools:
  - <実環境で確認した正確なツール名のみ記載する。不明な場合はこのフィールドを省略する。例: invoke_subagent>
---

# System Prompt

<以下がSubagentのsystem promptとして機能する。>

<このSubagentの役割と専門領域を定義する。>

## 手順

1. <親Agentから受け取ったタスクの処理手順を記述する。>
2. <完了時に親Agentへ返す内容を明記する。>

## 制約

<このSubagentが逸脱してはならない制約を記述する。なければこのセクションを削除する。>
