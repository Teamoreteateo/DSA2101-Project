DSA2101 Taylor Swifties
================

## Including Code

You can include R code in the document as follows:

``` r
library("tidytuesdayR")
```

    ## Warning: package 'tidytuesdayR' was built under R version 4.3.3

``` r
# Load the Tidy Tuesday datasets for Taylor Swift from Week 42 of 2023
tuesdata <- tidytuesdayR::tt_load(2023, week = 42)
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

``` r
# Extract specific datasets
taylor_album_songs <- tuesdata$taylor_album_songs
taylor_all_songs <- tuesdata$taylor_all_songs
taylor_albums <- tuesdata$taylor_albums

# Save each dataset to CSV files
write.csv(taylor_album_songs, "taylor_album_songs.csv", row.names = FALSE)
write.csv(taylor_all_songs, "taylor_all_songs.csv", row.names = FALSE)
write.csv(taylor_albums, "taylor_albums.csv", row.names = FALSE)
```

## Including Plots

You can also embed plots, for example:

``` r
#
```
