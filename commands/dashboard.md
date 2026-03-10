---
description: 並列開発の状態をVantage Pointダッシュボードに表示。agent一覧、タスクボード、メッセージログの3ペイン。
---

# CC Nav Dashboard — 並列開発ダッシュボード

Vantage Point に並列開発の全体像を表示する。

## 手順

1. Vantage Point のキャンバスを開く（未開の場合）:
   ```
   mcp__vantage-point__open_canvas()
   ```

2. 左右ペインを有効化:
   ```
   mcp__vantage-point__toggle_pane(pane_id: "left", visible: true)
   mcp__vantage-point__toggle_pane(pane_id: "right", visible: true)
   ```

3. **左ペイン: Agent 一覧 + ステータス**

   `wire_sessions` と `wire_status` でデータを取得し表示:

   ```
   mcp__vantage-point__show(
     pane_id: "left",
     content_type: "html",
     title: "Agents",
     content: <Agent一覧HTML>
   )
   ```

4. **メインペイン: タスクボード**

   TaskList がある場合はそのタスクを、なければ `ccws ls` の情報を表示:

   ```
   mcp__vantage-point__show(
     pane_id: "main",
     content_type: "html",
     title: "Tasks",
     content: <タスクボードHTML>
   )
   ```

5. **右ペイン: メッセージログ**

   `wire_receive` で最新メッセージを取得し、ログ形式で表示:

   ```
   mcp__vantage-point__show(
     pane_id: "right",
     content_type: "html",
     title: "Messages",
     content: <メッセージログHTML>
   )
   ```

## 更新タイミング

- 初回表示: コマンド実行時
- 以降: lead agent が適宜更新
- 推奨: タスク完了報告を受けるたびに更新
