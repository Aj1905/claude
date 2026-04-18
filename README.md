# claude


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

## 関連する外部リソース

- `my_data` アプリ: https://mydata-sigma.vercel.app/
  - Next.js + PostgreSQL
  - md エントリを DB に保管、API 経由で CRUD
  - ソース: https://github.com/Aj1905/my_data
  - 本ワークスペースからは **読み取り専用** で参照する方針（将来的に秘書として組み込み予定）

## プロンプトログの運用

`prompts.md` には、ユーザーからの発言を時系列で「並べるだけ」で記録する。日付・番号・見出しは付けない。
