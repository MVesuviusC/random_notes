# yaml header
---
title: "Title of Stuff"
author: "Matthew V. Cannon"
date: "`r format(Sys.time(), '%m/%d/%Y')`"
format:
    html:
        toc: true
        toc_float: true
        toc_depth: 5
        number_sections: false
        code-fold: true
        code-link: true
        df-print: kable
execute:
    cache: true
knitr:
    opts_chunk:
        cache.lazy: false
        cache.vars: ""
        warning: false
        message: false
lightbox: true
---

# To specify cache variables inside a chunk
```{r}
#| cache.vars: one_var
code
```

```{r}
#| cache.vars: c(one_var, another_var)
code
```

# Including child documents
{{< include child_qmds/helper_functions.qmd >}}
