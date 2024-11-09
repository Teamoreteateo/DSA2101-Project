DSA2101 Group Project: Analysis of Taylor Swift Spotify Data
================

## Group members

``` r
students <- data.frame(
  Group_Members = c("Chua Yong Sheng Joel", "Lim Zeen Kiat", "Robin Ghosh", "Timothy Teo Shao Jun"),
  Matriculation_Number = c("A_", "A0273151M", "A0271671A", "A0272851B")
)

kable(students, col.names = c("Group Members", "Matriculation Number"))
```

| Group Members        | Matriculation Number |
|:---------------------|:---------------------|
| Chua Yong Sheng Joel | A\_                  |
| Lim Zeen Kiat        | A0273151M            |
| Robin Ghosh          | A0271671A            |
| Timothy Teo Shao Jun | A0272851B            |

## Loading Data

``` r
# Load the Taylor Swift datasets
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
taylor_album_songs <- tuesdata$taylor_album_songs
taylor_all_songs <- tuesdata$taylor_all_songs
taylor_albums <- tuesdata$taylor_albums

# Save each dataset to new CSV files
# write.csv(taylor_album_songs, "taylor_album_songs.csv", row.names = FALSE)
# write.csv(taylor_all_songs, "taylor_all_songs.csv", row.names = FALSE)
# write.csv(taylor_albums, "taylor_albums.csv", row.names = FALSE)
```

=======

## 1. Introduction

Taylor Swift’s music has had a significant impact on the global pop and
country music scenes. In this project, we used the Taylor Swift Spotify
data sourced from the TidyTuesday repository to analyse the musical and
lyrical features of her music. This dataset includes detailed audio
attributes of her songs which we aim to use in this project. By
exploring the given data, we aim to discover the patterns that have
emerged throughout Taylor Swift’s career and gain insights into how her
music has evolved.

For our project, we aim to answer the following question:

``` bash
How do musical and lyrical features evolve across Taylor Swift's albums, and what insights can we gain about the progression of her artistry over time?
```

This analysis will allow us to identify trends in musical
characteristics and examine how stylistic changes have corresponded with
different stages of her career. By doing so, we hope to provide a deeper
understanding of how Taylor Swift has redefined her career and connect
with a global audience.

## 2. Data Cleaning & Summary

### Summary of relevant statistics

The following table contains a brief description of the variables we
have chosen to use for our project:

``` r
track_attributes <- data.frame(
  Variable = c(
    "album_name", "album_release", "track_name", "danceability", "energy", 
    "loudness", "mode", "speechiness", "acousticness", "instrumentalness", 
    "liveness", "valence", "tempo", "explicit"
  ),
  Class = c(
    "character", "double", "character", "double", "double", "double", "integer",
    "double", "double", "double", "double", "double", "double", "logical"
  ),
  Description = c(
    "Name of the album the track belongs to.",
    "Release date of the album.",
    "Name of the individual track.",
    "Measures how suitable a track is for dancing.",
    "Measures the intensity and activity of a track, with higher values indicating more energetic sounds.",
    "The overall volume of the track, measured in decibels (dB).",
    "This is also a categorical variable. It indicates the modality of the track: 1 for major, 0 for minor.",
    "Measures the presence of spoken words in a track, with higher values indicating more speech-like content.",
    "Represents the likelihood that the track is acoustic, with higher values indicating more acoustic qualities.",
    "Predicts whether a track is instrumental, with higher values suggesting a lack of vocals.",
    "Measures the presence of an audience in the recording, with higher values indicating a stronger likelihood that the track is live",
    "Describes the musical positiveness conveyed, with higher values indicating more cheerful and happy tones.",
    "The speed of the track, measured in beats per minute (BPM).",
    "Indicates whether the track contains explicit content: 1 for explicit, 0 otherwise."
  )
)
kable(track_attributes, col.names = c("Variable", "Class", "Description"), align = "l")
```

