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

