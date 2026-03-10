---
description: 並列ワーカーの変更ファイルを比較し、マージコンフリクトの可能性を早期検知する。
---

# CC Nav Conflict Check — コンフリクト早期検知

## 手順

1. `ccws ls` でアクティブなワーカー一覧を取得

2. 各ワーカーの変更ファイル一覧を収集:
   ```bash
   # 各ワーカーディレクトリで
   cd $(ccws path <name>)
   git diff --name-only main...HEAD
   ```

3. 変更ファイルの重複を検出:
   - 2つ以上のワーカーが同じファイルを変更している場合 → 警告

4. 結果を表示:

   ```
   ## コンフリクト検知結果

   issue-42 x issue-43: 重複なし
   issue-42 x issue-44: 2ファイルが重複
     - src/auth/middleware.ts
     - src/auth/types.ts

   推奨: issue-44 のマージは issue-42 のマージ後に行ってください。
   ```

5. ccwire 登録済みなら、コンフリクトがある場合に該当ワーカーへ wire_send で警告

## 引数

なし（全ワーカーを自動チェック）

## 推奨利用タイミング

- Wave 切り替え前
- PR 作成前
- リーダーが `/ccnav:dashboard` を更新する際
