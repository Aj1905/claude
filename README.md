# claude workspace

Claude Code のサンドボックス（`/home/claude/`）配下で、ユーザー（Aj1905 / jun.akita57）の作業用データを管理するためのディレクトリ。

## 構成

```
/home/claude/
├── README.md          # このファイル
├── prompts.md         # 過去・今後のプロンプト（時系列、並べるだけ）
└── company/           # 所属企業ごとのドキュメント置き場
    ├── AJ/
    ├── ARCRA/
    ├── S/
    ├── alanse/
    ├── kamomemon/
    ├── nocall/
    └── stardy/
```

## GitHub で管理する方針

- リモートリポジトリ: `Aj1905/<repo名>`（未作成）
- ユーザー生成物のみを対象とする（`prompts.md`, `company/`）
- 機微情報は `.gitignore` で除外：`.ssh/`, `.claude/`, `.claude.json`, `.cache/`, `.local/`, `.bash*`, `.env*`, `*.pem`, `id_rsa*` など

### 制約と運用

この環境には `git` コマンドも `sudo` も入っていないため、ローカルに `.git/` を持つ従来型のバージョン管理はできない。代わりに **GitHub REST API (curl) 経由** で直接リモートにコミットを作る運用をとる：

- リポジトリ作成: `POST /user/repos`
- ファイル作成/更新: `PUT /repos/:owner/:repo/contents/:path`
- API 1回 = 1コミット として GitHub 側に履歴が残る

つまり「ローカルでのコミット履歴管理はできないが、リモート（GitHub）上で履歴は残る」。差分閲覧・復元は GitHub UI / API を使う。

### なぜ毎回トークンが必要な設計にしているか

`prompts.md` や `README.md` の変更をリモートにpushする際、常に一時的なトークンを提示する必要がある運用にしている。これは **Claude Code が動いている SAKURA サーバ側から GitHub をあまり自由にいじれないように**するための意図的な設計：

- PAT（Personal Access Token）をサーバ上に恒久的に保管しない運用を前提にすれば、仮にこの環境が侵害されても攻撃者が GitHub 側に継続的な書き込み権限を持てない
- 作業者（ユーザー）が都度トークンを発行・受け渡しすることで、操作の意思決定を人間側に残す
- 有効期限・権限スコープ（特定リポジトリ・特定権限のみ）を絞ることで、漏洩時の被害範囲を最小化

利便性より安全性を優先した設計。長寿命の汎用トークンを置きっぱなしにしない。

### pull（リモート→サーバ）について

`git` コマンドが無いので厳密な `git pull` はできないが、等価の動作は REST API で可能：

- 1ファイルだけ同期: `GET /repos/Aj1905/claude/contents/<path>` → base64デコードして上書き
- リポジトリ全体を丸ごと取得: `GET /repos/Aj1905/claude/tarball/main` → tar 展開
- 差分確認: `GET /repos/Aj1905/claude/commits` や `/compare/:base...:head`

**push と違って、これらのread系 API はトークン不要**（publicリポなので）。そのため、リモートで行われた編集をサーバ側に取り込む操作は、認証のハードルなしに実行できる。これは「サーバ側からの書き込みは制限するが、読み込みは自由に行える」という非対称な設計で、上記の安全性の意図と整合している。

## 関連する外部リソース

- `my_data` アプリ: https://mydata-sigma.vercel.app/
  - Next.js + PostgreSQL
  - md エントリを DB に保管、API 経由で CRUD
  - ソース: https://github.com/Aj1905/my_data
  - 本ワークスペースからは **読み取り専用** で参照する方針（将来的に秘書として組み込み予定）

## プロンプトログの運用

`prompts.md` には、ユーザーからの発言を時系列で「並べるだけ」で記録する。日付・番号・見出しは付けない。