| Variable | Class | Description |
|:---|:---|:---|
| album_name | character | Name of the album the track belongs to. |
| album_release | double | Release date of the album. |
| track_name | character | Name of the individual track. |
| danceability | double | Measures how suitable a track is for dancing. |
| energy | double | Measures the intensity and activity of a track, with higher values indicating more energetic sounds. |
| loudness | double | The overall volume of the track, measured in decibels (dB). |
| mode | integer | This is also a categorical variable. It indicates the modality of the track: 1 for major, 0 for minor. |
| speechiness | double | Measures the presence of spoken words in a track, with higher values indicating more speech-like content. |
| acousticness | double | Represents the likelihood that the track is acoustic, with higher values indicating more acoustic qualities. |
| instrumentalness | double | Predicts whether a track is instrumental, with higher values suggesting a lack of vocals. |
| liveness | double | Measures the presence of an audience in the recording, with higher values indicating a stronger likelihood that the track is live |
| valence | double | Describes the musical positiveness conveyed, with higher values indicating more cheerful and happy tones. |
| tempo | double | The speed of the track, measured in beats per minute (BPM). |
| explicit | logical | Indicates whether the track contains explicit content: 1 for explicit, 0 otherwise. |

### Data cleaning

We’ll start off by filtering for the features listed above which we want
to keep:

``` r
taylor_album_songs <- taylor_album_songs %>%
    select(album_name, album_release, track_name, danceability, energy, loudness, mode, speechiness, acousticness, instrumentalness, liveness, valence, tempo, explicit)

taylor_all_songs <- taylor_all_songs %>%
    select(album_name, album_release, track_name, danceability, energy, loudness, mode, speechiness, acousticness, instrumentalness, liveness, valence, tempo, explicit)

taylor_albums <- taylor_albums %>%
    select(!ep)
```

Then, we group the data by their respective albums for aggregating album
data:

``` r
taylor_album_songs <- taylor_album_songs %>%
  group_by(album_name)

taylor_all_songs <- taylor_all_songs %>%
  group_by(album_name)

taylor_albums <- taylor_albums %>%
  group_by(album_name)
```

We will check and remove NA values in the data, which are not needed for
our visualisations:

``` r
taylor_album_songs_na_count <- sapply(taylor_album_songs, function(x) sum(is.na(x)))
print(taylor_album_songs_na_count)
```

    ##       album_name    album_release       track_name     danceability 
    ##                0                0                0                3 
    ##           energy         loudness             mode      speechiness 
    ##                3                3                3                3 
    ##     acousticness instrumentalness         liveness          valence 
    ##                3                3                3                3 
    ##            tempo         explicit 
    ##                3                3

``` r
taylor_all_songs_na_count <- sapply(taylor_all_songs, function(x) sum(is.na(x)))
print(taylor_all_songs_na_count)
```

    ##       album_name    album_release       track_name     danceability 
    ##               27               27                0               11 
    ##           energy         loudness             mode      speechiness 
    ##               11               11               11               11 
    ##     acousticness instrumentalness         liveness          valence 
    ##               11               11               11               11 
    ##            tempo         explicit 
    ##               11               11

``` r
taylor_albums_na_count <- sapply(taylor_albums, function(x) sum(is.na(x)))
print(taylor_albums_na_count)
```

    ##       album_name    album_release metacritic_score       user_score 
    ##                0                0                2                2

``` r
taylor_album_songs <- taylor_album_songs %>%
    na.omit()

taylor_all_songs <- taylor_all_songs %>%
    na.omit()

taylor_albums <- taylor_albums %>%
    na.omit()
```

To ensure that the visualisation representations are balanced, we will
check the range of the various song features to see whether there is a
need to scale the features:

``` r
# Checking range of values (for all the songs) of numerical variables in taylor_all_songs
ranges_taylor_all_songs <- taylor_all_songs %>%
  ungroup() %>%
  select(liveness:tempo) %>%
  summarise(across(liveness:tempo, ~paste(min(.), "to", max(.))))
glimpse(ranges_taylor_all_songs)
```

    ## Rows: 1
    ## Columns: 3
    ## $ liveness <chr> "0.0357 to 0.594"
    ## $ valence  <chr> "0.0382 to 0.942"
    ## $ tempo    <chr> "68.534 to 208.918"

``` r
# Checking range of values of numerical variables in taylor_album_songs
ranges_taylor_album_songs <- taylor_album_songs %>%
  select(liveness:tempo) %>%
  summarise(across(liveness:tempo, ~paste(min(.), "to", max(.))))
```

    ## Adding missing grouping variables: `album_name`

``` r
glimpse(ranges_taylor_album_songs)
```

    ## Rows: 10
    ## Columns: 4
    ## $ album_name <chr> "1989", "Fearless (Taylor's Version)", "Lover", "Midnights"…
    ## $ liveness   <chr> "0.0658 to 0.341", "0.0667 to 0.343", "0.0637 to 0.319", "0…
    ## $ valence    <chr> "0.107 to 0.942", "0.13 to 0.797", "0.166 to 0.865", "0.038…
    ## $ tempo      <chr> "92.008 to 184.014", "80.132 to 200.391", "68.534 to 207.47…

