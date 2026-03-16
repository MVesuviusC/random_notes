# raw strings
print(r"{this is not interpreted!@#$%^&*()}")

# Failure to install a package

```
> install.packages("gt")
--- Please select a CRAN mirror for use in this session ---
Warning: failed to download mirrors file (cannot open URL 'https://cran.r-project.org/CRAN_mirrors.csv'); using local file 'C:/Users/mvc002/AppData/Local/Programs/R/R-4.3.3/doc/CRAN_mirrors.csv'
Warning: unable to access index for repository https://cran.case.edu/src/contrib:
  cannot open URL 'https://cran.case.edu/src/contrib/PACKAGES'
Warning: unable to access index for repository https://cran.case.edu/bin/windows/contrib/4.3:
  cannot open URL 'https://cran.case.edu/bin/windows/contrib/4.3/PACKAGES'
Warning messages:
1: In download.file(url, destfile = f, quiet = TRUE) :
  URL 'https://cran.r-project.org/CRAN_mirrors.csv': status was 'SSL connect error'
2: package 'gt' is not available for this version of R

A version of this package for your version of R might be available elsewhere,
see the ideas at
https://cran.r-project.org/doc/manuals/r-patched/R-admin.html#Installing-packages 
> 
```

To fix, set `options(repos = c(CRAN = "https://cran.rstudio.com/"))`

## failed to install ragg due to missing webp and freetype2 in R 4.5.0
Package libwebp was not found in the pkg-config search path.
Perhaps you should add the directory containing `libwebp.pc'
to the PKG_CONFIG_PATH environment variable
Package 'libwebp', required by 'virtual:world', not found
Package 'libwebpmux', required by 'virtual:world', not found
Package libwebp was not found in the pkg-config search path.
Perhaps you should add the directory containing `libwebp.pc'
to the PKG_CONFIG_PATH environment variable
Package 'libwebp', required by 'virtual:world', not found
Package 'libwebpmux', required by 'virtual:world', not found
Using PKG_CFLAGS=
Using PKG_LIBS=-lfreetype -lpng16 -ltiff -lz -ljpeg -lbz2 -lwebp -lwebpmux
-----------------------------[ ANTICONF ]-------------------------------
Configuration failed to find one of freetype2 libpng libtiff-4 libjpeg libwebp libwebpmux. Try installing:
 * deb: libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev libwebp-dev (Debian, Ubuntu, etc)
 * rpm: freetype-devel libpng-devel libtiff-devel libjpeg-devel libwebp-devel (Fedora, CentOS, RHEL)
If freetype2 libpng libtiff-4 libjpeg libwebp libwebpmux is already installed, check that 'pkg-config' is in your
PATH and PKG_CONFIG_PATH contains a freetype2 libpng libtiff-4 libjpeg libwebp libwebpmux.pc file. If pkg-config
is unavailable you can set INCLUDE_DIR and LIB_DIR manually via:
R CMD INSTALL --configure-vars='INCLUDE_DIR=... LIB_DIR=...'
-------------------------- [ERROR MESSAGE] ---------------------------
<stdin>:1:10: fatal error: ft2build.h: No such file or directory
compilation terminated.
--------------------------------------------------------------------
ERROR: configuration failed for package ‘ragg’
* removing ‘/gpfs0/home2/gdrobertslab/mvc002/R/x86_64-pc-linux-gnu-library/4.5/ragg’

The downloaded source packages are in
        ‘/tmp/Rtmp5FaIpC/downloaded_packages’
Warning message:
In install.packages("ragg") :
  installation of package ‘ragg’ had non-zero exit status

### Fix by:
load R
make sure GCC version is compatable with freetype2

ml GCC/9.3.0  OpenMPI/4.0.3 R/4.5.0
ml load webp
ml load freetype/2.10.1
ml load Glib/2.64.1 # was getting message about missing glib
####This wasn't populated properly for some reason
export PKG_CONFIG_PATH=/export/apps/opt/webp/1.6.0/lib/pkgconfig:${PKG_CONFIG_PATH}

start R, install.packages("ragg")

## OpenSSL error when installing a package
*** installing help indices
** building package indices
** testing if installed package can be loaded from temporary location
Error: package or namespace load failed for ‘arrow’ in dyn.load(file, DLLpath = DLLpath, ...):
 unable to load shared object '/gpfs0/home2/gdrobertslab/mvc002/R/x86_64-pc-linux-gnu-library/4.3/00LOCK-arrow/00new/arrow/libs/arrow.so':
  libssl.so.3: cannot open shared object file: No such file or directory
