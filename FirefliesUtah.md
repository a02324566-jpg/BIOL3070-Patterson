Fireflies Utah Firefly Populatons 2014-2024
================
Grant Patterson
2025-10-29

- [Abstract](#abstract)
- [Background](#background)
- [Study questions and Hypothesis](#study-questions-and-hypothesis)
  - [Questions](#questions)
  - [Hypothesis](#hypothesis)
  - [Prediction](#prediction)
- [Methods](#methods)
  - [First Analysis - Boxplot](#first-analysis---boxplot)
  - [Interpretation of Analysis 1](#interpretation-of-analysis-1)
- [Conclusion](#conclusion)
- [References](#references)

# Abstract

# Background

# Study questions and Hypothesis

## Questions

How do geographic location and biome type impact Utah firefly population
size? How does this change North to South?

## Hypothesis

While Southern Utah grows warmer sooner, the higher forestation and more
marshlands in Northern Utah will lead to higher Firefly populations in
Norther Utah than Southern Utah.

## Prediction

There will be higher firefly popuations in Northern than Southern Utah

# Methods

This data on firefly populations was collected as a citizen science
effort, wherein data submissions were open to the general public, as
opposed to just those of a scientific background. This information was
then cleaned up by the collection team and then our team did further
cleaning up of the data (expanding abbreviations, clarifying negative
values that aught to have been positive, and removing irrelevant
information)

## First Analysis - Boxplot

``` r
# Firefly Boxplot

# Load needed packages
library(ggplot2)
library(stringi)

# Read in the data
fireflies <- read.csv("Copy of firefliesUtah - Usable Data.csv", stringsAsFactors = FALSE)

# Rename columns to simpler names
colnames(fireflies) <- c("firefly_count", "region")

# Diagnostics
cat("Original region values:\n")
```

    ## Original region values:

``` r
print(unique(fireflies$region))
```

    ## [1] "north" "south" ""

``` r
cat("\nCounts by region (before cleaning):\n")
```

    ## 
    ## Counts by region (before cleaning):

``` r
print(as.data.frame(table(region = fireflies$region, useNA = "ifany")))
```

    ##   region Freq
    ## 1           2
    ## 2  north  434
    ## 3  south   61

``` r
# Replace blanks with NA
fireflies$region[fireflies$region == ""] <- NA

# Normalize
fireflies$region <- stri_trans_general(fireflies$region, "NFKC")

# Remove invisible characters
fireflies$region <- stri_replace_all_regex(fireflies$region, "\\p{C}", "")

# Convert non-breaking spaces to regular and trim spaces
fireflies$region <- gsub("\u00A0", " ", fireflies$region)
fireflies$region <- trimws(fireflies$region)

# Convert to lowercase
fireflies$region <- tolower(fireflies$region)

# Fix obvious typos or abbreviations
fireflies$region[fireflies$region %in% c("n", "nrth", "noth")] <- "north"
fireflies$region[fireflies$region %in% c("s", "sth", "soth")] <- "south"

# Keep valid categories
fireflies$region <- factor(fireflies$region, levels = c("north", "south"))
fireflies_clean <- droplevels(subset(fireflies, !is.na(region)))

# Check column names
cat("\nUnique cleaned region values:\n")
```

    ## 
    ## Unique cleaned region values:

``` r
print(unique(fireflies_clean$region))
```

    ## [1] north south
    ## Levels: north south

``` r
cat("\nCounts by region (after cleaning):\n")
```

    ## 
    ## Counts by region (after cleaning):

``` r
print(as.data.frame(table(region = fireflies_clean$region)))
```

    ##   region Freq
    ## 1  north  434
    ## 2  south   61

``` r
#Box Plot
ggplot(fireflies_clean, aes(x = region, y = firefly_count, fill = region)) +
geom_boxplot(width = 0.6, color = "black", alpha = 0.7) + # clean, solid boxes
labs(
title = "Firefly Abundance by Region",
x = "Region",
y = "Firefly Count"
) +
scale_fill_manual(values = c("north" = "#8EC9E8", "south" = "#F4A261")) + # subtle professional palette
coord_cartesian(ylim = c(0, 50)) +
theme_minimal(base_size = 13) +
theme(
legend.position = "none",
plot.title = element_text(size = 16, face = "bold", hjust = 0.5),
axis.text = element_text(size = 12),
axis.title = element_text(size = 13),
panel.grid.major.x = element_blank(),
panel.grid.minor = element_blank()
)
```

    ## Warning: Removed 1 row containing non-finite outside the scale range
    ## (`stat_boxplot()`).

![](FirefliesUtah_files/figure-gfm/unnamed-chunk-1-1.png)<!-- --> \#
Discussion

## Interpretation of Analysis 1

*Fill later after I get this code running properly and the box plot
running*

# Conclusion

*Fill after all analyses are up*

# References

*For now, just the link, this will be updated later*
<https://docs.google.com/spreadsheets/d/1p3Gv6F1u4GXp2thnB-FvD5aYHqHRUH9K-lViqrgiSzo/edit?gid=2103391198#gid=2103391198>