Loudness is measured in decibels, which has a logarithmic scale. To
enable a clearer visual representation, we will first convert the values
of loudness to a linear scale, before scaling the values between 0 and
1.

``` r
# Scaling loudness for taylor_all_songs
taylor_all_songs <- taylor_all_songs %>%
  ungroup() %>%
  mutate(loudness = 10^(loudness/10)) #%>%
  #mutate(normalised_intensity = (intensity - min(intensity)) / (max(intensity) - min(intensity)))
glimpse(taylor_all_songs)
```

    ## Rows: 238
    ## Columns: 14
    ## $ album_name       <chr> "Taylor Swift", "Taylor Swift", "Taylor Swift", "Tayl…
    ## $ album_release    <date> 2006-10-24, 2006-10-24, 2006-10-24, 2006-10-24, 2006…
    ## $ track_name       <chr> "Tim McGraw", "Picture To Burn", "Teardrops On My Gui…
    ## $ danceability     <dbl> 0.580, 0.658, 0.621, 0.576, 0.418, 0.589, 0.479, 0.59…
    ## $ energy           <dbl> 0.491, 0.877, 0.417, 0.777, 0.482, 0.805, 0.578, 0.62…
    ## $ loudness         <dbl> 0.2258396, 0.6168790, 0.2022553, 0.5151100, 0.2649110…
    ## $ mode             <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    ## $ speechiness      <dbl> 0.0251, 0.0323, 0.0231, 0.0324, 0.0266, 0.0293, 0.029…
    ## $ acousticness     <dbl> 0.57500, 0.17300, 0.28800, 0.05100, 0.21700, 0.00491,…
    ## $ instrumentalness <dbl> 0.00e+00, 0.00e+00, 0.00e+00, 0.00e+00, 0.00e+00, 0.0…
    ## $ liveness         <dbl> 0.1210, 0.0962, 0.1190, 0.3200, 0.1230, 0.2400, 0.084…
    ## $ valence          <dbl> 0.425, 0.821, 0.289, 0.428, 0.261, 0.591, 0.192, 0.50…
    ## $ tempo            <dbl> 76.009, 105.586, 99.953, 115.028, 175.558, 112.982, 1…
    ## $ explicit         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALS…

``` r
# Scaling loudness for taylor_album_songs
taylor_album_songs <- taylor_album_songs %>%
  mutate(loudness = 10^(loudness/10)) #%>%
  #mutate(normalised_intensity = (intensity - min(intensity)) / (max(intensity) - min(intensity)))
glimpse(taylor_album_songs)
```

    ## Rows: 191
    ## Columns: 14
    ## Groups: album_name [10]
    ## $ album_name       <chr> "Taylor Swift", "Taylor Swift", "Taylor Swift", "Tayl…
    ## $ album_release    <date> 2006-10-24, 2006-10-24, 2006-10-24, 2006-10-24, 2006…
    ## $ track_name       <chr> "Tim McGraw", "Picture To Burn", "Teardrops On My Gui…
    ## $ danceability     <dbl> 0.580, 0.658, 0.621, 0.576, 0.418, 0.589, 0.479, 0.59…
    ## $ energy           <dbl> 0.491, 0.877, 0.417, 0.777, 0.482, 0.805, 0.578, 0.62…
    ## $ loudness         <dbl> 0.2258396, 0.6168790, 0.2022553, 0.5151100, 0.2649110…
    ## $ mode             <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    ## $ speechiness      <dbl> 0.0251, 0.0323, 0.0231, 0.0324, 0.0266, 0.0293, 0.029…
    ## $ acousticness     <dbl> 0.57500, 0.17300, 0.28800, 0.05100, 0.21700, 0.00491,…
    ## $ instrumentalness <dbl> 0.00e+00, 0.00e+00, 0.00e+00, 0.00e+00, 0.00e+00, 0.0…
    ## $ liveness         <dbl> 0.1210, 0.0962, 0.1190, 0.3200, 0.1230, 0.2400, 0.084…
    ## $ valence          <dbl> 0.425, 0.821, 0.289, 0.428, 0.261, 0.591, 0.192, 0.50…
    ## $ tempo            <dbl> 76.009, 105.586, 99.953, 115.028, 175.558, 112.982, 1…
    ## $ explicit         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALS…

Finally, the cleaned data are saved as new files for us to reference
later on:

