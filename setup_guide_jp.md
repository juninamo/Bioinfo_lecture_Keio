# セットアップガイド：シングルセル解析環境の構築

この[ガイド](https://github.com/juninamo/SoM_class/blob/main/setup_guide_jp.md)では、シングルセル解析ワークショップに必要な環境を構築する手順を説明します。**macOS** と **Windows** の両方に対応しています。

> **⚠️ 重要：バージョンについて**
> 本ガイドでは、再現性を確保するために**具体的なバージョン番号**を指定しています。
> 講義資料 (`single_cell_analysis_T.html`) の結果を再現するため、可能な限り指定されたバージョンを使用してください。

---

## 0. 必要なPCスペック

シングルセル解析はメモリを多く使用します。以下のスペックを推奨します。

| 項目 | 推奨スペック |
|------|------------|
| OS | macOS 13以降 / Windows 10 (64-bit) 以降 |
| メモリ (RAM) | **8 GB以上**（16 GB推奨） |
| ディスク空き容量 | **10 GB以上** |
| CPU | Intel / Apple Silicon (M1/M2/M3/M4) |

---

## 1. コマンドライン（ターミナル）の使い方

本ガイドでは、コマンドライン（ターミナル）を使用してソフトウェアをインストールします。
コマンドラインとは、テキストでコンピュータに指示を出すためのツールです。

### macOS の場合

1. **Spotlight検索**（`Command ⌘` + `Space`）で「**ターミナル**」または「**Terminal**」と入力
2. 表示された **ターミナル.app** を開きます
3. 黒い画面にテキストが表示されれば準備完了です

> **💡 ヒント**：Launchpad → その他 → ターミナル からも開けます。

### Windows の場合

Windows には複数のコマンドラインツールがありますが、本ガイドでは **Miniforge Prompt** を使用します（セクション2でインストールします）。

Miniforgeインストール前に動作確認したい場合は、以下の方法で **PowerShell** を開くことができます：

1. **キーボードの `Windows` キー** を押して、「**PowerShell**」と入力
2. 表示された **Windows PowerShell** をクリックして開きます
3. 青い画面に `PS C:\Users\(ユーザー名)>` と表示されれば準備完了です

> **⚠️ 注意（Windows）**：セクション2以降では、必ず **Miniforge Prompt** を使用してください。
> Miniforge Prompt は Conda および Mamba コマンドが使えるように設定されたコマンドラインです。

### コマンドの実行方法

コマンドラインでは、表示されたプロンプト（`$` や `>` の後）にコマンドを入力し、**Enterキー** を押して実行します。
本ガイドのコードブロック内のテキストを**1行ずつ**コピー＆ペーストして実行してください。

```
# この行は「コメント」です（# で始まる行は実行されません）
# 下の行がコマンドの例です：
mamba --version
```

---

## 2. Mamba（Miniforge）のインストール

R本体やRパッケージを含め、必要なソフトウェアはすべて **Mamba** を使ってインストールします。
MambaはCondaの機能をC++で再実装したものであり、パッケージの依存関係解決やダウンロードが非常に高速に行えるため、本講義ではCondaの代わりにMambaを使用することを推奨します。Condaと同様に、バージョンを正確に指定し、環境を分離管理することができます。

### macOS の場合

ターミナルを開き、以下を実行します。

#### Apple Silicon (M1/M2/M3/M4) Mac の場合

```bash
# Miniforgeのインストーラをダウンロード
curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh"

# インストール（全てデフォルトで進めてください）
bash Miniforge3-MacOSX-arm64.sh
```

#### Intel Mac の場合

```bash
curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-x86_64.sh"
bash Miniforge3-MacOSX-x86_64.sh
```

インストール後、**ターミナルを閉じて再度開いてください**。
プロンプトの先頭に `(base)` と表示されていればインストール成功です。

### Windows の場合

1. [Miniforgeダウンロードページ](https://github.com/conda-forge/miniforge/releases/latest) にアクセスします
2. **`Miniforge3-Windows-x86_64.exe`** をダウンロードして実行します
3. インストール途中の設定画面で：
   - **「Add Miniforge3 to my PATH environment variable」** に ☑️ チェックを入れてください
   - **「Register Miniforge3 as my default Python」** にもチェックを入れてください
4. インストール完了後、**スタートメニュー** を開き、「**Miniforge Prompt**」と検索して開きます

> **💡 ヒント（Windows）**：以降のすべての作業は **Miniforge Prompt** で行ってください。
> 通常のコマンドプロンプトやPowerShellでは `mamba` や `conda` コマンドが動作しない場合があります。

---

## 3. 環境の作成とソフトウェアのインストール

以下のコマンドを **1行ずつ** ターミナル (macOS) または Miniforge Prompt (Windows) に入力して実行してください。

### Step A. 環境の作成（R 4.3.2 と Jupyter Notebook）

本ガイドでは、RStudioの代わりに**Jupyter Notebook**を推奨します。理由は、コード、実行結果、そして説明書きとなるメモを一つのノートブック形式でまとめて記録でき、解析プロセスの再現性確認や他の研究者との共有が容易になるためです。

#### macOS / Windows 共通

```bash
# scworkshop という名前の環境を作成
# R 4.3.2、Jupyter Notebook、およびRをJupyterで使うためのIRkernelをインストールします
mamba create -n scworkshop -c conda-forge r-base=4.3.2 jupyter r-irkernel -y
```

> **⏱ 注意：初回のインストールには10〜20分程度かかることがあります。** 途中で止まっているように見えても、そのまま待ってください。

> **⚠️ バージョン指定でエラーが出る場合**：
> `=4.3.2` などのバージョン番号を削除して再実行してください。
> 例：
> ```bash
> # バージョン指定なし
> mamba create -n scworkshop -c conda-forge r-base jupyter r-irkernel -y
> ```

### Step B. 環境の有効化

```bash
# 環境を有効化（以降の作業は必ずこのコマンドを先に実行してください）
conda activate scworkshop
```

プロンプトの先頭が `(scworkshop)` に変わったことを確認してください。

> **⚠️ 重要**：ターミナル / Miniforge Prompt を開くたびに `conda activate scworkshop` を実行する必要があります。

### Step C. Rパッケージのインストール（mamba経由）

`single_cell_analysis_T.Rmd` で使用するパッケージをインストールします。

```bash
# CRANパッケージ
mamba install -c conda-forge \
  r-seurat=5.2.1 \
  r-patchwork=1.1.3 \
  r-ggplot2 \
  r-harmony \
  r-symphony \
  r-lisi \
  r-dplyr=1.1.4 \
  r-magrittr=2.0.3 \
  r-rmarkdown=2.25 \
  r-knitr=1.45 \
  r-bookdown=0.37 \
  -y
```

> **💡 Windows での注意**：Windows では `\`（バックスラッシュ、行の継続記号）が動作しない場合があります。
> その場合は、すべてを **1行** にまとめて実行してください：
> ```bash
> mamba install -c conda-forge r-seurat=5.2.1 r-patchwork=1.1.3 r-ggplot2 r-harmony r-symphony r-lisi r-dplyr=1.1.4 r-magrittr=2.0.3 r-rmarkdown=2.25 r-knitr=1.45 r-bookdown=0.37 -y
> ```

> **⚠️ バージョン指定でエラーが出る場合**：
> `=5.2.1` などのバージョン番号をすべて削除して再実行してください。
> ```bash
> mamba install -c conda-forge r-seurat r-patchwork r-ggplot2 r-harmony r-symphony r-lisi r-dplyr r-magrittr r-rmarkdown r-knitr r-bookdown -y
> ```

### Step D. Bioconductorパッケージのインストール（mamba経由）

```bash
# BiocStyle をBiocondaチャンネルからインストール
mamba install -c bioconda -c conda-forge \
  bioconductor-biocstyle=2.30.0 \
  bioconductor-stabmap \
  -y
```

> **💡 Windows での1行版**：
> ```bash
> mamba install -c bioconda -c conda-forge bioconductor-biocstyle=2.30.0 bioconductor-stabmap -y
> ```

> **⚠️ バージョン指定でエラーが出る場合**：
> ```bash
> mamba install -c bioconda -c conda-forge bioconductor-biocstyle bioconductor-stabmap -y
> ```

---

## 4. Jupyter Notebook の起動

Step AでインストールしたJupyter Notebookを起動します。macOS、Windows共通で以下の手順を使用します。

```bash
# 環境を有効化（まだ実行していない場合）
conda activate scworkshop

# Jupyter Notebookを起動
jupyter notebook
```

実行すると、ブラウザが自動的に開き、Jupyter Notebookの画面が表示されます。

> **💡 右上の「New」メニュー** から「R」を選択することで、Rのノートブックを新規作成できます。作成したノートブック上でRのコードを実行してデータ解析を進めることができます。
> 
> **⚠️ 注意**：デスクトップのショートカットなどから起動するのではなく、必ず上記のコマンドを使用してターミナル / Miniforge Prompt から起動してください。そうすることで、正しい環境（`scworkshop`）のRが使用されます。

#### （代替案）JupyterなしでRを直接使う

上記の方法でもうまくいかない場合は、Miniforge Prompt から R を直接起動できます：

```bash
conda activate scworkshop
R
```

R のコンソールが表示されるので、そこにコマンドを直接入力して実行してください。

---

## 5. データの準備

### A. プロジェクトフォルダの作成

デスクトップにプロジェクト用のフォルダを作成します。

#### macOS の場合（ターミナル）
```bash
# デスクトップへ移動
cd ~/Desktop

# フォルダの作成
mkdir SingleCellWorkshop
cd SingleCellWorkshop

# データ用フォルダの作成
mkdir data
```

#### Windows の場合（Miniforge Prompt）
```bash
# デスクトップへ移動
cd %USERPROFILE%\Desktop

# フォルダの作成
mkdir SingleCellWorkshop
cd SingleCellWorkshop

# データ用フォルダの作成
mkdir data
```

#### フォルダ構成のイメージ
作成後の構成は以下のようになります：
```
SingleCellWorkshop/
│── JIAsyno_CITEseq_T_NK.rds                  (T細胞解析用データ)
├── single_cell_analysis.Rmd                  (Part 1: 全体解析)
└── single_cell_analysis_part2_Tcell.Rmd      (Part 2: T細胞サブクラス解析)
```

### B. データのダウンロード

本講義では **GSE278962** (JIA Synovial CITE-seq) のデータを使用します。
- 講義資料で配布される `.rds` ファイルを[こちら](https://drive.google.com/drive/folders/18LeP2pMmQd7oYmMOLgW2Cr2P6e52UrSj?usp=sharing)よりダウンロードし、上記の `data/` フォルダに配置してください。
- 講義資料で作業ディレクトリの設定方法を確認してください。

---

## 6. インストールの確認

Jupyter Notebookで新規Rノートブックを作成する（セクション4参照）か、コマンドラインで直接Rを起動し、以下のスクリプトを実行してください。

```r
# --- バージョン確認スクリプト ---

cat("=== R バージョン ===\n")
cat(R.version.string, "\n\n")

cat("=== パッケージバージョン確認 ===\n")

# 必須パッケージの確認（single_cell_analysis_T.Rmd で使用）
packages <- c("Seurat", "BiocStyle", "patchwork", "ggplot2", "harmony", "symphony",
              "lisi", "StabMap", "dplyr", "magrittr", "knitr", "rmarkdown")

for (pkg in packages) {
  tryCatch({
    library(pkg, character.only = TRUE)
    cat(sprintf("  ✓ %-15s : %s\n", pkg, packageVersion(pkg)))
  }, error = function(e) {
    cat(sprintf("  ✗ %-15s : インストールされていません！\n", pkg))
  })
}

cat("\n=== 結果 ===\n")
cat("上記のパッケージがすべて ✓ であれば、準備完了です！\n")
```

### 期待される出力（参考）

| パッケージ | 期待されるバージョン |
|-----------|-------------------|
| R         | 4.3.2             |
| Seurat    | 5.2.1             |
| BiocStyle | 2.30.0            |
| patchwork | 1.1.3             |
| ggplot2   | (Any)             |
| harmony   | (Any)             |
| symphony  | (Any)             |
| lisi      | (Any)             |
| StabMap   | (Any)             |
| dplyr     | 1.1.4             |
| magrittr  | 2.0.3             |
| knitr     | 1.45              |
| rmarkdown | 2.25              |

> **💡 注意**：多少のマイナーバージョンの違い（例：1.1.3 vs 1.1.4）は問題ありません。
> メジャーバージョン（例：Seurat 5.x.x vs 4.x.x）が異なる場合は、インストールをやり直してください。

---

## 7. トラブルシューティング

### Q1. `conda` コマンドが見つからない（"command not found"）

**macOS の場合：**
ターミナルを閉じて再度開いてください。それでも解決しない場合：
```bash
# シェルの初期化
~/miniforge3/bin/conda init
```
その後、ターミナルを再起動してください。

**Windows の場合：**
通常のコマンドプロンプトではなく、**Miniforge Prompt** を使用してください。
スタートメニューから「Miniforge Prompt」と検索して開いてください。

### Q2. `conda activate scworkshop` が動作しない

```bash
conda init
```
を実行後、ターミナル / Miniforge Prompt を**閉じて再度開いて**ください。

### Q3. パッケージのインストールでエラーが出る

**Mambaで解決できない場合**は、R内から直接インストールを試みてください：

```bash
conda activate scworkshop
R
```

R起動後：

```r
# CRANパッケージの場合
install.packages("パッケージ名")

# バージョンを指定してインストールする場合（例：Seurat 5.2.1）
install.packages("remotes")
remotes::install_version("Seurat", version = "5.2.1")

# Bioconductorパッケージの場合
if (!require("BiocManager", quietly = TRUE)) install.packages("BiocManager")
BiocManager::install("パッケージ名")
```

**macOS でコンパイルエラーが出る場合：**
Xcode Command Line Toolsが必要です。ターミナルで以下を実行：
```bash
xcode-select --install
```

**Windows でコンパイルエラーが出る場合：**
Rtoolsが必要な場合があります：
```bash
mamba install -c conda-forge m2w64-toolchain -y
```

### Q4. Jupyter NotebookでRが選択できない / Rのバージョンが違う

必ず `conda activate scworkshop` を実行してから `jupyter notebook` コマンドで起動してください。
別のショートカットからの起動では、別のRやPython環境が使われる可能性があります。

### Q5. メモリ不足エラーが発生する

- 不要なアプリケーションを閉じる
- コード内で不要になったオブジェクトを削除し、ガベージコレクションを実行する（`rm(object_name); gc()`）

### Q6. Seuratのバージョンが5.x.xではなく4.x.xになる

```bash
conda activate scworkshop
mamba install -c conda-forge r-seurat=5.2.1 r-seuratobject=5.0.2 -y
```

---

## 8. 代替手段：Mamba (Conda) を使わない環境構築

どうしてもMambaでの環境構築がうまくいかない場合、RとJupyter Notebook（またはRStudio）を個別にインストールし、パッケージを手動でインストールすることも可能です。

### A. Rのインストール
1. CRAN (The Comprehensive R Archive Network) にアクセスします：https://cran.r-project.org/
2. お使いのOS（macOS または Windows）に合わせてRのインストーラーをダウンロードし、実行します。講義環境に合わせる場合は**R 4.3.2**の過去バージョンを探してインストールしてください（最新版でも動作する可能性はありますが、図の出力等が異なる場合があります）。

### B. RStudio のインストール
1. Positの公式サイトにアクセスします：https://posit.co/download/rstudio-desktop/
2. 無料版（Free version）のRStudio Desktopをダウンロードし、インストールします。

### C. Jupyter Notebookの設定
独立してインストールしたRをJupyter Notebookで使用するには、以下の手順で設定します。
1. Python/Jupyterがインストールされている環境で、Rのコンソールを開きます。
2. 以下のコマンドを実行してIRkernelをインストールします：
   ```r
   install.packages("IRkernel")
   IRkernel::installspec(user = FALSE)
   ```
3. これでJupyter NotebookからRが選択できるようになります。

### D. Rパッケージの手動インストール
Jupyter NotebookのRカーネル（またはRコンソール）を起動し、以下のコマンドを実行してパッケージをインストールします。

```r
# CRANパッケージのインストール
install.packages(c("patchwork", "ggplot2", "harmony", "symphony", "dplyr", "magrittr", "rmarkdown", "knitr", "bookdown"))

# 次に remotes パッケージを使って Seurat 5.2.1 をインストールします
if (!require("remotes", quietly = TRUE)) install.packages("remotes")
remotes::install_version("Seurat", version = "5.2.1")

# LISI と StabMap を GitHub からインストールします
remotes::install_github("immunogenomics/LISI")
remotes::install_github("SydneyBioX/StabMap")

# Bioconductor パッケージのインストール
if (!requireNamespace("BiocManager", quietly = TRUE)) {
    install.packages("BiocManager")
}
BiocManager::install("BiocStyle")
```

すべてのインストールが完了したら、セクション6「インストールの確認」のスクリプトを実行して正しくインストールされたか確認してください。

---

## 9. 参考：sessionInfo() の完全出力

`single_cell_analysis_T.html` を生成した環境の完全な情報です。トラブルシューティング時にご参照ください。

```
R version 4.3.2 (2023-10-31)
Platform: aarch64-apple-darwin20 (64-bit)
Running under: macOS 26.2

attached base packages:
 stats     graphics  grDevices utils     datasets  methods   base     

other attached packages:
 patchwork_1.1.3    magrittr_2.0.3     Seurat_5.2.1
 SeuratObject_5.0.2 sp_2.1-2           BiocStyle_2.30.0  

loaded via a namespace (and not attached):
 RColorBrewer_1.1-3     rstudioapi_0.15.0      jsonlite_1.8.8        
 spatstat.utils_3.1-2    ggbeeswarm_0.7.2       magick_2.8.2          
 farver_2.1.1            rmarkdown_2.25         vctrs_0.6.5           
 ROCR_1.0-11             spatstat.explore_3.3-4  htmltools_0.5.7       
 sass_0.4.8              sctransform_0.4.1      parallelly_1.36.0     
 KernSmooth_2.23-22      bslib_0.6.1            htmlwidgets_1.6.4     
 ica_1.0-3               plyr_1.8.9             plotly_4.10.3         
 zoo_1.8-12              cachem_1.0.8           igraph_1.6.0          
 mime_0.12               lifecycle_1.0.4        pkgconfig_2.0.3       
 Matrix_1.6-5            R6_2.5.1               fastmap_1.1.1         
 fitdistrplus_1.1-11     future_1.33.1          shiny_1.8.0           
 digest_0.6.33           colorspace_2.1-0       tensor_1.5            
 RSpectra_0.16-1         irlba_2.3.5.1          labeling_0.4.3        
 progressr_0.14.0        fansi_1.0.6            spatstat.sparse_3.1-0 
 httr_1.4.7              polyclip_1.10-6        abind_1.4-5           
 compiler_4.3.2          withr_2.5.2            fastDummies_1.7.3     
 highr_0.10              MASS_7.3-60            tools_4.3.2           
 vipor_0.4.7             lmtest_0.9-40          beeswarm_0.4.0        
 httpuv_1.6.13           future.apply_1.11.1    goftest_1.2-3         
 glue_1.6.2              nlme_3.1-163           promises_1.2.1        
 grid_4.3.2              Rtsne_0.17             cluster_2.1.4         
 reshape2_1.4.4          generics_0.1.3         gtable_0.3.4          
 spatstat.data_3.1-4     tidyr_1.3.0            data.table_1.16.0     
 utf8_1.2.4              spatstat.geom_3.3-5    RcppAnnoy_0.0.21      
 ggrepel_0.9.4           RANN_2.6.1             pillar_1.9.0          
 stringr_1.5.1           limma_3.58.1           spam_2.10-0           
 RcppHNSW_0.5.0          later_1.3.2            splines_4.3.2         
 dplyr_1.1.4             lattice_0.21-9         survival_3.5-7        
 deldir_2.0-2            tidyselect_1.2.0       miniUI_0.1.1.1        
 pbapply_1.7-2           knitr_1.45             gridExtra_2.3         
 bookdown_0.37           scattermore_1.2        xfun_0.41             
 statmod_1.5.0           matrixStats_1.2.0      stringi_1.8.3         
 lazyeval_0.2.2          yaml_2.3.8             evaluate_0.23         
 codetools_0.2-19        tibble_3.2.1           BiocManager_1.30.22   
 cli_3.6.2               uwot_0.1.16            xtable_1.8-4          
 reticulate_1.35.0       munsell_0.5.0          jquerylib_0.1.4       
 Rcpp_1.0.11             globals_0.16.2         spatstat.random_3.3-2 
 png_0.1-8               ggrastr_1.0.2          spatstat.univar_3.1-2 
 parallel_4.3.2          ellipsis_0.3.2         ggplot2_3.4.4         
 presto_1.0.0            dotCall64_1.1-1        listenv_0.9.0         
 viridisLite_0.4.2       scales_1.3.0           ggridges_0.5.5        
 purrr_1.0.2             rlang_1.1.2            cowplot_1.1.2
```
