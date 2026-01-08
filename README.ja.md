# wrap-framwork-sample

このリポジトリは、**GitHub Copilot coding agent** を使って **WRAP（W/R/A/P）** を実践するための **ワークショップ用テンプレート**です。  
ゴールは「空に近いリポジトリ」から、Issue を起点に **Copilot に PR を作らせ、レビューし、マージする**という反復で、プロジェクトを段階的に育てることです。

> **運用ポリシー（ワークショップ想定）**  
> - `main` ブランチは **意図的に “ワークショップ用のブランク基準”** として維持します（`.github/**` + `README.md` が中心）。  
> - 実装作業は `workshop/YYYY-MM-DD[-suffix]` のような **日付付きブランチ**で行います。  
> - 完成形（参照実装）を残す場合は `solution` など別ブランチに保持します。

---

## このリポジトリでできること

- WRAP の考え方（なぜ必要か／どう回すか）を、**テンプレートと手順**として提供
- Issue 起票を起点に、ワークショップ用ブランチ（例: `workshop/2026-01-08`）を生成し、演習を開始
- “R（Refine）から始める” ことで、以降の Copilot タスクの成功率とレビュー容易性を上げる
- “A（Atomic）” を守って、1 Issue = 1 PR の粒度で安全に進める
- PR レビュー（P）で `@copilot` を活用し、人間が伴走して成果物を整える

---

## WRAP とは何か（価値と狙い）

WRAP は、Copilot coding agent を **「プロダクション品質に寄せて使う」**ための実践パターンです。

### W — Write effective issues（良いIssueを書く）
Copilot にとって Issue は “作業指示書” です。  
曖昧な Issue は、曖昧な PR になります。W の狙いは **曖昧さの排除**です。

このリポジトリの Issue Form では、特に以下を必須にしています：

- **Change surface**（触る／触らない範囲）
- **Acceptance criteria**（検証可能な完了条件）
- **Test plan**（実行コマンド・手順）
- **Edge cases / failure modes**（落とし穴の先回り）

### R — Refine your instructions（指示を整備する）
R は、Copilot とレビュワー（人間）が共有する「リポジトリの作法」を定義します。

- どのコマンドが正か（build/test/lint/run）
- 禁止事項（秘密情報、無関係な変更、巨大な依存追加など）
- ディレクトリ構成の意味（どこに何を書くべきか）
- PR の期待（要約、テスト方法、リスクの書き方）

空リポからのワークショップでは、**最初の PR を R だけで作る（Pattern A）**と安定します。

### A — Atomic tasks（タスクを原子化する）
A は、Copilot の得意領域に合わせるための分割戦略です。

- 1 Issue = 1 PR
- 1 PR でレビュー可能な変更量
- 「設計」と「実装」を混ぜない（必要なら設計 Issue を先に切る）

### P — Pair with the coding agent（伴走する）
Copilot の PR は “下書き” と捉え、**レビューで完成させる**のが P です。

- PR をレビューし、具体的な修正指示を出す
- PR コメントで `@copilot` を使い、修正を反復させる
- 同じ指摘が繰り返されるなら **R を更新**して再発防止

---

## このリポジトリが提供する仕組み

### 1) Issue Forms（W と R を支える）
`.github/ISSUE_TEMPLATE/` に、目的別の Issue Form を配置します（例）：

- **Copilot Task (Ultra-detailed)**  
  通常の実装・改善タスク（W を強制するフォーム）
- **WRAP R Task（R専用）**  
  指示ファイルや Copilot セットアップの整備（R の改善）
- **R をさらに Atomic にしたフォーム（任意）**  
  - instructions 専用  
  - AGENTS 専用  
  - setup-steps 専用
- **Workshop: Create a dated branch（ワークショップ開始用）**  
  Issue 起票で `workshop/YYYY-MM-DD` ブランチを生成

> 注意: 「ブランクブランチが欲しい」場合、branch 作成は **base ref の複製**です。  
> つまり、`base_ref=main` なら **main がブランクである必要**があります（このREADMEの運用ポリシー）。  
> もしくは “ブランチ作成後に空化コミットを入れる版” を採用します。

### 2) GitHub Actions（ワークショップ運用の自動化）
`.github/workflows/` には、ワークショップを繰り返しやすくする仕組みを置けます。

- Issue 起票 → 日付付きブランチ作成（例: `workshop/2026-01-08`）
- （任意）main をブランクへ戻す PR を作成する reset ワークフロー
- （後続）Copilot coding agent のための `copilot-setup-steps.yml`（R の一部として整備する対象）