``` r
write.csv(taylor_album_songs, "CSV files/taylor_album_songs_cleaned.csv", row.names = FALSE)
write.csv(taylor_all_songs, "CSV files/taylor_all_songs_cleaned.csv", row.names = FALSE)
write.csv(taylor_albums, "CSV files/taylor_albums.csv_cleaned", row.names = FALSE)
```

### Data engineering

``` r
head(taylor_album_songs)
```

    ## # A tibble: 6 × 14
    ## # Groups:   album_name [1]
    ##   album_name   album_release track_name       danceability energy loudness  mode
    ##   <chr>        <date>        <chr>                   <dbl>  <dbl>    <dbl> <dbl>
    ## 1 Taylor Swift 2006-10-24    Tim McGraw              0.58   0.491    0.226     1
    ## 2 Taylor Swift 2006-10-24    Picture To Burn         0.658  0.877    0.617     1
    ## 3 Taylor Swift 2006-10-24    Teardrops On My…        0.621  0.417    0.202     1
    ## 4 Taylor Swift 2006-10-24    A Place In This…        0.576  0.777    0.515     1
    ## 5 Taylor Swift 2006-10-24    Cold As You             0.418  0.482    0.265     1
    ## 6 Taylor Swift 2006-10-24    The Outside             0.589  0.805    0.393     1
    ## # ℹ 7 more variables: speechiness <dbl>, acousticness <dbl>,
    ## #   instrumentalness <dbl>, liveness <dbl>, valence <dbl>, tempo <dbl>,
    ## #   explicit <lgl>

``` r
head(taylor_all_songs)
```

    ## # A tibble: 6 × 14
    ##   album_name   album_release track_name       danceability energy loudness  mode
    ##   <chr>        <date>        <chr>                   <dbl>  <dbl>    <dbl> <dbl>
    ## 1 Taylor Swift 2006-10-24    Tim McGraw              0.58   0.491    0.226     1
    ## 2 Taylor Swift 2006-10-24    Picture To Burn         0.658  0.877    0.617     1
    ## 3 Taylor Swift 2006-10-24    Teardrops On My…        0.621  0.417    0.202     1
    ## 4 Taylor Swift 2006-10-24    A Place In This…        0.576  0.777    0.515     1
    ## 5 Taylor Swift 2006-10-24    Cold As You             0.418  0.482    0.265     1
    ## 6 Taylor Swift 2006-10-24    The Outside             0.589  0.805    0.393     1
    ## # ℹ 7 more variables: speechiness <dbl>, acousticness <dbl>,
    ## #   instrumentalness <dbl>, liveness <dbl>, valence <dbl>, tempo <dbl>,
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

    ##  [1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE

``` r
checking_conflicts <- taylor_album_songs %>% anti_join(taylor_all_songs, by=names(taylor_all_songs))
head(checking_conflicts)
```

    ## # A tibble: 0 × 14
    ## # Groups:   album_name [0]
    ## # ℹ 14 variables: album_name <chr>, album_release <date>, track_name <chr>,
    ## #   danceability <dbl>, energy <dbl>, loudness <dbl>, mode <dbl>,
    ## #   speechiness <dbl>, acousticness <dbl>, instrumentalness <dbl>,
    ## #   liveness <dbl>, valence <dbl>, tempo <dbl>, explicit <lgl>

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
  mean_danceability = mean(danceability),
  mean_liveness = mean(liveness),
  mean_energy = mean(energy),
  mean_mode = round(mean(mode)),
  mean_speechiness = mean(speechiness),
  mean_acousticness = mean(acousticness),
  mean_instrumentalness = mean(instrumentalness),
  mean_valence = mean(valence),
  mean_tempo = mean(tempo)
) %>% inner_join(taylor_albums, by=c("album_name")) %>% relocate(album_release, .after=album_name)
```

Overall Metacritic scores and user scores are representative of the
popularity of the albums because they are the “weighted average” of the
albums’ individual scores from critics and the public respectively
(Metacritic, 2023).

While the Metacritic score ranges from 0 to 100 (Metacritic, 2023), the
user score has a different range of 0 to 10 which we noticed when
exploring the “user reviews” section (Metacritic, n.d.). We will
aggregate these into one statistic, “Popularity”, weighted by their
respective ranges.

Hence we use the following formula: Popularity =
(metacritic_score+(user_score\*10))/2

``` r
taylor_album_summary <- taylor_album_summary %>% mutate(Popularity = (metacritic_score+user_score*10)/2)
taylor_album_summary[, c(1,2,12)]
```

    ## # A tibble: 10 × 3
    ##    album_name                  album_release mean_tempo
    ##    <chr>                       <date>             <dbl>
    ##  1 1989                        2014-10-27          131.
    ##  2 Fearless (Taylor's Version) 2021-04-09          131.
    ##  3 Lover                       2019-08-23          120.
    ##  4 Midnights                   2022-10-21          120.
    ##  5 Red (Taylor's Version)      2021-11-12          124.
    ##  6 Speak Now                   2010-10-25          140.
    ##  7 Taylor Swift                2006-10-24          126.
    ##  8 evermore                    2020-12-11          121.
    ##  9 folklore                    2020-07-24          120.
    ## 10 reputation                  2017-11-10          128.

## 3. Visualisations

### a. What are the most significant features?

``` r
library(ggthemes)
x_labels = c("Acousticness", "Danceability", "Energy", "Instrumentalness", "Liveness", "Loudness", "Speechiness", "Valence")
taylor_long = taylor_album_songs %>% select(-mode) %>% pivot_longer(danceability:valence, values_to = "values", names_to = "labels") %>%
  mutate(labels = as.factor(labels))
