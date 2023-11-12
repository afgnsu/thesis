蘇介吾簡介 1121112
========================

此專案是 [Pandoc](https://github.com/jgm/pandoc) 的臺大碩博士論文模板，讓使用者能透過 (Pandoc) **Markdown** 語法撰寫論文 (無須安裝與了解 LaTeX!!)。此專案的前身是 [ntuthesis2](https://github.com/liao961120/ntuthesis2) (Python Script) 以及 [ntuthesis](https://github.com/liao961120/ntuthesis) (R 套件)。這個版本大幅精簡了先前的相依需求。


輸出論文
------------------

### GitHub

如果你有 GitHub 帳號，可以透過 GitHub Action 直接輸出論文 (Overleaf 專案連結與 HTML)，無需在電腦上安裝任何東西。只要 fork 此 Repo，每次 push commit 至 GitHub 後，即會以靜態網頁以及 Overleaf 專案的形式輸出論文。

- 靜態網頁的網址：`https://{user}.github.io/{repo}/`，例如 <https://liao961120.github.io/thesis>。
- Overleaf 專案的網址為： `https://www.overleaf.com/docs?engine=xelatex&main_document=main.tex&snip_uri=https://{user}.github.io/{repo}/overleaf.zip`，例如 <https://www.overleaf.com/docs?engine=xelatex&main_document=main.tex&snip_uri=https://liao961120.github.io/thesis/overleaf.zip>。點擊進入這網址後，Overleaf 就會自動匯入專案並輸出 PDF。


### 個人電腦

如果想在本機預覽 HTML 輸出結果，需安裝 [Pandoc](https://github.com/jgm/pandoc/releases) 2.14 (或更新) 以及 [pandoc-crossref](https://github.com/lierdakil/pandoc-crossref/releases)。安裝之後，至 Terminal 執行下方指令 (Windows 系統因為沒有 Shell，建議可以安裝 [Git Bash](https://gitforwindows.org)):

```bash
bash build-html.sh  # 輸出 docs/index.html
bash build-tex.sh   # 輸出 docs/overleaf.zip，可上傳至 Overleaf 或是自己輸出成 PDF
```

### 透過 VSCode 預覽 Pandoc

VSCode 的擴充套件 [Markdown Preview Enhanced](https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced) 支援使用 Pandoc 作為 Markdown 的 Parser，對於撰寫論文時很有幫助 (即時預覽)。另一個套件 [Pandoc Citer](https://marketplace.visualstudio.com/items?itemName=notZaki.pandocciter) 則可協助於 markdown 裡插入引用文獻。建議可以將下方的設定加入 VSCode 的 `settings.json`:

```json
{
  ...,
  "markdown-preview-enhanced.usePandocParser": true,
      "markdown-preview-enhanced.pandocArguments": [
          "--filter=pandoc-crossref",
          "--lua-filter=deps/pandoc-ling.lua",
          "--citeproc",
          "--mathjax",
          "--number-sections",
          "--include-in-header=deps/style.html",
          "--bibliography=references.bib",
      ],
  "PandocCiter.DefaultBib": "references.bib",
  "PandocCiter.UseDefaultBib": true
}
```


### 論文語言設定

論文可以使用中文或是英文撰寫。相關的設定在 `setup.md` 開頭的 yaml。若 
`acknowledgement-in-en` 為 true, 則論文中的標題會是 "Acknowledgement"；反之，則為「致謝辭」。

```markdown
---
language     : "chinese"  # chinese | english
acknowledgement-in-en: false  # true for english
---
```

此外，由於 Pandoc 處理 in-text citation 的預設行為可能不適合中文論文 (多出不必要的空白)，可至 `build-html.sh` 以及 `build-tex.sh` 中，將下方這行移除：

```bash
# 其它內容...
    --lua-filter deps/lua/rmCiteSpace.lua \
# 其它內容...
```



使用
------------------

由於 LaTeX ([TeX Live](https://www.tug.org/texlive) 或 [MikTeX](https://miktex.org)) 的安裝十分令人頭痛，而且即使安裝完成，後續生成 PDF 的過程仍會常常出錯。為了儘量避開這些問題，此專案在設計上結合 [Overleaf](https://www.overleaf.com)，讓使用者能將輸出的 `.tex` 檔透過 (免費) 網路服務輸出成 PDF 檔。當然，若使用者個人電腦上已有 LaTeX，也可直接在電腦上輸出 PDF。此外，這裡亦提供一個簡約的 HTML 輸出格式，目的是方便在撰寫時預覽論文 (因為 LaTeX 的輸出相當耗時)。


### 論文撰寫

下方為 Repo 的結構，基本上使用者僅需要更動兩個地方---`setup.md` 以及 `chapters/` 裡的檔案。其它部份若不清楚是什麼可以不用理會，也不要去刪除它們。

```bash
/
├── setup.md               # 論文參數設定 (標題、作者、指導教授、摘要、謝辭...)
├── build-html.sh          # 輸出 HTML
├── build-tex.sh           # 輸出 overleaf.zip (可上傳至 Overleaf compile)
├── docs/                  # 論文輸出
├── chapters/              # 論文內容
│   ├── 01-intro.md           # 第一章 (命名規則：{##}-{title}.md)
│   ├── 02-pandocSyntax.md    # 第二章 (命名規則：{##}-{title}.md)
│   ├── appendix-A.md         # 附錄 A (命名規則：appendix-{編號}.md)
│   ├── appendix-B.md         # 附錄 B (命名規則：appendix-{編號}.md)
│   ├── denotations.md        # 符號列表 (若用不到，請刪除檔案內容，但勿刪檔案本身)
│   ├── references.yaml       # 引用書目 (CSL YAML)
│   ├── tables.tex            # 複雜表格定義 (LaTeX 語法)
│   ├── tables.html           # 複雜表格定義 (HTML 語法)
│   └── figures/              # 圖片存放處 (勿刪除此資料夾)
└── deps/                  # 論文模板、程式輸出相依檔案
    ├── ...
    └── citation-style.csl # 引用格式 (至 zotero.org/styles 下載，模板使用 APA6)
```
