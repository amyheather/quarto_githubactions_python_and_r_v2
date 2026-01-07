# Quarto via GitHub actions (w/ Python and R code) - version 2

This repository demonstrates how to render and deploy a quarto site on GitHub pages via GitHub actions, when it contains executable Python and R code.

It attempts to address the issues encountered with incompatibility between conda system dependencies and the base rocker image in <https://github.com/amyheather/quarto_githubactions_python_and_r> by using a different base image.

1. Make `environment.yaml`.

2. Build conda environment.

```
conda env create --file environment.yaml
conda activate quarto_python_r_v2
```

3. Open RStudio and create `.Rproj`.

4. Create `DESCRIPTION`.

5. Set-up R environment with `renv` (if working from terminal, use `R` to open R console and `q()` to close it). Use `all` snapshot type.

```
renv::init()
renv::snapshot()
```

6. Create a simple quarto site.

7. If using VSCode, set Python interpreter.

8. Render quarto site locally. Some troubleshooting:

* Forcing/making sure it has installed rmarkdown and reticulate
* Downgrading Python and fixing package versions to deal with libtiff and jpeg compatability issue in recent conda stacks.
* Getting it to find Python and R... trying `engine: knitr`, `use_python()`, `use_condaenv()`, etc.

9. Publish on GitHub pages.

```
quarto publish gh-pages
```

10. Create `Dockerfile` and `.github/workflows/quarto.yaml`. Uses two step process: (a) create docker image and host on GHCR, and (b) render within docker. Why? To avoid the long process of setting up the environments everytime render - can move quicker if docker already has everything needed!