ggplot(taylor_long, aes(y=labels, x=values, fill = labels)) +
  geom_boxplot(show.legend = F, alpha=0.55, outlier.fill = "lightblue", width = 0.4) +
  theme_economist() +
  labs(y= "", x = "Magnitude (From 0 to 1)", title = "The spread of each feature across all of Taylor Swift's album songs") +
  scale_y_discrete(labels = x_labels) +
  theme(
    axis.text.x = element_text(size = 10), 
    axis.text.y = element_text(size = 15),
    axis.title.x = element_text(vjust=-1, size=16),
    aspect.ratio = 0.6
  )
```

![](Code_files/figure-gfm/boxplot-1.png)<!-- -->

``` r
ggplot(taylor_long, aes(x=labels, y=values, fill = labels)) +
  geom_violin(bw=0.05, alpha=0.7, color = FALSE,
 trim=FALSE) +
 theme_fivethirtyeight()
```

![](Code_files/figure-gfm/violin-1.png)<!-- -->

### b. Have the features we selected influenced Taylor Swift’s popularity?

Exploratory plots: May or may not use, just to select

``` r
points = lm(data=taylor_album_summary, album_release~Popularity)
ggplot(taylor_album_summary, aes(x=album_release, y=Popularity, color=album_name)) +
  geom_point(size=2) 
```

![](Code_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

``` r
plotting = taylor_album_summary %>% pivot_longer(cols =c(mean_liveness, mean_danceability, mean_energy, mean_acousticness, mean_instrumentalness, mean_valence), names_to="variable", values_to="value")
ggplot(plotting, aes(x=album_release, y=value, color = variable)) +
  geom_point(size=3) +
  geom_smooth(method="lm") +
  theme_classic()
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](Code_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

``` r
ggplot(taylor_album_summary, aes(x=album_release, y=mean_loudness)) +
  geom_point(size=3) +
  geom_smooth(method="lm") +
  theme_classic()
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](Code_files/figure-gfm/loudness-plot-1.png)<!-- -->

Valence dropped, songs became sadder. Instrumentalness dropped, songs
became more vocally driven. Acousticness increased sharply, songs
involved lesser electronic sounds and sounded more raw. Energy of the
tracks have also reduced, further emphasising the mellow trend of her
music. Though, it appears that danceability has increased, meaning that
the downbeats in her songs have become more pronounced, creating a
danceable vibe. It seems that Taylor Swift aims to be more intimate with
her audience while maintaining vibe and groove to make up for the lack
of energy. Her popularity has increased over the years also, signalling
the yearn for such music.

### c. Which feature(s) has/have the greatest impact on Taylor Swift’s songs?

## 4. Discussions

### a.

### b.

### c.

## 5. Teamwork

## 6. References

Harmon, J. (2023, October 16). Taylor Swift. GitHub.
<https://github.com/rfordatascience/tidytuesday/blob/master/data/2023/2023-10-17/readme.md>  
Metacritic. (2023). How do you compute METASCORES? Retrieved November 7,
2024, from
<https://metacritichelp.zendesk.com/hc/en-us/articles/14478499933079-How-do-you-compute-METASCORES>  
Metacritic. (n.d.). Taylor Swift by Taylor Swift.
<https://www.metacritic.com/music/taylor-swift/taylor-swift/user-reviews>
