Warm-up-mosquitoes-PATTERSON
================
Grant Patterson
2025-10-12

- [ABSTRACT](#abstract)
- [BACKGROUND](#background)
- [STUDY QUESTION and HYPOTHESIS](#study-question-and-hypothesis)
  - [Questions](#questions)
  - [Hypothesis](#hypothesis)
  - [Prediction](#prediction)
- [METHODS](#methods)
  - [Fill in first analysis](#fill-in-first-analysis)
  - [Fill in second analysis/plot](#fill-in-second-analysisplot)
- [DISCUSSION](#discussion)
  - [Interpretation - fill in
    analysis](#interpretation---fill-in-analysis)
  - [Interpretation - fill in
    analysis/plot](#interpretation---fill-in-analysisplot)
- [CONCLUSION](#conclusion)
- [REFERENCES](#references)

# ABSTRACT

Fill in abstract at the end after we have finished the methods, results,
discussion, conclusions and know what our data “says”.

# BACKGROUND

The West Nile Virus (WNV) is a mosquito-born virus that proliferates in
avian hosts and can be transferred to various different host, both human
and animal. House finches populations in particular have been shown to
have been shown to have a positive correlation with WNV-positive
collected blood meals (Komer, 2003). The data used in this report was
gathered in the Salt Lake UT area with the intent of analyzing the most
common avian hosts of the virus in areas with high percentages of
WNV-positive tests on mosquito blood meals.

To collect these blood meals, mosquitoes were caught in traps spread
around Salt Lake and the surrounding area. Female mosquitoes were then
separated from the rest and crushed to release the blood meal. Using
Polymerase Chain Reactions (PCR) a specific sequence unique to
vertebrates could then be targeted and replicated on mass. This produced
many copies of the host’s gene that could then be sequenced to determine
its identity. This information was then analyzed to find the hosts that
most contributed to the proliferation of WNV.

``` r
# Manually transcribe duration (mean, lo, hi) from the last table column
bird_data <- data.frame(
  Bird = c("Canada Goose","Mallard", 
           "American Kestrel","Northern Bobwhite",
           "Japanese Quail","Ring-necked Pheasant",
           "American Coot","Killdeer",
           "Ring-billed Gull","Mourning Dove",
           "Rock Dove","Monk Parakeet",
           "Budgerigar","Great Horned Owl",
           "Northern Flicker","Blue Jay",
           "Black-billed Magpie","American Crow",
           "Fish Crow","American Robin",
           "European Starling","Red-winged Blackbird",
           "Common Grackle","House Finch","House Sparrow"),
  mean = c(4.0,4.0,4.5,4.0,1.3,3.7,4.0,4.5,5.5,3.7,3.2,2.7,1.7,6.0,4.0,
           4.0,5.0,3.8,5.0,4.5,3.2,3.0,3.3,6.0,4.5),
  lo   = c(3,4,4,3,0,3,4,4,4,3,3,1,0,6,3,
           3,5,3,4,4,3,3,3,5,2),
  hi   = c(5,4,5,5,4,4,4,5,7,4,4,4,4,6,5,
           5,5,5,7,5,4,3,4,7,6)
)

# selecting colors
cols <- c(rainbow(30)[c(10:29,1:5)])  # rainbow colors

# horizontal barplot
par(mar=c(5,12,2,2))  # wider left margin for names
bp <- barplot(bird_data$mean, horiz=TRUE, names.arg=bird_data$Bird,
              las=1, col=cols, xlab="Days of detectable viremia", xlim=c(0,7))

# add error bars to horizontal bar plot
arrows(bird_data$lo, bp, bird_data$hi, bp,
       angle=90, code=3, length=0.05, col="black", xpd=TRUE)
```

<img src="knit_files/figure-gfm/viremia-1.png" style="display: block; margin: auto auto auto 0;" />

# STUDY QUESTION and HYPOTHESIS

## Questions

Which bird species acts as an amplifying host of WNV in the Salt Lake
City area?

## Hypothesis

There is a strong correlation between house finch populations and WNV
proliferation in a given area due to house finches being a good host for
the virus.

## Prediction

If house finches are acting as important amplifying hosts, we predict
that trapping locations where mosquitoes feed on house finches will also
have higher rates of confirmed WNV in tested mosquito pools.

# METHODS

SLMAD
Capture Methods
BLAST
Graph Types

Fill in here, including overview of procedure and methods used for this
project.

## Fill in first analysis

``` {r first-analysis, include=TRUE, eval=TRUE, warning=FALSE, fig.align='left', fig.width=4.75, fig.height=7}
### insert code for analysis part here - cut do to not being able to get files to function properly
blood <-"https://raw.githubusercontent.com/saarman/BIOL3070/main/bloodmeal_for_BIOL3070.csv" #inserting url for data
bloodmeal <- read.csv(blood) #reading in the file for future reference
host_cols <- grep("^host_", names(counts_matrix), value = TRUE)

if (length(host_cols) == 0) {
  stop("No columns matching '^host_' were found in counts_matrix.")
```

## Fill in second analysis/plot

``` r
# put code for plotting here
```

# DISCUSSION

## Interpretation - fill in analysis

## Interpretation - fill in analysis/plot

# CONCLUSION

The data indicates that there is a strong correlation between Housefinch
bloodmeals and West Nile Virus spread in the Salt Lake Area

# REFERENCES

1.  Komar N, Langevin S, Hinten S, Nemeth N, Edwards E, Hettler D, Davis
    B, Bowen R, Bunning M. Experimental infection of North American
    birds with the New York 1999 strain of West Nile virus. Emerg Infect
    Dis. 2003 Mar;9(3):311-22. <https://doi.org/10.3201/eid0903.020628>

2.  Google Gemini. OpenAI, version Sep 2025. Used as a reference for
    functions such as plot() and to correct syntax errors. Accessed
    2025-10-12.
