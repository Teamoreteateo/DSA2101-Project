DSA2101 Group Project
================

# Analysis of Taylor Swift Spotify Data

``` r
students <- data.frame(
  Group_Members = c("Chua Yong Sheng Joel", "Lim Zeen Kiat", "Robin Ghosh", "Timothy Teo Shao Jun"),
  Matriculation_Number = c("A_", "A_", "A0271671A", "A0272851B")
)

kable(students, col.names = c("Group Members", "Matriculation Number"), 
      caption = "Student Information")
```

| Group Members        | Matriculation Number |
|:---------------------|:---------------------|
| Chua Yong Sheng Joel | A\_                  |
| Lim Zeen Kiat        | A\_                  |
| Robin Ghosh          | A0271671A            |
| Timothy Teo Shao Jun | A0272851B            |

Student Information

=======

## Introduction

(To start on) Reminders: Define musical attributes (features of the
songs) in intro

## Loading Data

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
# write.csv(taylor_album_songs, "taylor_album_songs.csv", row.names = FALSE)
# write.csv(taylor_all_songs, "taylor_all_songs.csv", row.names = FALSE)
# write.csv(taylor_albums, "taylor_albums.csv", row.names = FALSE)
# >>>>>> e3542bd358543883b147a6664c5777a5ed2ce66e
```

## Description of our Data

``` r
summary(taylor_album_songs)
```

    ##   album_name            ep          album_release         track_number  
    ##  Length:194         Mode :logical   Min.   :2006-10-24   Min.   : 1.00  
    ##  Class :character   FALSE:194       1st Qu.:2017-11-10   1st Qu.: 5.00  
    ##  Mode  :character                   Median :2020-07-24   Median :10.00  
    ##                                     Mean   :2018-06-21   Mean   :10.71  
    ##                                     3rd Qu.:2021-11-12   3rd Qu.:15.00  
    ##                                     Max.   :2022-10-21   Max.   :30.00  
    ##                                                                         
    ##   track_name           artist           featuring         bonus_track    
    ##  Length:194         Length:194         Length:194         Mode :logical  
    ##  Class :character   Class :character   Class :character   FALSE:171      
    ##  Mode  :character   Mode  :character   Mode  :character   TRUE :23       
    ##                                                                          
    ##                                                                          
    ##                                                                          
    ##                                                                          
    ##  promotional_release  single_release       track_release         danceability  
    ##  Min.   :2010-10-04   Min.   :2006-06-19   Min.   :2006-06-19   Min.   :0.292  
    ##  1st Qu.:2011-11-02   1st Qu.:2011-06-25   1st Qu.:2017-09-07   1st Qu.:0.511  
    ##  Median :2016-06-26   Median :2017-09-23   Median :2020-07-24   Median :0.594  
    ##  Mean   :2016-08-03   Mean   :2015-12-30   Mean   :2018-06-28   Mean   :0.584  
    ##  3rd Qu.:2021-02-12   3rd Qu.:2020-03-12   3rd Qu.:2021-11-12   3rd Qu.:0.652  
    ##  Max.   :2022-10-25   Max.   :2022-11-29   Max.   :2022-10-21   Max.   :0.897  
    ##  NA's   :176          NA's   :158                               NA's   :3      
    ##      energy            key            loudness            mode      
    ##  Min.   :0.1310   Min.   : 0.000   Min.   :-15.434   Min.   :0.000  
    ##  1st Qu.:0.4465   1st Qu.: 2.000   1st Qu.: -9.326   1st Qu.:1.000  
    ##  Median :0.5800   Median : 5.000   Median : -6.937   Median :1.000  
    ##  Mean   :0.5745   Mean   : 4.686   Mean   : -7.518   Mean   :0.911  
    ##  3rd Qu.:0.7170   3rd Qu.: 7.000   3rd Qu.: -5.606   3rd Qu.:1.000  
    ##  Max.   :0.9500   Max.   :11.000   Max.   : -2.098   Max.   :1.000  
    ##  NA's   :3        NA's   :3        NA's   :3         NA's   :3      
    ##   speechiness       acousticness      instrumentalness       liveness      
    ##  Min.   :0.02310   Min.   :0.000191   Min.   :0.0000000   Min.   :0.03570  
    ##  1st Qu.:0.03080   1st Qu.:0.034600   1st Qu.:0.0000000   1st Qu.:0.09295  
    ##  Median :0.03960   Median :0.162000   Median :0.0000014   Median :0.11500  
    ##  Mean   :0.05831   Mean   :0.321225   Mean   :0.0039358   Mean   :0.14081  
    ##  3rd Qu.:0.05740   3rd Qu.:0.662000   3rd Qu.:0.0000399   3rd Qu.:0.15050  
    ##  Max.   :0.51900   Max.   :0.971000   Max.   :0.3480000   Max.   :0.59400  
    ##  NA's   :3         NA's   :3          NA's   :3           NA's   :3        
    ##     valence           tempo        time_signature   duration_ms    
    ##  Min.   :0.0382   Min.   : 68.53   Min.   :1.000   Min.   :148781  
    ##  1st Qu.:0.2535   1st Qu.: 99.98   1st Qu.:4.000   1st Qu.:209326  
    ##  Median :0.4040   Median :121.96   Median :4.000   Median :232107  
    ##  Mean   :0.4009   Mean   :125.99   Mean   :3.979   Mean   :237079  
    ##  3rd Qu.:0.5345   3rd Qu.:150.03   3rd Qu.:4.000   3rd Qu.:254448  
    ##  Max.   :0.9420   Max.   :208.92   Max.   :5.000   Max.   :613027  
    ##  NA's   :3        NA's   :3        NA's   :3       NA's   :3       
    ##   explicit         key_name          mode_name           key_mode        
    ##  Mode :logical   Length:194         Length:194         Length:194        
    ##  FALSE:172       Class :character   Class :character   Class :character  
    ##  TRUE :19        Mode  :character   Mode  :character   Mode  :character  
    ##  NA's :3                                                                 
    ##                                                                          
    ##                                                                          
    ##                                                                          
    ##   lyrics       
    ##  Mode:logical  
    ##  NA's:194      
    ##                
    ##                
    ##                
    ##                
    ## 

``` r
summary(taylor_all_songs)
```

    ##   album_name            ep          album_release         track_number  
    ##  Length:274         Mode :logical   Min.   :2006-10-24   Min.   : 1.00  
    ##  Class :character   FALSE:235       1st Qu.:2010-10-25   1st Qu.: 5.00  
    ##  Mode  :character   TRUE :12        Median :2019-08-23   Median :10.00  
    ##                     NA's :27        Mean   :2016-09-22   Mean   :10.37  
    ##                                     3rd Qu.:2021-04-09   3rd Qu.:15.00  
    ##                                     Max.   :2022-10-21   Max.   :30.00  
    ##                                     NA's   :27           NA's   :27     
    ##   track_name           artist           featuring         bonus_track    
    ##  Length:274         Length:274         Length:274         Mode :logical  
    ##  Class :character   Class :character   Class :character   FALSE:212      
    ##  Mode  :character   Mode  :character   Mode  :character   TRUE :35       
    ##                                                           NA's :27       
    ##                                                                          
    ##                                                                          
    ##                                                                          
    ##  promotional_release  single_release       track_release       
    ##  Min.   :2008-06-23   Min.   :2006-06-19   Min.   :2006-06-19  
    ##  1st Qu.:2010-10-09   1st Qu.:2011-04-19   1st Qu.:2012-01-18  
    ##  Median :2014-10-17   Median :2015-02-09   Median :2019-08-23  
    ##  Mean   :2015-04-04   Mean   :2015-03-29   Mean   :2016-09-29  
    ##  3rd Qu.:2020-04-03   3rd Qu.:2019-08-16   3rd Qu.:2021-04-09  
    ##  Max.   :2022-10-25   Max.   :2022-11-29   Max.   :2022-10-21  
    ##  NA's   :234          NA's   :213                              
    ##   danceability        energy            key            loudness      
    ##  Min.   :0.2920   Min.   :0.1180   Min.   : 0.000   Min.   :-15.910  
    ##  1st Qu.:0.5135   1st Qu.:0.4560   1st Qu.: 2.000   1st Qu.: -9.001  
    ##  Median :0.5980   Median :0.5930   Median : 5.000   Median : -6.918  
    ##  Mean   :0.5866   Mean   :0.5765   Mean   : 4.692   Mean   : -7.376  
    ##  3rd Qu.:0.6535   3rd Qu.:0.7205   3rd Qu.: 7.000   3rd Qu.: -5.398  
    ##  Max.   :0.8970   Max.   :0.9500   Max.   :11.000   Max.   : -2.098  
    ##  NA's   :11       NA's   :11       NA's   :11       NA's   :11       
    ##       mode         speechiness       acousticness      instrumentalness  
    ##  Min.   :0.0000   Min.   :0.02310   Min.   :0.000191   Min.   :0.000000  
    ##  1st Qu.:1.0000   1st Qu.:0.02990   1st Qu.:0.034600   1st Qu.:0.000000  
    ##  Median :1.0000   Median :0.03630   Median :0.157000   Median :0.000002  
    ##  Mean   :0.9087   Mean   :0.05328   Mean   :0.299749   Mean   :0.003664  
    ##  3rd Qu.:1.0000   3rd Qu.:0.05490   3rd Qu.:0.581000   3rd Qu.:0.000044  
    ##  Max.   :1.0000   Max.   :0.51900   Max.   :0.971000   Max.   :0.348000  
    ##  NA's   :11       NA's   :11        NA's   :11         NA's   :11        
    ##     liveness          valence           tempo        time_signature 
    ##  Min.   :0.03570   Min.   :0.0382   Min.   : 57.96   Min.   :1.000  
    ##  1st Qu.:0.09325   1st Qu.:0.2510   1st Qu.: 99.99   1st Qu.:4.000  
    ##  Median :0.11400   Median :0.3990   Median :122.88   Median :4.000  
    ##  Mean   :0.14052   Mean   :0.4046   Mean   :125.15   Mean   :3.966  
    ##  3rd Qu.:0.15050   3rd Qu.:0.5385   3rd Qu.:146.04   3rd Qu.:4.000  
    ##  Max.   :0.59400   Max.   :0.9420   Max.   :208.92   Max.   :5.000  
    ##  NA's   :11        NA's   :11       NA's   :11       NA's   :11     
    ##   duration_ms      explicit         key_name          mode_name        
    ##  Min.   :148781   Mode :logical   Length:274         Length:274        
    ##  1st Qu.:210898   FALSE:241       Class :character   Class :character  
    ##  Median :234516   TRUE :22        Mode  :character   Mode  :character  
    ##  Mean   :237384   NA's :11                                             
    ##  3rd Qu.:254554                                                        
    ##  Max.   :613027                                                        
    ##  NA's   :11                                                            
    ##    key_mode          lyrics       
    ##  Length:274         Mode:logical  
    ##  Class :character   NA's:274      
    ##  Mode  :character                 
    ##                                   
    ##                                   
    ##                                   
    ## 

``` r
summary(taylor_albums)
```

    ##   album_name            ep          album_release        metacritic_score
    ##  Length:14          Mode :logical   Min.   :2006-10-24   Min.   :67.00   
    ##  Class :character   FALSE:12        1st Qu.:2009-05-08   1st Qu.:75.25   
    ##  Mode  :character   TRUE :2         Median :2016-05-04   Median :78.00   
    ##                                     Mean   :2015-05-21   Mean   :79.25   
    ##                                     3rd Qu.:2020-11-06   3rd Qu.:85.00   
    ##                                     Max.   :2022-10-21   Max.   :91.00   
    ##                                                          NA's   :2       
    ##    user_score   
    ##  Min.   :8.200  
    ##  1st Qu.:8.375  
    ##  Median :8.500  
    ##  Mean   :8.583  
    ##  3rd Qu.:8.900  
    ##  Max.   :9.000  
    ##  NA's   :2

A brief description of the variables relevant to our project is given
below: (To start on)

## Data Cleaning

Start off by filtering for the columns that we want:

``` r
taylor_album_songs <- taylor_album_songs %>%
    select(album_name, album_release, track_name, loudness, mode, speechiness, acousticness, instrumentalness, valence, tempo, explicit)

