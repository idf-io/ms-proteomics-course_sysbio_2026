# MS Proteomics Analysis Tutorial — Ependymoma Cohort

**Systems Biology M.Sc. — End-to-End Proteomics Course**
*Neuropathology Department, University Hospital Heidelberg*

This repository is the **bioinformatics analysis tutorial** of the end-to-end mass-spectrometry (MS) proteomics course taught in the Systems Biology master's programme. The course walks you through the full proteomics workflow — from sample acquisition and digestion, through LC-MS data acquisition and DIA-NN search, to the computational analysis you find here.

The notebook in [code/analysis_ependymoma.ipynb](code/analysis_ependymoma.ipynb) is the **hands-on tutorial**: it takes you from a raw DIA-NN `report.parquet` to differential abundance and pathway enrichment on a real ependymoma cohort, with "your turn" prompts along the way.

## Course context

This tutorial is the **analysis step** in a longer pipeline you will see across the course:

1. Sample acquisition & preparation (wet lab)
2. LC-MS/MS data acquisition (DIA)
3. Spectral search with DIA-NN
4. **Bioinformatics analysis** ← *you are here*

## What the tutorial covers

- Reading and filtering DIA-NN long-format output
- Building a sample × protein matrix with [`proteopy`](https://proteopy.readthedocs.io/) and [`AnnData`](https://anndata.readthedocs.io/)
- Quality control: contaminant removal, sample / protein coverage filtering, CV inspection
- Preprocessing: log2 transform, median normalization, downshift imputation
- Exploratory analysis: sample-sample correlation, PCA, UMAP (via [`scanpy`](https://scanpy.readthedocs.io/))
- Differential protein abundance (two-sample t-test, BH correction)
- Gene-set enrichment with [`decoupler`](https://decoupler.readthedocs.io/) against MSigDB

## Requirements

- A **laptop** you can bring to the practical session.
- **Linux** strongly recommended (native or via **WSL2** on Windows). macOS works. Plain Windows can also work but we can advise you best in Linux.
- **mamba** or **conda** for environment management (instructions below).
- **VSCode** with the Jupyter extension.

## Setup

### 1. Install mamba

Follow the official Miniforge-based instructions from the mamba docs:

➡️ **[mamba installation guide](https://mamba.readthedocs.io/en/latest/installation/mamba-installation.html)**

These instructions install the [Miniforge](https://github.com/conda-forge/miniforge) distribution, which ships `mamba` with `conda-forge` set as the default channel. Pick the installer for your OS (Linux / WSL2 recommended) and follow the prompts, then **restart your shell** (or `source ~/.bashrc` / `~/.zshrc`).

Verify the install:

```bash
mamba --version
```

If you already have `conda` but not `mamba`, you can also use it as they share almost identical syntax.

### 2. Create the course environment

Create a dedicated environment for the tutorial. We pin Python 3.10 and pre-install `pip` and `ipykernel` (the kernel package VSCode talks to):

```bash
mamba create -n sysbio-proteomics "python=3.10" pip ipykernel -c conda-forge
mamba activate sysbio-proteomics
```

### 3. Install the analysis packages with pip

With the environment **activated**, install `proteopy`, `scanpy` and `decoupler` from PyPI:

```bash
pip install git+https://github.com/UKHD-NP/proteopy.git
pip install scanpy decoupler
```

This will pull in their dependencies (`anndata`, `numpy`, `pandas`, `scipy`, `matplotlib`, `seaborn`, `pyarrow`, …) automatically.

### 4. Register the environment as a Jupyter kernel (so VSCode finds it)

VSCode's Jupyter extension discovers kernels via `ipykernel`. Register this environment as a named kernel:

```bash
python -m ipykernel install --user --name sysbio-proteomics
```

### 5. Open the notebook in VSCode

1. Install the **Python** and **Jupyter** extensions in VSCode (search the Extensions panel).
2. Clone this repository to your local system and open it in VSCode: `File → Open Folder…`
3. **Make your own working copy** of the tutorial notebook so the original stays clean and you can always diff against it:

   ```bash
   cp code/analysis_ependymoma.ipynb code/analysis_ependymoma_<your-name>.ipynb
   ```

   Open the copy (`code/analysis_ependymoma_<your-name>.ipynb`) — this is the file you will edit and run during the session.
4. Click the kernel picker in the top-right of the notebook and select **Python (sysbio-proteomics)**.
   - If you don't see it, run `Jupyter: Select Interpreter to Start Jupyter Server` from the command palette (`Ctrl+Shift+P`) and pick the `sysbio-proteomics` Python interpreter.
5. Run the first cell to verify the imports succeed.

## Repository layout

```
.
├── tutorials/ # Jupyter notebooks — start with analysis_ependymoma.ipynb
├── data/      # Dataset symlink + contaminant FASTA
└── results/   # Outputs you generate during the tutorial
```

## References

- [`proteopy` documentation](https://proteopy.readthedocs.io/)
- [`scanpy` documentation](https://scanpy.readthedocs.io/)
- [`decoupler` documentation](https://decoupler.readthedocs.io/)
- [`AnnData` documentation](https://anndata.readthedocs.io/)
- [DIA-NN](https://github.com/vdemichev/DiaNN)
- [MSigDB](https://www.gsea-msigdb.org/gsea/msigdb)

---

*Authors: Ian Dirk Fichtner, Dennis Friedel, Rushda Patel — Neuropathology, Heidelberg.*
