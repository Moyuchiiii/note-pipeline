---
name: article-gen
description: note記事を生成する。記事タイプに応じて調査→構成→執筆を自動実行。
user-invocable: true
allowed-tools: WebSearch, WebFetch, Write, Read, Glob, Grep, Bash
---

# /article-gen スキル

note.com向けの記事を生成するスキル。

## 起動時の入力

ユーザーに以下を確認する:
1. 記事タイプ（jituroku / sokuho / knowhow）
2. トピック（何について書くか）
3. ソース素材（メモ・URL・ファイルパス等。あれば）

## 実行フロー

### Phase 1: コンテキスト読み込み
- `context/existing-articles.md` を読み、過去記事との重複を確認
- `context/voice-samples.md` を読み、文体を把握
- `context/profile.md` を読み、プロフィールとの整合性を確認

### Phase 2: 調査（sokuho・knowhowの場合）
- WebSearchで関連情報を最低3ソースから収集
- 競合のnote記事を調査（同テーマで既に書かれている記事）
- 差別化ポイントを特定

### Phase 3: 構成設計
- 記事の章立てを作成
- 各章のポイントを1行メモで整理
- 有料ラインの位置を提案（有料記事の場合）

### Phase 4: 執筆
- CLAUDE.mdのルールに従って執筆
- 文体は `context/voice-samples.md` に合わせる
- 以下を禁止:
  - 「〜と言えるでしょう」「〜ではないでしょうか」等のAI定型文
  - 過度な装飾（絵文字の多用、箇条書きの羅列）
  - 括弧（）での補足説明の多用
- 以下を意識:
  - 具体的な数字・体験を入れる
  - 「自分ごと」として読める書き方
  - 次の記事への導線（最後に予告）

### Phase 5: 保存
- `drafts/{記事タイプ}/draft_{日付}_{トピック}.md` に保存
- ファイル冒頭にメタ情報を記載:
  ```
  ---
  title: 記事タイトル
  type: jituroku/sokuho/knowhow
  price: 0/500/800/1000/1500/2000
  tags: [タグ1, タグ2, ...]
  created: YYYY-MM-DD
  status: draft
  ---
  ```