taylor_all_songs <- taylor_all_songs %>%
    select(album_name, album_release, track_name, loudness, mode, speechiness, acousticness, instrumentalness, valence, tempo, explicit)

taylor_albums <- taylor_albums %>%
    select(!ep)
```

Group by albums:

``` r
taylor_album_songs <- taylor_album_songs %>%
  group_by(album_name)

taylor_all_songs <- taylor_all_songs %>%
  group_by(album_name)

taylor_albums <- taylor_albums %>%
  group_by(album_name)
```

Checking for NA values in each file:

``` r
taylor_album_songs_na_count <- sapply(taylor_album_songs, function(x) sum(is.na(x)))
print(taylor_album_songs_na_count)
```

    ##       album_name    album_release       track_name         loudness 
    ##                0                0                0                3 
    ##             mode      speechiness     acousticness instrumentalness 
    ##                3                3                3                3 
    ##          valence            tempo         explicit 
    ##                3                3                3

``` r
taylor_all_songs_na_count <- sapply(taylor_all_songs, function(x) sum(is.na(x)))
print(taylor_all_songs_na_count)
```

    ##       album_name    album_release       track_name         loudness 
    ##               27               27                0               11 
    ##             mode      speechiness     acousticness instrumentalness 
    ##               11               11               11               11 
    ##          valence            tempo         explicit 
    ##               11               11               11

``` r
taylor_albums_na_count <- sapply(taylor_albums, function(x) sum(is.na(x)))
print(taylor_albums_na_count)
```

    ##       album_name    album_release metacritic_score       user_score 
    ##                0                0                2                2

There are many occurences of NA data present. Hence, we will need to
remove them:

``` r
taylor_album_songs <- taylor_album_songs %>%
    na.omit()