Error: loading failed
Execution halted
ERROR: loading failed
* removing ‘/gpfs0/home2/gdrobertslab/mvc002/R/x86_64-pc-linux-gnu-library/4.3/arrow’
Warning message:
In i.p(...) :
  installation of package ‘/tmp/Rtmp0lZShJ/file18cb322f786c9e/arrow_21.0.0.tar.gz’ had non-zero exit status
> q()

Fix with:
module load OpenSSL/3

# Debugging

`browser()` can put into a function to initiate a debug session at a certain point

```{r}
do_stuff <- function(x) {
    x <- x * 3

    browser()

    x <- x - 1

    return(x)
}
```

`options(error = recover)` will make R stop and debug when it hits an error

22.5.1 dump.frames()
dump.frames() is the equivalent to recover() for non-interactive code; it saves a last.dump.rda file in the working directory. Later, an interactive session, you can load("last.dump.rda"); debugger() to enter an interactive debugger with the same interface as recover(). This lets you “cheat”, interactively debugging code that was run non-interactively.

```
# In batch R process ----
dump_and_quit <- function() {
  # Save debugging info to file last.dump.rda
  dump.frames(to.file = TRUE)
  # Quit R with error status
  q(status = 1)
}
options(error = dump_and_quit)

# In a later interactive session ----
load("last.dump.rda")
debugger()
```

# Lintr
when lintr decides you should be using 2 spaces instead of 4:
Add this to your .lintr file (in project folder or home)
Make sure you have a \n after the code
```
linters: linters_with_defaults(
    indentation_linter(indent = 4L))
```
From https://stackoverflow.com/questions/75766381/how-to-set-tab-size-equivalent-of-4-spaces-in-lintr-configuration-file


# R hover links/documentation not working
This stopped working for me when working on the server. I think the ultimate issue was that the R languageserver was not being started. What fixed this was manually starting an R terminal/interactive session, which started the languageserver. I could then close the terminal/interactive session and it still worked. I'm not sure what changed to make this stop working.

# RV package manager
This manages R packages on a project-by-project basis

[documentation](https://a2-ai.github.io/rv-docs/reference/faq/)

It uses whatever R version is in your path when you run rv init

```
rv init
```

Make sure to populate repositories in rproject.toml
```
repositories = [
    { alias = "CRAN", url = "https://cran.rstudio.com/" },
    { alias = "PPM", url = "https://packagemanager.posit.co/cran/latest" },
    { alias = "bioconductor", url = "https://bioconductor.org/packages/3.22/bioc" },
    { alias = "BioCann", url = "https://bioconductor.org/packages/3.22/data/annotation"},
    { alias = "BioCexp", url = "https://bioconductor.org/packages/3.22/data/experiment" },
    { alias = "BioCworkflows", url = "https://bioconductor.org/packages/3.22/workflows"},
]
```

```
rv add tidyverse

// The modules here are just for my HPC
ml load CMake/3.16.4 XZ/5.2.5 NLopt/2.6.1 webp freetype/2.10.1
rv add nichenetr --git https://github.com/saeyslab/nichenetr --tag v2.2.0
```

or add the packages to the rproject.toml so it can find compatible versions all at once
```
dependencies = [
    "tidyverse",
    { name = "ggrepel", git = "https://github.com/slowkow/ggrepel", tag = "0.9.6"},
    "Seurat",
    { name = "nichenetr", git = "https://github.com/saeyslab/nichenetr", tag = "v2.2.0" },
    "ComplexHeatmap",
    { name = "qs", git = "https://github.com/qsbase/qs", branch = "master" },
    { name = "rrrSingleCellUtils", git = "https://github.com/kidcancerlab/rrrSingleCellUtils", tag = "v0.21.0" },
    "celldex",
    "qs2",
    "hdf5r",
    "SingleCellExperiment",
    "scrapper",
    "rmarkdown",
    "jsonlite",
]
```

use `rv plan` to make sure it can find compatible versions

use `rv sync` to install everything

`Failed to load config at `.`` likely means a syntax error in rproject.toml

make sure to add these files to your github:
- rv/scripts/activate.R
- rv/scripts/rvr.R
- rv/.gitignore
- .Rprofile
- rv.lock
- rproject.toml

When cloning a new repo that used rv:
- clone
- cd into the repo
- rv sync to install all packages

In vscode to keep from getting angry messages about your R version not matching your R version path in the settings, change your settings json:
"r.rterm.linux": "R",
    "r.rpath.linux": "R",

Making the .vscode/settings.json folder for each project is a way around this. The root cause seems to be the vscode has multiple processes running in the background for linting, languageserver, etc.. and they key off of the R version in the R extension settings.