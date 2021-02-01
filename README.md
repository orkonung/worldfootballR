
<!-- README.md is generated from README.Rmd. Please edit that file -->

# worldfootballR <img src="man/figures/logo.png" align="right" width="181" height="201"/>

<!-- badges: start -->

[![Travis build
status](https://travis-ci.org/JaseZiv/worldfootballR.svg?branch=main)](https://travis-ci.org/JaseZiv/worldfootballR)
[![Codecov test
coverage](https://codecov.io/gh/JaseZiv/worldfootballR/branch/master/graph/badge.svg)](https://codecov.io/gh/JaseZiv/worldfootballR?branch=master)
<!-- badges: end -->

## Overview

This package is designed to allow users to extract various world
football results and player statistics data from fbref.com

## Installation

You can install the `worldfootballR` package from github with:

``` r
# install.packages("devtools")
devtools::install_github("JaseZiv/worldfootballR")
```

``` r
library(worldfootballR)
library(tidyverse)
```

## Usage

The functions available in this package are designed to enable the
extraction of world football data.

There are three main categories of data extract functions in this
package:

  - Match-level statistics (team and player)
  - Season-level statistics (team and player)
  - League / Team metadata

### Match-level statistics

#### Get match results

To get the match results (and additional metadata) for all games for a
tier-1 league season, the following function can be used:

``` r
# function to extract match results data
serieA_2020 <- get_match_results(country = "ITA", gender = "M", season_end_year = 2020)
#> [1] "Scraping match results"
#> [1] "Match results finished scraping"
glimpse(serieA_2020)
#> Rows: 380
#> Columns: 19
#> $ Competition_Name <chr> "Serie A", "Serie A", "Serie A", "Serie A", "Serie A…
#> $ Gender           <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M…
#> $ Country          <chr> "ITA", "ITA", "ITA", "ITA", "ITA", "ITA", "ITA", "IT…
#> $ Season_End_Year  <int> 2020, 2020, 2020, 2020, 2020, 2020, 2020, 2020, 2020…
#> $ Round            <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
#> $ Wk               <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2…
#> $ Day              <chr> "Sat", "Sat", "Sun", "Sun", "Sun", "Sun", "Sun", "Su…
#> $ Date             <date> 2019-08-24, 2019-08-24, 2019-08-25, 2019-08-25, 201…
#> $ Time             <chr> "18:00", "20:45", "18:00", "20:45", "20:45", "20:45"…
#> $ Home             <chr> "Parma", "Fiorentina", "Udinese", "Torino", "SPAL", …
#> $ HomeGoals        <dbl> 0, 3, 1, 2, 2, 3, 1, 0, 0, 4, 1, 1, 4, 1, 2, 1, 0, 1…
#> $ Home_xG          <dbl> 0.4, 1.7, 1.0, 1.2, 1.6, 1.9, 0.2, 1.0, 0.8, 1.7, 2.…
#> $ Away             <chr> "Juventus", "Napoli", "Milan", "Sassuolo", "Atalanta…
#> $ AwayGoals        <dbl> 1, 4, 0, 1, 3, 3, 1, 1, 3, 0, 0, 0, 3, 1, 3, 2, 1, 3…
#> $ Away_xG          <dbl> 1.3, 2.0, 0.5, 1.5, 1.7, 1.3, 1.6, 1.5, 2.3, 0.7, 0.…
#> $ Attendance       <dbl> 20073, 33614, 24584, 16536, 11706, 38779, 16324, 160…
#> $ Venue            <chr> "Stadio Ennio Tardini", "Stadio Artemio Franchi", "D…
#> $ Referee          <chr> "Fabio Maresca", "Davide Massa", "Fabrizio Pasqua", …
#> $ Notes            <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
```

**More than one league season**

The `get_match_results` function can be used to get data for multiple
seasons/leages/genders/etc also:

``` r
big_5_2020_results <- get_match_results(country = c("ENG", "ESP", "ITA", "GER", "FRA"),
                                        gender = "M", season_end_year = 2020)
```

#### Get match report

This function will return similar results to that of
`get_match_results()`, however `get_match_report()` will provide some
additional information. It will also only provide it for a single match,
not the whole season:

``` r
# function to extract match report data
liv_mci_2020 <- get_match_report(match_url = "https://fbref.com/en/matches/47880eb7/Liverpool-Manchester-City-November-10-2019-Premier-League")
glimpse(liv_mci_2020)
#> Rows: 1
#> Columns: 17
#> $ League         <chr> "Premier League"
#> $ Gender         <chr> "M"
#> $ Country        <chr> "ENG"
#> $ Season         <chr> "2019-2020"
#> $ Match_Date     <chr> "Sunday November 10, 2019"
#> $ Matchweek      <chr> "Premier League (Matchweek 12)"
#> $ Home_Team      <chr> "Liverpool"
#> $ Home_Formation <chr> "4-3-3"
#> $ Home_Score     <dbl> 3
#> $ Home_xG        <dbl> 1
#> $ Home_Goals     <chr> "\n\t\t\n\t\t\tFabinho · 6&rsquor; \n\t\t\n\t\t\tMoham…
#> $ Away_Team      <chr> "Manchester City"
#> $ Away_Formation <chr> "4-2-3-1"
#> $ Away_Score     <dbl> 1
#> $ Away_xG        <dbl> 1.3
#> $ Away_Goals     <chr> "\n\t\t\n\t\t\t Bernardo Silva · 78&rsquor;\n\t\t\n\t"
#> $ Game_URL       <chr> "https://fbref.com/en/matches/47880eb7/Liverpool-Manch…
```

#### Get match summaries

This function will return the main events that occur during a match,
including goals, substitutions and red/yellow cards:

``` r
# function to extract match summary data
liv_mci_2020_summary <- get_match_summary(match_url = "https://fbref.com/en/matches/47880eb7/Liverpool-Manchester-City-November-10-2019-Premier-League")
glimpse(liv_mci_2020_summary)
#> Rows: 10
#> Columns: 23
#> $ League            <chr> "Premier League", "Premier League", "Premier League…
#> $ Gender            <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M"
#> $ Country           <chr> "ENG", "ENG", "ENG", "ENG", "ENG", "ENG", "ENG", "E…
#> $ Season            <chr> "2019-2020", "2019-2020", "2019-2020", "2019-2020",…
#> $ Match_Date        <chr> "Sunday November 10, 2019", "Sunday November 10, 20…
#> $ Matchweek         <chr> "Premier League (Matchweek 12)", "Premier League (M…
#> $ Home_Team         <chr> "Liverpool", "Liverpool", "Liverpool", "Liverpool",…
#> $ Home_Formation    <chr> "4-3-3", "4-3-3", "4-3-3", "4-3-3", "4-3-3", "4-3-3…
#> $ Home_Score        <dbl> 3, 3, 3, 3, 3, 3, 3, 3, 3, 3
#> $ Home_xG           <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1
#> $ Home_Goals        <chr> "\n\t\t\n\t\t\tFabinho · 6&rsquor; \n\t\t\n\t\t\tMo…
#> $ Away_Team         <chr> "Manchester City", "Manchester City", "Manchester C…
#> $ Away_Formation    <chr> "4-2-3-1", "4-2-3-1", "4-2-3-1", "4-2-3-1", "4-2-3-…
#> $ Away_Score        <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1
#> $ Away_xG           <dbl> 1.3, 1.3, 1.3, 1.3, 1.3, 1.3, 1.3, 1.3, 1.3, 1.3
#> $ Away_Goals        <chr> "\n\t\t\n\t\t\t Bernardo Silva · 78&rsquor;\n\t\t\n…
#> $ Game_URL          <chr> "https://fbref.com/en/matches/47880eb7/Liverpool-Ma…
#> $ Team              <chr> "Liverpool", "Liverpool", "Liverpool", "Liverpool",…
#> $ Home_Away         <chr> "Home", "Home", "Home", "Home", "Away", "Away", "Aw…
#> $ event_time        <dbl> 6, 13, 51, 61, 65, 71, 78, 79, 87, 95
#> $ event_type        <chr> "Goal", "Goal", "Goal", "Substitute", "Yellow Card"…
#> $ event_players     <chr> "Fabinho", "Mohamed Salah Assist: Andrew Robertson"…
#> $ score_progression <chr> "1:0", "2:0", "3:0", "3:0", "3:0", "3:0", "3:1", "3…
```

#### Get advanced match statistics

The `get_advanced_match_stats` function allows the user to return a data
frame of different stat types for matches played.

Note, some stats may not be available for all leagues. The big five
European leagues should have all of these stats.

The following stat types can be selected:

  - *summary*
  - *passing*
  - *passing\_types*
  - *defense*
  - *possession*
  - *misc*
  - *keeper*

The function can be used for either all players individually:

``` r
test_urls_multiple <- c("https://fbref.com/en/matches/c0996cac/Bordeaux-Nantes-August-21-2020-Ligue-1",
                        "https://fbref.com/en/matches/9cbccb37/Dijon-Angers-August-22-2020-Ligue-1",
                        "https://fbref.com/en/matches/f96cd5a0/Lorient-Strasbourg-August-23-2020-Ligue-1")

advanced_match_stats <- get_advanced_match_stats(match_url = test_urls_multiple, stat_type = "possession", team_or_player = "player")
glimpse(advanced_match_stats)
#> Rows: 92
#> Columns: 48
#> $ League                <chr> "Ligue 1", "Ligue 1", "Ligue 1", "Ligue 1", "Li…
#> $ Gender                <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M…
#> $ Country               <chr> "FRA", "FRA", "FRA", "FRA", "FRA", "FRA", "FRA"…
#> $ Season                <chr> "2020-2021", "2020-2021", "2020-2021", "2020-20…
#> $ Match_Date            <chr> "Friday August 21, 2020", "Friday August 21, 20…
#> $ Matchweek             <chr> "Ligue 1 (Matchweek 1)", "Ligue 1 (Matchweek 1)…
#> $ Home_Team             <chr> "Bordeaux", "Bordeaux", "Bordeaux", "Bordeaux",…
#> $ Home_Formation        <chr> "4-3-3", "4-3-3", "4-3-3", "4-3-3", "4-3-3", "4…
#> $ Home_Score            <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
#> $ Home_xG               <dbl> 0.4, 0.4, 0.4, 0.4, 0.4, 0.4, 0.4, 0.4, 0.4, 0.…
#> $ Home_Goals            <chr> "\n\t\t\n\t\t\tMehdi Zerkane · 20&rsquor; \n\t\…
#> $ Away_Team             <chr> "Nantes", "Nantes", "Nantes", "Nantes", "Nantes…
#> $ Away_Formation        <chr> "4-4-2", "4-4-2", "4-4-2", "4-4-2", "4-4-2", "4…
#> $ Away_Score            <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
#> $ Away_xG               <dbl> 0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.…
#> $ Away_Goals            <chr> "\n\t\t\n\t", "\n\t\t\n\t", "\n\t\t\n\t", "\n\t…
#> $ Game_URL              <chr> "https://fbref.com/en/matches/c0996cac/Bordeaux…
#> $ Team                  <chr> "Bordeaux", "Bordeaux", "Bordeaux", "Bordeaux",…
#> $ Home_Away             <chr> "Home", "Home", "Home", "Home", "Home", "Home",…
#> $ Player                <chr> "Josh Maja", "Remi Oudin", "Hwang Ui-jo", "Samu…
#> $ Player_Num            <dbl> 9, 28, 18, 10, 12, 7, 26, 17, 5, 29, 25, 6, 24,…
#> $ Nation                <chr> "NGA", "FRA", "KOR", "NGA", "FRA", "FRA", "CRO"…
#> $ Pos                   <chr> "FW", "FW", "LW", "LW", "RW", "RW", "CM", "CM",…
#> $ Age                   <chr> "21-238", "23-277", "27-359", "22-361", "29-226…
#> $ Min                   <dbl> 45, 45, 74, 16, 63, 27, 90, 19, 90, 74, 16, 90,…
#> $ Touches_Touches       <dbl> 14, 22, 34, 13, 42, 13, 48, 18, 82, 47, 10, 69,…
#> $ `Def Pen_Touches`     <dbl> 0, 1, 1, 0, 0, 0, 0, 0, 2, 2, 2, 12, 11, 5, 28,…
#> $ `Def 3rd_Touches`     <dbl> 0, 9, 6, 2, 3, 3, 14, 0, 21, 14, 7, 52, 49, 21,…
#> $ `Mid 3rd_Touches`     <dbl> 9, 7, 17, 5, 27, 7, 29, 17, 50, 27, 4, 21, 36, …
#> $ `Att 3rd_Touches`     <dbl> 5, 6, 12, 7, 14, 4, 8, 2, 16, 7, 0, 0, 1, 19, 0…
#> $ `Att Pen_Touches`     <dbl> 1, 0, 3, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
#> $ Live_Touches          <dbl> 14, 21, 33, 13, 36, 13, 47, 17, 82, 43, 9, 69, …
#> $ Succ_Dribbles         <dbl> 0, 0, 1, 0, 2, 0, 0, 2, 1, 1, 0, 0, 0, 1, 0, 0,…
#> $ Att_Dribbles          <dbl> 0, 1, 3, 1, 4, 0, 2, 3, 1, 2, 0, 0, 0, 2, 0, 0,…
#> $ Succ_percent_Dribbles <dbl> NA, 0.0, 33.3, 0.0, 50.0, NA, 0.0, 66.7, 100.0,…
#> $ Player_NumPl_Dribbles <dbl> 0, 0, 2, 0, 2, 0, 0, 2, 1, 1, 0, 0, 0, 1, 0, 0,…
#> $ Megs_Dribbles         <dbl> 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
#> $ Carries_Carries       <dbl> 10, 9, 24, 10, 28, 6, 36, 19, 62, 35, 6, 51, 55…
#> $ TotDist_Carries       <dbl> 67, 39, 94, 31, 130, 48, 179, 99, 185, 71, 25, …
#> $ PrgDist_Carries       <dbl> 15, 7, 49, 15, 67, 32, 72, 54, 106, 32, 23, 82,…
#> $ Prog_Carries          <dbl> 0, 0, 5, 0, 2, 2, 2, 3, 3, 2, 0, 0, 2, 2, 0, 5,…
#> $ Final_Third_Carries   <dbl> 0, 0, 1, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 2,…
#> $ CPA_Carries           <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
#> $ Mis_Carries           <dbl> 1, 1, 4, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 3,…
#> $ Dis_Carries           <dbl> 0, 0, 0, 0, 2, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0,…
#> $ Targ_Receiving        <dbl> 18, 22, 33, 16, 37, 16, 41, 18, 67, 35, 3, 47, …
#> $ Rec_Receiving         <dbl> 12, 13, 22, 11, 28, 8, 37, 17, 64, 34, 3, 47, 6…
#> $ Rec_percent_Receiving <dbl> 66.7, 59.1, 66.7, 68.8, 75.7, 50.0, 90.2, 94.4,…
```

Or used for the team totals for each match:

``` r
test_urls_multiple <- c("https://fbref.com/en/matches/c0996cac/Bordeaux-Nantes-August-21-2020-Ligue-1",
                        "https://fbref.com/en/matches/9cbccb37/Dijon-Angers-August-22-2020-Ligue-1",
                        "https://fbref.com/en/matches/f96cd5a0/Lorient-Strasbourg-August-23-2020-Ligue-1")

advanced_match_stats_team <- get_advanced_match_stats(match_url = test_urls_multiple, stat_type = "passing_types", team_or_player = "team")
glimpse(advanced_match_stats_team)
#> Rows: 6
#> Columns: 45
#> $ League          <chr> "Ligue 1", "Ligue 1", "Ligue 1", "Ligue 1", "Ligue 1"…
#> $ Gender          <chr> "M", "M", "M", "M", "M", "M"
#> $ Country         <chr> "FRA", "FRA", "FRA", "FRA", "FRA", "FRA"
#> $ Season          <chr> "2020-2021", "2020-2021", "2020-2021", "2020-2021", "…
#> $ Match_Date      <chr> "Friday August 21, 2020", "Friday August 21, 2020", "…
#> $ Matchweek       <chr> "Ligue 1 (Matchweek 1)", "Ligue 1 (Matchweek 1)", "Li…
#> $ Home_Team       <chr> "Bordeaux", "Bordeaux", "Dijon", "Dijon", "Lorient", …
#> $ Home_Formation  <chr> "4-3-3", "4-3-3", "4-3-3", "4-3-3", "4-2-3-1", "4-2-3…
#> $ Home_Score      <dbl> 0, 0, 0, 0, 3, 3
#> $ Home_xG         <dbl> 0.4, 0.4, 0.5, 0.5, 2.8, 2.8
#> $ Home_Goals      <chr> "\n\t\t\n\t\t\tMehdi Zerkane · 20&rsquor; \n\t\t\n\t"…
#> $ Away_Team       <chr> "Nantes", "Nantes", "Angers", "Angers", "Strasbourg",…
#> $ Away_Formation  <chr> "4-4-2", "4-4-2", "4-1-4-1", "4-1-4-1", "4-2-3-1", "4…
#> $ Away_Score      <dbl> 0, 0, 1, 1, 1, 1
#> $ Away_xG         <dbl> 0.3, 0.3, 2.1, 2.1, 0.5, 0.5
#> $ Away_Goals      <chr> "\n\t\t\n\t", "\n\t\t\n\t", "\n\t\t\n\t\t\t Ismaël Tr…
#> $ Game_URL        <chr> "https://fbref.com/en/matches/c0996cac/Bordeaux-Nante…
#> $ Team            <chr> "Bordeaux", "Nantes", "Dijon", "Angers", "Lorient", "…
#> $ Home_Away       <chr> "Home", "Away", "Home", "Away", "Home", "Away"
#> $ Min             <dbl> 919, 990, 990, 990, 990, 990
#> $ Att             <dbl> 525, 662, 578, 495, 451, 466
#> $ Live_Pass       <dbl> 476, 623, 530, 443, 402, 413
#> $ Dead_Pass       <dbl> 49, 39, 48, 52, 49, 53
#> $ FK_Pass         <dbl> 18, 9, 8, 18, 10, 18
#> $ TB_Pass         <dbl> 0, 1, 2, 3, 2, 0
#> $ Press_Pass      <dbl> 81, 40, 124, 28, 54, 72
#> $ Sw_Pass         <dbl> 10, 12, 8, 15, 14, 17
#> $ Crs_Pass        <dbl> 5, 11, 7, 15, 12, 5
#> $ CK_Pass         <dbl> 2, 3, 3, 9, 6, 1
#> $ In_Corner       <dbl> 2, 0, 2, 0, 3, 1
#> $ Out_Corner      <dbl> 0, 3, 1, 3, 3, 0
#> $ Str_Corner      <dbl> 0, 0, 0, 5, 0, 0
#> $ Ground_Height   <dbl> 356, 529, 458, 369, 293, 303
#> $ Low_Height      <dbl> 64, 64, 62, 46, 70, 76
#> $ High_Height     <dbl> 105, 69, 58, 80, 88, 87
#> $ Left_Body       <dbl> 147, 277, 146, 198, 121, 122
#> $ Right_Body      <dbl> 326, 331, 377, 253, 276, 286
#> $ Head_Body       <dbl> 13, 24, 11, 9, 17, 11
#> $ TI_Body         <dbl> 24, 19, 25, 19, 25, 26
#> $ Other_Body      <dbl> 1, 4, 10, 10, 8, 7
#> $ Cmp_Outcomes    <dbl> 431, 577, 499, 416, 363, 368
#> $ Off_Outcomes    <dbl> 0, 3, 1, 0, 1, 2
#> $ Out_Outcomes    <dbl> 10, 7, 7, 9, 10, 14
#> $ Int_Outcomes    <dbl> 1, 2, 6, 3, 6, 8
#> $ Blocks_Outcomes <dbl> 9, 18, 9, 11, 17, 16
```

#### Get match lineups

This function will return a dataframe of all players listed for that
match, including whether they started on the pitch, or on the bench.

``` r
# function to extract match lineups
liv_mci_2020_lineups <- get_match_lineups(match_url = "https://fbref.com/en/matches/47880eb7/Liverpool-Manchester-City-November-10-2019-Premier-League")
#> [1] "Scraping lineups"
glimpse(liv_mci_2020_lineups)
#> Rows: 36
#> Columns: 6
#> $ Matchday    <chr> "Liverpool vs. Manchester City Match Report – Sunday Nove…
#> $ Team        <chr> "Liverpool", "Liverpool", "Liverpool", "Liverpool", "Live…
#> $ Formation   <chr> "4-3-3", "4-3-3", "4-3-3", "4-3-3", "4-3-3", "4-3-3", "4-…
#> $ Player_Num  <chr> "1", "3", "4", "5", "6", "9", "10", "11", "14", "26", "66…
#> $ Player_Name <chr> "Alisson", "Fabinho", "Virgil van Dijk", "Georginio Wijna…
#> $ Starting    <chr> "Pitch", "Pitch", "Pitch", "Pitch", "Pitch", "Pitch", "Pi…
```

-----

### Season-level statistics

#### Get Season Team Stats

The `get_season_team_stats` function allows the user to return a data
frame of different stat types for all teams in tier-1 league seasons.

Note, some stats may not be available for all leagues. The big five
European leagues should have all of these stats.

The following stat types can be selected:

  - *league\_table*
  - *league\_table\_home\_away*
  - *standard*
  - *keeper*
  - *keeper\_adv*
  - *shooting*
  - *passing*
  - *passing\_types*
  - *goal\_shot\_creation*
  - *defense*
  - *possession*
  - *playing\_time*
  - *misc*

<!-- end list -->

``` r
# function to extract season teams stats
prem_2020_shooting <- get_season_team_stats(country = "ENG", gender = "M", season_end_year = "2020", stat_type = "shooting")
#> Scraping season shooting stats
glimpse(prem_2020_shooting)
#> Rows: 40
#> Columns: 25
#> $ Competition_Name         <chr> "Premier League", "Premier League", "Premier…
#> $ Gender                   <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M",…
#> $ Country                  <chr> "ENG", "ENG", "ENG", "ENG", "ENG", "ENG", "E…
#> $ Season_End_Year          <int> 2020, 2020, 2020, 2020, 2020, 2020, 2020, 20…
#> $ Squad                    <chr> "Arsenal", "Aston Villa", "Bournemouth", "Br…
#> $ Team_or_Opponent         <chr> "team", "team", "team", "team", "team", "tea…
#> $ Num_Players              <dbl> 29, 28, 27, 25, 22, 27, 25, 24, 24, 24, 24, …
#> $ Mins_Per_90              <dbl> 38, 38, 38, 38, 38, 38, 38, 38, 38, 38, 38, …
#> $ Gls_Standard             <dbl> 56, 40, 38, 35, 41, 69, 29, 42, 65, 83, 100,…
#> $ Sh_Standard              <dbl> 401, 453, 384, 456, 384, 619, 372, 465, 533,…
#> $ SoT_Standard             <dbl> 144, 146, 116, 137, 124, 210, 116, 155, 181,…
#> $ SoT_percent_Standard     <dbl> 35.9, 32.2, 30.2, 30.0, 32.3, 33.9, 31.2, 33…
#> $ Sh_per_90_Standard       <dbl> 10.55, 11.92, 10.11, 12.00, 10.11, 16.29, 9.…
#> $ SoT_per_90_Standard      <dbl> 3.79, 3.84, 3.05, 3.61, 3.26, 5.53, 3.05, 4.…
#> $ G_per_Sh_Standard        <dbl> 0.13, 0.09, 0.09, 0.07, 0.10, 0.10, 0.07, 0.…
#> $ G_per_SoT_Standard       <dbl> 0.37, 0.27, 0.29, 0.25, 0.31, 0.30, 0.22, 0.…
#> $ Dist_Standard            <dbl> 15.9, 16.9, 16.4, 17.1, 15.7, 16.2, 16.5, 15…
#> $ FK_Standard              <dbl> 19, 17, 21, 11, 17, 27, 15, 20, 20, 18, 27, …
#> $ PK_Standard              <dbl> 3, 1, 4, 1, 3, 7, 3, 1, 5, 5, 6, 10, 0, 2, 1…
#> $ PKatt_Standard           <dbl> 3, 3, 4, 2, 3, 7, 3, 1, 7, 5, 11, 14, 1, 2, …
#> $ xG_Expected              <dbl> 49.2, 40.1, 42.7, 41.2, 43.9, 66.6, 34.0, 49…
#> $ npxG_Expected            <dbl> 46.9, 37.7, 39.7, 39.7, 41.6, 61.7, 31.9, 48…
#> $ npxG_per_Sh_Expected     <dbl> 0.12, 0.08, 0.11, 0.09, 0.11, 0.10, 0.09, 0.…
#> $ G_minus_xG_Expected      <dbl> 6.8, -0.1, -4.7, -6.2, -2.9, 2.4, -5.0, -7.3…
#> $ `np:G_minus_xG_Expected` <dbl> 6.1, 1.3, -5.7, -5.7, -3.6, 0.3, -5.9, -7.5,…
```

**More than one league season**

The `get_season_team_stats` function can be used to get data for
multiple seasons/leages/genders/etc.

Important to note, this function can only be used for one `stat-type` at
a time, however all other parameters can have multiple values:

``` r
big_5_2020_possessions <- get_season_team_stats(country = c("ENG", "ESP", "ITA", "GER", "FRA"),
                                        gender = "M", season_end_year = 2020, stat_type = "possession")
```

-----

## Contributing

### Issues and Feature Requests

Issues, feature requests and improvement ideas are all welcome.

When reporting an issue, please include:

  - Reproducible examples
  - A brief description of what the expected results are
  - If applicable, the fbref.com page the observed behaviour is occuring
    on

For feature requests, raise an issue with the following:

  - The desired functionality
  - Example inputs and desired output

### Pull Requests

Pull requests are also welcomed. Before doing so, please create an issue
or email me with your idea.

Any new functions should follow the conventions established by the the
package’s existing functions. Please ensure:

  - Functions are sensibly named
  - The intent of the contribution is clear
  - At least one example is provided in the documentation