taylor_all_songs <- taylor_all_songs %>%
    na.omit()

taylor_albums <- taylor_albums %>%
    na.omit()
```

Save cleaned data as new files:

``` r
write.csv(taylor_album_songs, "CSV files/taylor_album_songs_cleaned.csv", row.names = FALSE)
write.csv(taylor_all_songs, "CSV files/taylor_all_songs_cleaned.csv", row.names = FALSE)
write.csv(taylor_albums, "CSV files/taylor_albums.csv_cleaned", row.names = FALSE)
```

``` r
head(taylor_album_songs)
```

    ## # A tibble: 6 × 11
    ## # Groups:   album_name [1]
    ##   album_name   album_release track_name  loudness  mode speechiness acousticness
    ##   <chr>        <date>        <chr>          <dbl> <dbl>       <dbl>        <dbl>
    ## 1 Taylor Swift 2006-10-24    Tim McGraw     -6.46     1      0.0251      0.575  
    ## 2 Taylor Swift 2006-10-24    Picture To…    -2.10     1      0.0323      0.173  
    ## 3 Taylor Swift 2006-10-24    Teardrops …    -6.94     1      0.0231      0.288  
    ## 4 Taylor Swift 2006-10-24    A Place In…    -2.88     1      0.0324      0.051  
    ## 5 Taylor Swift 2006-10-24    Cold As You    -5.77     1      0.0266      0.217  
    ## 6 Taylor Swift 2006-10-24    The Outside    -4.06     1      0.0293      0.00491
    ## # ℹ 4 more variables: instrumentalness <dbl>, valence <dbl>, tempo <dbl>,
    ## #   explicit <lgl>

``` r
head(taylor_all_songs)
```

    ## # A tibble: 6 × 11
    ## # Groups:   album_name [1]
    ##   album_name   album_release track_name  loudness  mode speechiness acousticness
    ##   <chr>        <date>        <chr>          <dbl> <dbl>       <dbl>        <dbl>
    ## 1 Taylor Swift 2006-10-24    Tim McGraw     -6.46     1      0.0251      0.575  
    ## 2 Taylor Swift 2006-10-24    Picture To…    -2.10     1      0.0323      0.173  
    ## 3 Taylor Swift 2006-10-24    Teardrops …    -6.94     1      0.0231      0.288  
    ## 4 Taylor Swift 2006-10-24    A Place In…    -2.88     1      0.0324      0.051  
    ## 5 Taylor Swift 2006-10-24    Cold As You    -5.77     1      0.0266      0.217  
    ## 6 Taylor Swift 2006-10-24    The Outside    -4.06     1      0.0293      0.00491
    ## # ℹ 4 more variables: instrumentalness <dbl>, valence <dbl>, tempo <dbl>,
    ## #   explicit <lgl>

``` r
head(taylor_albums)
```

    ## # A tibble: 6 × 4
    ## # Groups:   album_name [6]
    ##   album_name   album_release metacritic_score user_score
    ##   <chr>        <date>                   <dbl>      <dbl>
    ## 1 Taylor Swift 2006-10-24                  67        8.5
    ## 2 Fearless     2008-11-11                  73        8.4
    ## 3 Speak Now    2010-10-25                  77        8.6
    ## 4 Red          2012-10-22                  77        8.5
    ## 5 1989         2014-10-27                  76        8.2
    ## 6 reputation   2017-11-10                  71        8.3

``` r
names(taylor_album_songs)==names(taylor_all_songs)
```

    ##  [1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE

``` r
checking_conflicts <- taylor_album_songs %>% anti_join(taylor_all_songs, by=names(taylor_all_songs))
head(checking_conflicts)
```

    ## # A tibble: 0 × 11
    ## # Groups:   album_name [0]
    ## # ℹ 11 variables: album_name <chr>, album_release <date>, track_name <chr>,
    ## #   loudness <dbl>, mode <dbl>, speechiness <dbl>, acousticness <dbl>,
    ## #   instrumentalness <dbl>, valence <dbl>, tempo <dbl>, explicit <lgl>

All the column names match for both tables. A tibble of no elements was
generated, thus confirming that all rows are present in either tables.
Since we can choose which table we want, we will use taylor_album_songs.

Next, since we are trying to compare popularity statistics by album, we
need to check which albums are present in `taylor_album_songs` and
`taylor_albums`.

``` r
names1 <- taylor_albums %>% select(album_name) %>% unique() %>% arrange(album_name)
names2 <- taylor_album_songs %>% select(album_name) %>% unique() %>% arrange(album_name)

