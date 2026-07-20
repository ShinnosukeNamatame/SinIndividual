# SinIndividual

個人用ドキュメントリポジトリ（旅のしおり等）

# ディレクトリ構成

```
SinIndividual/
└── Kaede/                  # 楓さん関連
    └── Nagoya/             # 名古屋旅行のしおり
        ├── Nagoya.tex      # しおり本文（LuaLaTeX）
        ├── NagoyaScreenShots/  # 行きたい場所のスクリーンショット（しおりに掲載）
        └── out/            # ビルド成果物（git 管理外）
```

# 環境構築

以下の環境を想定:

- macOS
- IntelliJ IDEA 2025
- TeX フルインストール
- lualatex

## File Watchers による自動コンパイル設定

SinGraduate / SinGraduateClass と同一の設定。`.idea/watcherTasks.xml`（2025.3 形式）に定義済みで、
File Watchers プラグインが入っていればプロジェクトを開くだけで有効になる。

- `.tex` ファイルを編集すると逐次コンパイルが走る ( vsCode とは異なりCtrl+Sではない )
- PDF は `out/` に出力される。自動表示されないので、PDF Viewer プラグインで手動で一度開く必要あり

手動で再作成する場合の設定値：

| 項目           | 値                                                                                                 |
|--------------|---------------------------------------------------------------------------------------------------|
| 名前           | `latexmk`                                                                                         |
| ファイルタイプ      | `LaTeX source file`                                                                               |
| スコープ         | `プロジェクトファイル`                                                                                      |
| プログラム        | `/Library/TeX/texbin/latexmk`                                                                     |
| 引数           | `-lualatex -g -synctex=1 -interaction=nonstopmode -output-directory="$FileDir$/out" "$FilePath$"` |
| リフレッシュする出力パス | `$FileDir$/out/$FileNameWithoutExtension$.pdf`                                                    |
| 作業ディレクトリ     | `$FileDir$`                                                                                       |

高度なオプション：

- `編集したファイルを自動保存してウォッチャーをトリガーする` → **オン**
- `外部の変更でウォッチャーをトリガーする` → **オフ**

`-g` の必要性や IDE バージョン統一（**2025.3 固定**、`.idea/watcherTasks.xml` のフォーマット非互換）の詳細は
`SinGraduate/README.md` を参照。

## 手動コンパイル

```bash
cd Kaede/Nagoya
latexmk -lualatex -g -synctex=1 -interaction=nonstopmode -output-directory=out Nagoya.tex
```

# 注意（LuaTeX-ja の既知の落とし穴）

`luatexja` 使用時、`\section` 直後に本文を挟まず `center` 環境＋ `tabular` を置くと
`! Undefined control sequence. <argument> p{...}\@nil` でコンパイルに失敗する。
見出しと表の間に 1 行本文を入れると回避できる（`Nagoya.tex` はこの構成にしてある）。
