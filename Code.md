DSA2101 Taylor Swifties
================

## GitHub Documents

Testing! Nullus This is an R Markdown format used for publishing
markdown documents to GitHub. When you click the **Knit** button all R
code chunks are run and a markdown file (.md) suitable for publishing to
GitHub is generated.

## Including Code

You can include R code in the document as follows:

``` r
install.packages("tidytuesdayR")
```

    ## # Downloading packages -------------------------------------------------------
    ## - Downloading tidytuesdayR from CRAN ...        OK [84 Kb in 0.81s]
    ## - Downloading gh from CRAN ...                  OK [115.2 Kb in 1.2s]
    ## - Downloading gitcreds from CRAN ...            OK [93.7 Kb in 0.93s]
    ## - Downloading httr2 from CRAN ...               OK [601 Kb in 0.43s]
    ## - Downloading ini from CRAN ...                 OK [13.3 Kb in 0.29s]
    ## - Downloading usethis from CRAN ...             OK [884.3 Kb in 0.59s]
    ## - Downloading gert from CRAN ...                OK [2.8 Mb in 2.1s]
    ## - Downloading credentials from CRAN ...         OK [214.3 Kb in 1.3s]
    ## - Downloading zip from CRAN ...                 OK [202.9 Kb in 1.1s]
    ## - Downloading whisker from CRAN ...             OK [64.4 Kb in 0.72s]
    ## Successfully downloaded 10 packages in 11 seconds.
    ## 
    ## The following package(s) will be installed:
    ## - credentials  [2.0.2]
    ## - gert         [2.1.4]
    ## - gh           [1.4.1]
    ## - gitcreds     [0.1.2]
    ## - httr2        [1.0.5]
    ## - ini          [0.3.1]
    ## - tidytuesdayR [1.1.2]
    ## - usethis      [3.0.0]
    ## - whisker      [0.4.1]
    ## - zip          [2.3.1]
    ## These packages will be installed into "~/Desktop/NUS/Y2S1/DSA2101/project/git/DSA2101-Project/renv/library/R-4.3/aarch64-apple-darwin20".
    ## 
    ## # Installing packages --------------------------------------------------------
    ## - Installing gitcreds ...                       OK [installed binary and cached]
    ## - Installing httr2 ...                          OK [installed binary and cached]
    ## - Installing ini ...                            OK [installed binary and cached]
    ## - Installing gh ...                             OK [installed binary and cached]
    ## - Installing credentials ...                    OK [installed binary and cached]
    ## - Installing zip ...                            OK [installed binary and cached]
    ## - Installing gert ...                           OK [installed binary and cached]
    ## - Installing whisker ...                        OK [installed binary and cached]
    ## - Installing usethis ...                        OK [installed binary and cached]
    ## - Installing tidytuesdayR ...                   OK [installed binary and cached]
    ## Successfully installed 10 packages in 0.93 seconds.

``` r
tuesdata <- tidytuesdayR::tt_load('2023-10-17')
```

    ## ---- Compiling #TidyTuesday Information for 2023-10-17 ----
    ## --- There are 3 files available ---
    ## 
    ## 
    ## ── Downloading files ───────────────────────────────────────────────────────────
    ## 
    ##   1 of 3: "taylor_album_songs.csv"
    ##   2 of 3: "taylor_all_songs.csv"
    ##   3 of 3: "taylor_albums.csv"

## Including Plots

``` r
# Test
```