comp_table=left_join(names1, names2, by=c("album_name"))

combined_table = rep(NA,length(comp_table$album_name))

for (i in 1:length(names1$album_name) ){
  if (names1$album_name[i] %in% names2$album_name) {
    combined_table[i] <- comp_table$album_name[i]
  }
}

combined_table = cbind(names1$album_name,combined_table)
knitr::kable(combined_table, col.names = c("Taylor Albums","Taylor Album Songs"), caption="Comparison of album names between the tables")
```

| Taylor Albums               | Taylor Album Songs          |
|:----------------------------|:----------------------------|
| 1989                        | 1989                        |
| Fearless                    | NA                          |
| Fearless (Taylor’s Version) | Fearless (Taylor’s Version) |
| Lover                       | Lover                       |
| Midnights                   | Midnights                   |
| Red                         | NA                          |
| Red (Taylor’s Version)      | Red (Taylor’s Version)      |
| Speak Now                   | Speak Now                   |
| Taylor Swift                | Taylor Swift                |
| evermore                    | evermore                    |
| folklore                    | folklore                    |
| reputation                  | reputation                  |

Comparison of album names between the tables

Checking the table generated, Fearless and Red do not appear in Taylor
Album Songs, thus they will not be considered for popularity comparison.
Next, we will aggregate statistics for each of the musical attributes by
taking their mean. Then, we will combine `taylor_album_songs` and
`taylor_albums` to tie the popularity metrics with each common album.

``` r
taylor_album_summary <- taylor_album_songs %>% group_by(album_name)%>% summarize(
  mean_loudness = mean(loudness),
  mean_mode = round(mean(mode)),
  mean_speechiness = mean(speechiness),
  mean_acousticness = mean(acousticness),
  mean_instrumentalness = mean(instrumentalness),
  mean_valence = mean(valence),
  mean_tempo = mean(tempo)
) %>% inner_join(taylor_albums, by=c("album_name")) %>% relocate(album_release, .after=album_name)
```

Metacritic scores and User scores are representative of the popularity
of the albums because they are averaged ratings of the albums from
critics and the public respectively (Source: NEED SOURCE metacritic.com,
another one).

Metacritic score ranges from 0 to 100 while User Score ranges from 0 to
10. We will aggregate these into one statistic, “Popularity”, weighted
by their respective ranges. (Source: NEED SOURCE)

Hence we use the following formula: Popularity =
(metacritic_score+(user_score\*10))/2

``` r
taylor_album_summary <- taylor_album_summary %>% mutate(Popularity = (metacritic_score+user_score*10)/2)
taylor_album_summary[, c(1,2,12)]
```

    ## # A tibble: 10 × 3
    ##    album_name                  album_release Popularity
    ##    <chr>                       <date>             <dbl>
    ##  1 1989                        2014-10-27          79  
    ##  2 Fearless (Taylor's Version) 2021-04-09          85.5
    ##  3 Lover                       2019-08-23          81.5
    ##  4 Midnights                   2022-10-21          84  
    ##  5 Red (Taylor's Version)      2021-11-12          90.5
    ##  6 Speak Now                   2010-10-25          81.5
    ##  7 Taylor Swift                2006-10-24          76  
    ##  8 evermore                    2020-12-11          87  
    ##  9 folklore                    2020-07-24          89  
    ## 10 reputation                  2017-11-10          77
