# Setup Guide: Building a Single-Cell Analysis Environment

This [guide](https://github.com/juninamo/SoM_class/blob/main/setup_guide_jp.md) provides instructions for setting up the environment required for the single-cell analysis workshop. It is compatible with both **macOS** and **Windows**.

> **⚠️ Important: About Versions**
> This guide specifies **precise version numbers** to ensure reproducibility.
> To reproduce the results in the lecture material (`single_cell_analysis_T.html`), please use the specified versions as much as possible.

---

## 0. Required PC Specifications

Single-cell analysis is memory-intensive. The following specifications are recommended.

| Item | Recommended Spec |
|------|-----------|
| OS | macOS 13+ / Windows 10 (64-bit)+ |
| Memory (RAM) | **8 GB+** (16 GB recommended) |
| Disk Space | **10 GB+** |
| CPU | Intel / Apple Silicon (M1/M2/M3/M4) |

---

## 1. How to Use the Command Line (Terminal)

In this guide, we will use the command line (Terminal) to install the software.
The command line is a tool for giving instructions to your computer using text.

### For macOS

1. Use **Spotlight Search** (`Command ⌘` + `Space`) and type "**Terminal**".
2. Open the **Terminal.app**.
3. You are ready when a black screen with text appears.

> **💡 Hint**: You can also open it from Launchpad → Other → Terminal.

### For Windows

Windows has several command-line tools, but in this guide, we will use the **Miniforge Prompt** (which will be installed in Section 2).

If you want to check if the command line works before installing Miniforge, you can open **PowerShell**:

1. Press the **`Windows` key** and type "**PowerShell**".
2. Click and open **Windows PowerShell**.
3. You are ready when a blue screen displays `PS C:\Users\(username)>`.

> **⚠️ Note (Windows)**: For Section 2 and onwards, always use the **Miniforge Prompt**.
> The Miniforge Prompt is a command line specifically configured to use Conda and Mamba commands.

### How to Execute Commands

On the command line, type the command after the prompt (after `$` or `>`) and press the **Enter key** to execute it.
Please copy and paste the text from the code blocks in this guide **one line at a time** and execute them.

```bash
# This line is a "comment" (lines starting with # are not executed)
# Below is an example of a command:
mamba --version
```

---

## 2. Installing Mamba (Miniforge)

We will use **Mamba** to install everything, including R itself and R packages.
We recommend using Mamba instead of Conda because Mamba is a C++ reimplementation of Conda that is significantly faster at resolving dependencies and installing packages. Like Conda, it allows you to specify exact versions and manage environments separately.

### For macOS

Open the Terminal and execute the following.

#### For Apple Silicon (M1/M2/M3/M4) Mac

```bash
# Download the Miniforge installer
curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh"

# Install (Please proceed with the default settings)
bash Miniforge3-MacOSX-arm64.sh
```

#### For Intel Mac

```bash
curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-x86_64.sh"
bash Miniforge3-MacOSX-x86_64.sh
```

After installation, **close the Terminal and reopen it**.
You have successfully installed it if `(base)` appears at the beginning of the prompt.

### For Windows

1. Go to the [Miniforge Download Page](https://github.com/conda-forge/miniforge/releases/latest).
2. Download and run **`Miniforge3-Windows-x86_64.exe`**.
3. During the installation setup:
   - Check ☑️ **"Add Miniforge3 to my PATH environment variable"**.
   - Check **"Register Miniforge3 as my default Python"**.
4. After installation, open the **Start Menu**, search for "**Miniforge Prompt**", and open it.

> **💡 Hint (Windows)**: Perform all subsequent tasks in the **Miniforge Prompt**.
> The `mamba` or `conda` commands may not work in the regular Command Prompt or PowerShell.

---

## 3. Creating an Environment and Installing Software

Enter and execute the following commands **one line at a time** in the Terminal (macOS) or Miniforge Prompt (Windows).

### Step A. Creating the Environment (R 4.3.2 and Jupyter Notebook)

In this guide, we recommend using **Jupyter Notebook** instead of RStudio. The reason is that it allows you to combine code, execution outputs, and explanatory notes in a single interactive document format. This makes it easier to verify the reproducibility of your analysis workflow and share it with other researchers.

#### For macOS / Windows

```bash
# Create an environment named scworkshop
# This installs R 4.3.2, Jupyter Notebook, and IRkernel (to use R in Jupyter)
mamba create -n scworkshop -c conda-forge r-base=4.3.2 jupyter r-irkernel -y
```

> **⏱ Note**: Initial installation may take 10–20 minutes. Please wait even if it seems to have stopped.

> **⚠️ If an error occurs with the version specification**:
> Remove the version numbers such as `=4.3.2` and try again.
> Example:
> ```bash
> # Without version
> mamba create -n scworkshop -c conda-forge r-base jupyter r-irkernel -y
> ```

### Step B. Activating the Environment

```bash
# Activate the environment (Always run this command first for subsequent tasks)
conda activate scworkshop
```

Confirm that the beginning of the prompt has changed to `(scworkshop)`.

> **⚠️ Important**: You must run `conda activate scworkshop` every time you open the Terminal / Miniforge Prompt.

### Step C. Installing R Packages (via Mamba)

Install the packages used in `single_cell_analysis_T.Rmd`.

```bash
# CRAN Packages
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

> **💡 Note for Windows**: The `\` (backslash, line continuation character) may not work on Windows.
> In that case, combine everything into **one line**:
> ```bash
> mamba install -c conda-forge r-seurat=5.2.1 r-patchwork=1.1.3 r-ggplot2 r-harmony r-symphony r-lisi r-dplyr=1.1.4 r-magrittr=2.0.3 r-rmarkdown=2.25 r-knitr=1.45 r-bookdown=0.37 -y
> ```

> **⚠️ If an error occurs with the version specification**:
> Remove all version numbers such as `=5.2.1` and try again.
> ```bash
> mamba install -c conda-forge r-seurat r-patchwork r-ggplot2 r-harmony r-symphony r-lisi r-dplyr r-magrittr r-rmarkdown r-knitr r-bookdown -y
> ```

### Step D. Installing Bioconductor Packages (via Mamba)

```bash
# Install BiocStyle from the Bioconda channel
mamba install -c bioconda -c conda-forge \
  bioconductor-biocstyle=2.30.0 \
  bioconductor-stabmap \
  -y
```

> **💡 Windows One-Line Version**:
> ```bash
> mamba install -c bioconda -c conda-forge bioconductor-biocstyle=2.30.0 bioconductor-stabmap -y
> ```

> **⚠️ If an error occurs with the version specification**:
> ```bash
> mamba install -c bioconda -c conda-forge bioconductor-biocstyle bioconductor-stabmap -y
> ```

---

## 4. Launching Jupyter Notebook

Launch Jupyter Notebook, which was installed in Step A. The procedure is the same for both macOS and Windows.

```bash
# Activate the environment (if not already done)
conda activate scworkshop

# Launch Jupyter Notebook
jupyter notebook
```

Upon execution, your web browser will open automatically and display the Jupyter Notebook interface.

> **💡 Creating a new notebook**: You can create a new R notebook by selecting "R" from the "New" menu in the top right corner. You can run R code and proceed with your data analysis in the created notebook.
> 
> **⚠️ Note**: Do not launch Jupyter Notebook from a desktop shortcut. Always launch it from the Terminal / Miniforge Prompt using the command above. This ensures that the R in the correct environment (`scworkshop`) is used.

#### (Alternative) Using R Directly Without Jupyter

If the above method doesn't work, you can launch R directly from the Miniforge Prompt:

```bash
conda activate scworkshop
R
```

The R console will appear, so you can enter and execute commands directly.

---

## 5. Preparing Data

### A. Creating a Project Folder

Create a project folder on your Desktop.

#### For macOS (Terminal)
```bash
# Navigate to Desktop
cd ~/Desktop

# Create a folder
mkdir SingleCellWorkshop
cd SingleCellWorkshop

# Create a data folder
mkdir data
```

#### For Windows (Miniforge Prompt)
```bash
# Navigate to Desktop
cd %USERPROFILE%\Desktop

# Create a folder
mkdir SingleCellWorkshop
cd SingleCellWorkshop

# Create a data folder
mkdir data
```

#### Folder Structure Image
The structure after creation will look like this:
```
SingleCellWorkshop/
│── JIAsyno_CITEseq_T_NK.rds                  (Data for T-cell analysis)
├── single_cell_analysis.Rmd                  (Part 1: Overall analysis)
└── single_cell_analysis_part2_Tcell.Rmd      (Part 2: T-cell subclass analysis)

```

### B. Downloading Data

This lecture uses data from **GSE278962** (JIA Synovial CITE-seq).
- Download the `.rds` files distributed in the lecture material from [here](https://drive.google.com/drive/folders/18LeP2pMmQd7oYmMOLgW2Cr2P6e52UrSj?usp=sharing) and place them in the `data/` folder mentioned above.
- Check the lecture material for how to set the working directory.

---

## 6. Verifying Installation

Create a new R notebook in Jupyter Notebook (see Section 4) or launch R directly from the command line, and execute the following script.

```r
# --- Version Verification Script ---

cat("=== R Version ===\n")
cat(R.version.string, "\n\n")

cat("=== Package Version Verification ===\n")

# Check required packages (used in single_cell_analysis_T.Rmd)
packages <- c("Seurat", "BiocStyle", "patchwork", "ggplot2", "harmony", "symphony",
              "lisi", "StabMap", "dplyr", "magrittr", "knitr", "rmarkdown")

for (pkg in packages) {
  tryCatch({
    library(pkg, character.only = TRUE)
    cat(sprintf("  ✓ %-15s : %s\n", pkg, packageVersion(pkg)))
  }, error = function(e) {
    cat(sprintf("  ✗ %-15s : Not installed!\n", pkg))
  })
}

cat("\n=== Result ===\n")
cat("If all of the above packages are marked with ✓, you are ready!\n")
```

### Expected Output (Reference)

| Package | Expected Version |
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

> **💡 Note**: Small differences in minor versions (e.g., 1.1.3 vs. 1.1.4) are not an issue.
> If the major version differs (e.g., Seurat 5.x.x vs. 4.x.x), please reinstall.

---

## 7. Troubleshooting

### Q1. `conda` Command Not Found

**For macOS:**
Close the Terminal and reopen it. If that doesn't resolve it:
```bash
# Initialize the shell
~/miniforge3/bin/conda init
```
Then restart the Terminal.

**For Windows:**
Use the **Miniforge Prompt** instead of the regular Command Prompt.
Search for "Miniforge Prompt" in the Start Menu and open it.

### Q2. `conda activate scworkshop` Doesn't Work

Execute:
```bash
conda init
```
Then **close and reopen** the Terminal / Miniforge Prompt.

### Q3. Error During Package Installation

**If it cannot be solved with Mamba**, try installing directly from within R:

```bash
conda activate scworkshop
R
```

After launching R:

```r
# For CRAN packages
install.packages("package_name")

# If you want to install a specific version (example: Seurat 5.2.1)
install.packages("remotes")
remotes::install_version("Seurat", version = "5.2.1")

# For Bioconductor packages
if (!require("BiocManager", quietly = TRUE)) install.packages("BiocManager")
BiocManager::install("package_name")
```

**If a compilation error occurs on macOS:**
Xcode Command Line Tools are required. Run the following in the Terminal:
```bash
xcode-select --install
```

**If a compilation error occurs on Windows:**
Rtools may be required:
```bash
mamba install -c conda-forge m2w64-toolchain -y
```

### Q4. Cannot Select R in Jupyter Notebook / R Version Is Different

Make sure to run `conda activate scworkshop` and launch using the `jupyter notebook` command.
Launching from another shortcut might use a different R or Python environment.

### Q5. Out of Memory Error Occurs

- Close unnecessary applications.
- Delete unnecessary objects in your code and run garbage collection (`rm(object_name); gc()`).

---

## 8. Alternative: Environment Setup Without Mamba (Conda)

If setting up the environment using Mamba consistently fails, you can install R and Jupyter Notebook (or RStudio) separately and install the packages manually.

### A. Installing R
1. Access CRAN (The Comprehensive R Archive Network): https://cran.r-project.org/
2. Download and run the R installer for your OS (macOS or Windows). To match the lecture environment, search for and install **R 4.3.2** from the past releases (the latest version may work, but outputs such as plots might differ slightly).

### B. Installing RStudio
1. Access the official Posit website: https://posit.co/download/rstudio-desktop/
2. Download and install the free version of RStudio Desktop.

### C. Setting up Jupyter Notebook
To use a standalone installed R inside Jupyter Notebook, follow these steps:
1. Open the R console in an environment where Python/Jupyter is already installed.
2. Run the following commands to install IRkernel:
   ```r
   install.packages("IRkernel")
   IRkernel::installspec(user = FALSE)
   ```
3. You will now be able to select R when creating a new Jupyter Notebook.

### D. Manual Installation of R Packages
Launch a Jupyter Notebook with the R kernel (or the R console) and execute the following commands to install the necessary packages.

```r
# Install CRAN packages
install.packages(c("patchwork", "ggplot2", "harmony", "symphony", "dplyr", "magrittr", "rmarkdown", "knitr", "bookdown"))

# Install Seurat version 5.2.1 specifically
# Use the remotes package to specify the Seurat version
if (!require("remotes", quietly = TRUE)) install.packages("remotes")
remotes::install_version("Seurat", version = "5.2.1")

# Install LISI and StabMap from GitHub
remotes::install_github("immunogenomics/LISI")
remotes::install_github("SydneyBioX/StabMap")

# Install Bioconductor packages
if (!requireNamespace("BiocManager", quietly = TRUE)) {
    install.packages("BiocManager")
}
BiocManager::install("BiocStyle")
```

Once all installations are complete, run the script in Section 6 "Verifying Installation" to check if everything is installed correctly.

---

## 9. Reference: Full sessionInfo() Output

This is the complete information for the environment that generated `single_cell_analysis_T.html`. Please refer to it during troubleshooting.

```
R version 4.3.2 (2023-10-31)
Platform: aarch64-apple-darwin20 (64-bit)
Running under: macOS 26.2

attached base packages:
 stats     graphics  grDevices utils     datasets  methods   base     

other attached packages:
 patchwork_1.1.3    magrittr_2.0.3     Seurat_5.2.1
 SeuratObject_5.0.2 sp_2.1-2           BiocStyle_2.30.0  
```
