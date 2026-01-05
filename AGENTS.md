# AGENTS.md

このファイルは、AI coding agent のための運用マニュアルです（エージェント用 README）。
このリポジトリは “ほぼ空” から新規アプリを育てるため、特に「迷いやすいポイント」を明文化します。


## 最重要ルール（絶対）
- **Issue が仕様**：Issue の Acceptance Criteria を満たすこと以外に広げない。
- **Atomic**：関係ない変更を混ぜない。必要なら Issue 分割を提案する。
- **常に緑**：PR を出す前に build/test/lint を通す（可能な範囲で）。
- **秘密情報ゼロ**：トークン、鍵、接続文字列、個人情報は絶対にコミットしない。


## まず見るべきファイル
- `.github/copilot-instructions.md`（リポジトリ共通指示）
- `.github/ISSUE_TEMPLATE/copilot-task.yml`（Issueフォーム）
- `README.md`（セットアップ、実行、テスト手順の正）
- `.github/workflows/**`（CI。存在する場合はここが正）


## 標準コマンド（このリポの “共通インタフェース”）
このリポでは、スタックが何であっても「外から叩く共通コマンド」を揃えることを推奨します。

- 推奨：リポジトリ直下に `Makefile` を用意し、以下のターゲットを提供する
  - `make setup` : 依存導入 / restore
  - `make build` : ビルド / コンパイル
  - `make test`  : テスト実行
  - `make lint`  : lint 実行（存在する場合）
  - `make fmt`   : フォーマット（存在する場合）
  - `make run`   : ローカル起動
  - `make ci`    : build + lint + test（まとめ）

Makefile がまだ無い場合：
- ブートストラップの一部として Makefile を追加してよい（Issue が禁止していない限り）。
- 内部では、スタック標準のコマンド（例：npm / pnpm / poetry / dotnet 等）に委譲する形でよい。
- README.md に同じ内容を必ず書く（人間もエージェントも迷わない）。


## 作業手順（毎タスク共通）
1) Issue を読み、Acceptance Criteria をチェックリスト化する  
2) 対象ファイルを特定（無ければ新規作成）  
3) 実装（差分ノイズを増やさない）  
4) `make ci`（または README 記載の同等コマンド）を実行  
5) 必要ならテスト追加/更新  
6) README / docs 更新（手順や意思決定を残す）  
7) PR 本文に「何をしたか」「どう検証したか」「前提/仮定」を必ず書く


## ブートストラップフェーズ（アプリが無い/薄い状態）での指針
- 最初のゴールは “Hello World でもよいので動く最小構成” を作ること。
- 1PR で詰め込みすぎない：
  - PR1：最小アプリ + README（起動/テスト） + 共通コマンド（Makefile など）
  - PR2：CI（GitHub Actions）で build/test/lint
  - PR3：Lint/format/テスト増強 … のように分割

技術スタックの扱い：
- Issue に明記があるならそれに従う。
- Issue に明記がないなら、勝手に選定して作り込まない。
  - `docs/stack-decision.md` に 2〜3案＋理由を書いて提案し、意思決定を待つ。


## 境界（Boundaries）
### Never（禁止）
- secrets/鍵/トークン/個人情報をコミットする
- `LICENSE` を編集する
- `.github/ISSUE_TEMPLATE/**` を Issue 指示なしに編集する
- 生成物や vendor 相当（例：`node_modules/` など）をコミットする
- “依頼されていない” 大規模リファクタ、ファイル移動、整形祭りをする

### Ask first（PR で明示して判断を仰ぐ）
- 新規依存パッケージ追加（特に大きいもの）
- 公開API/外部I/F の破壊的変更
- DBスキーマや永続化形式の変更
- CI やセキュリティ設定（CORS/認証/権限）に関わる変更
- 大規模なディレクトリ再編

### Always（常にやる）
- 変更を最小化し、レビュー可能性を最大化する
- “なぜそうしたか” を README / docs / PR に残す
- 失敗時に原因が追えるエラーメッセージ/ログを残す（必要最小限）


## PR 指示
- タイトルは Issue の意図が分かる形にする（例：`[bootstrap] init: ...`）
- PR 本文に必ず含める：
  - Issue へのリンク
  - 変更要約
  - 実行した検証コマンド（例：`make ci`）
  - 手動確認手順（必要なら）
  - 前提/仮定、非ゴール

