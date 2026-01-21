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

# OpenSSL error when installing a package




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
