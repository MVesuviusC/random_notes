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