---

## ワークショップの進め方（空に近い状態から始める）

ここが、このリポジトリの “一番大事な” 使い方です。

### Step 0 — ワークショップ用ブランチを作る（Issue → Actions）
1. GitHub で Issue を作成  
   - テンプレート: **Workshop: Create a dated branch**
2. date（YYYY-MM-DD）と base ref を入力  
   - 通常は `base_ref=main`（ブランク基準）
3. Issue を送信すると Actions が動き、Issue に **作成したブランチ名と checkout コマンド**が返ります
4. ローカルで checkout

```bash
git fetch origin workshop/2026-01-08
git checkout workshop/2026-01-08
```

### Step 1 — 最初の PR は “R だけ” を作る（Pattern A）
**目的**: Copilot と人間の共通ルールを先に固定し、以降のタスク成功率を上げる。

1. Issue を作成（テンプレ: **WRAP R Task**）
2. 変更対象（例）を明記  
   - `.github/copilot-instructions.md`  
   - `AGENTS.md`  
   - `.github/workflows/copilot-setup-steps.yml`
3. Copilot にアサインして PR を作らせる
4. PR をレビューしてマージ（スコープ外の変更があれば差し戻す）

**レビューの観点（R-PR）**
- 変更が R ファイルに限定されているか（diff が小さいか）
- 「TBD（未確定）」の扱いが明確か（勝手にスタックを決めていないか）
- 禁止事項・PRの期待・標準コマンドが明文化されているか

### Step 2 — 実装タスクは Atomic に分割して回す（A）
R ができたら、実装を始めます。いきなり “全部入り Issue” は作りません。

おすすめの分割例（スタック非依存）：
1. **Stack decision**（採用する言語/フレームワークを決める）  
2. **Scaffold**（最小のアプリ雛形）  
3. **Build/Test/Lint**（コマンドが揃う）  
4. **Hello/Health**（最小の動作確認）  
5. **CI**（PRで自動実行）  
6. **Docs**（README/運用手順）

### Step 3 — PR で “P（伴走）” を回す
- 指摘は PR にまとめて書く（箇条書き）
- `@copilot` を付けて修正依頼
- “同じ指摘が2回出たら” R を更新して再発防止

---

## ワークショップで扱えるコンテンツ例（メニュー）

目的や時間に合わせて、以下のモジュールを組み合わせられます。

### Module 1: WRAP 入門（W/R/A/P の型）
- WRAP の意義、役割分担（Issue = プロンプト）
- “良い Issue” の書き方（Change surface / Acceptance criteria / Test plan）
- PR レビューでの `@copilot` の使い方（P）

### Module 2: R-first ブートストラップ（最初の PR は R だけ）
- `.github/copilot-instructions.md` の作成／改善
- `AGENTS.md` の作成／改善
- `copilot-setup-steps.yml` を作り、次の Copilot セッションから効かせる

### Module 3: Atomic 実装（1 Issue = 1 PR の反復）
- Scaffold → build/test → health endpoint
- テスト戦略（最低限のユニット/統合）
- 依存追加の判断と PR での説明

### Module 4: 品質と運用（継続できる形にする）
- CI を追加（lint/test/build）
- セキュリティと秘密情報の扱い
- “繰り返し指摘” を R に吸収する運用

---

## トラブルシューティング（よくある落とし穴）

### Q. 作成された workshop ブランチがブランクにならない
A. ブランチ作成は **base ref の複製**です。`base_ref=main` なら、main がブランク基準である必要があります。  
（代替）ブランチ作成後に “空化コミット” を入れるワークフローを使います。

### Q. Issue に追記したのに Copilot の挙動が変わらない
A. Copilot はアサイン時点の Issue を元に作業することがあるため、変更要求は **PR 側のコメント**で伝える運用に寄せます。

### Q. Actions がブランチ作成に失敗する／権限エラーになる
A. Workflow の `permissions` と、リポジトリ/組織の Actions 権限設定を確認してください。  
（安全のため）Issue 起票者の権限を `write/maintain/admin` に制限するガードを入れるのが推奨です。

---

## 参考: 参照実装（solution ブランチ）について（任意）

ワークショップの “完成形” を参照したい場合は、`solution` ブランチ（またはタグ）を用意しておきます。  
実装手順や実行方法は、そのブランチの README を参照してください。

---

## License
MIT

