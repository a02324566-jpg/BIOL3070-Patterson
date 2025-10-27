# Warm-up mini-Report: Mosquito Blood Hosts in Salt Lake City, Utah

Grant Patterson `r Sys.Date()`

-   [ABSTRACT](#abstract)
-   [BACKGROUND](#background)
-   [Choose some colors](#choose-some-colors)
-   [horizontal barplot](#horizontal-barplot)
-   [add error bars](#add-error-bars)
    -   [Fill in 2nd analysis/plot e.g. generalized linear model](#fill-in-2nd-analysisplot-eg-generalized-linear-model)
-   [DISCUSSION](#discussion)
    -   [Interpretation of 1st analysis (e.g. barplots)](#interpretation-of-1st-analysis-eg-barplots)
    -   [Interpretation of 2nd analysis (e.g. generalized linear model)](#interpretation-of-2nd-analysis-eg-generalized-linear-model)
-   [CONCLUSION](#conclusion)
-   [REFERENCES](#references)

`{r setup, include=FALSE} knitr::opts_chunk$set(echo = TRUE)`

# ABSTRACT {#abstract}



# BACKGROUND {#background}

The West Nile Virus (WNV) is a mosquito-born virus that proliferates in avian hosts and can be transferred to various different host, both human and animal. House finches populations in particular have been shown to have been shown to have a positive correlation with WNV-positive collected blood meals (Komer, 2003). The data used in this report was gathered in the Salt Lake UT area with the intent of analyzing the most common avian hosts of the virus in areas with high percentages of WNV-positive tests on mosquito bloodmeals.

To collect these blood meals, mosquitoes were caught in traps spread around Salt Lake and the surrounding area. Female mosquitoes were then separated from the rest and crushed to release the blood meal. Using Polymerase Chain Reactions (PCR) a specific sequence unique to vertebrates could then be targeted and replicated on mass. This produced many copies of the host’s gene that could then be sequenced to determine its identity. This information was then analyzed to find the hosts that most contributed to the proliferation of WNV.

NOTE: Examples of data you can plot for the background info at <https://github.com/saarman/BIOL3070/>

\`\`\`{r viremia, include=FALSE, eval=TRUE, warning=FALSE, fig.align=‘left’, fig.width=4.75, fig.height=7} 
# Manually transcribe duration (mean, lo, hi) from the last table column 
bird_data <- data.frame (
Bird = c(“Canada Goose”,“Mallard”, “American Kestrel”,“Northern Bobwhite”,
“Japanese Quail”,“Ring-necked Pheasant”, “American Coot”,“Killdeer”,
“Ring-billed Gull”,“Mourning Dove”, “Rock Dove”,“Monk Parakeet”, 
“Budgerigar”,“Great Horned Owl”, “Northern Flicker”,“Blue Jay”, 
“Black-billed Magpie”,“American Crow”, “Fish Crow”,“American Robin”, 
“European Starling”,“Red-winged Blackbird”, “Common Grackle”,“House Finch”,
“House Sparrow”), 
mean = c(4.0,4.0,4.5,4.0,1.3,3.7,4.0,4.5,5.5,3.7,3.2,2.7,1.7,6.0,4.0, 4.0,5.0,
3.8,5.0,4.5,3.2,3.0,3.3,6.0,4.5), 
lo = c(3,4,4,3,0,3,4,4,4,3,3,1,0,6,3, 3,5,3,4,4,3,3,3,5,2), 
hi = c(5,4,5,5,4,4,4,5,7,4,4,4,4,6,5, 5,5,5,7,5,4,3,4,7,6) 
)

# Choose some colors {#choose-some-colors}

cols \<- c(rainbow(30)$$c(10:29,1:5)$$) \# rainbow colors

# horizontal barplot {#horizontal-barplot}

par(mar=c(5,12,2,2)) 
# wider left margin for names bp 
<- barplot(bird_data$mean, horiz=TRUE, names.arg=bird_data$Bird, 
las=1, col=cols, xlab=“Days of detectable viremia”, xlim=c(0,7))

barplot(bird_data\$mean, bird) 
summary(birddata)

# add error bars {#add-error-bars}

arrows(bird_data$lo, bp, bird_data$hi, bp, angle=90, code=3, length=0.05, col=“black”, xpd=TRUE)

```         
# STUDY QUESTION and HYPOTHESIS

## Questions

Which bird acts as an amplifying host of WNV in the Salt Lake City area?

## Hypothesis

There is a strong correlation between house finch populations and WNV proliferation in a given area due to house finches being a good host for the virus.

## Prediction

In the areas with high populations of house finches, a high proportion of mosquito blood meals will carry WNV.

# METHODS



## Fill in 1st analysis e.g. barplots

Fill in here... Explain that you compared the number of mosquito blood meals from each host species between sites with no WNV-positive pools and sites with one or more WNV-positive pools. Describe why a barplot helps visualize this comparison.

```{r first-analysis, warning=FALSE}
## import counts_matrix: data.frame with column 'loc_positives' (0/1) and host columns 'host_*'
counts_matrix <- read.csv("./bloodmeal_plusWNV_for_BIOL3070.csv")

## 1) Identify host columns
host_cols <- grep("^host_", names(counts_matrix), value = TRUE)

if (length(host_cols) == 0) {
  stop("No columns matching '^host_' were found in counts_matrix.")
}

## 2) Ensure loc_positives is present and has both levels 0 and 1 where possible
counts_matrix$loc_positives <- factor(counts_matrix$loc_positives, levels = c(0, 1))

## 3) Aggregate host counts by loc_positives
agg <- stats::aggregate(
  counts_matrix[, host_cols, drop = FALSE],
  by = list(loc_positives = counts_matrix$loc_positives),
  FUN = function(x) sum(as.numeric(x), na.rm = TRUE)
)

## make sure both rows exist; if one is missing, add a zero row
need_levels <- setdiff(levels(counts_matrix$loc_positives), as.character(agg$loc_positives))
if (length(need_levels)) {
  zero_row <- as.list(rep(0, length(host_cols)))
  names(zero_row) <- host_cols
  for (lv in need_levels) {
    agg <- rbind(agg, c(lv, zero_row))
  }
  ## restore proper type
  agg$loc_positives <- factor(agg$loc_positives, levels = c("0","1"))
  ## coerce numeric host cols (they may have become character after rbind)
  for (hc in host_cols) agg[[hc]] <- as.numeric(agg[[hc]])
  agg <- agg[order(agg$loc_positives), , drop = FALSE]
}

## 4) Decide species order (overall abundance, descending)
overall <- colSums(agg[, host_cols, drop = FALSE], na.rm = TRUE)
host_order <- names(sort(overall, decreasing = TRUE))
species_labels <- rev(sub("^host_", "", host_order))  # nicer labels

## 5) Build count vectors for each panel in the SAME order
counts0 <- rev(as.numeric(agg[agg$loc_positives == 0, host_order, drop = TRUE]))
counts1 <- rev(as.numeric(agg[agg$loc_positives == 1, host_order, drop = TRUE]))

## 6) Colors: reuse your existing 'cols' if it exists and is long enough; otherwise generate
if (exists("cols") && length(cols) >= length(host_order)) {
  species_colors <- setNames(cols[seq_along(host_order)], species_labels)
} else {
  species_colors <- setNames(rainbow(length(host_order) + 10)[seq_along(host_order)], species_labels)
}

## 7) Shared x-limit for comparability
xmax <- max(c(counts0, counts1), na.rm = TRUE)
xmax <- if (is.finite(xmax)) xmax else 1
xlim_use <- c(0, xmax * 1.08)

## 8) Plot: two horizontal barplots with identical order and colors
op <- par(mfrow = c(1, 2),
          mar = c(4, 12, 3, 2),  # big left margin for species names
          xaxs = "i")           # a bit tighter axis padding

## Panel A: No WNV detected (loc_positives = 0)
barplot(height = counts0,
        names.arg = species_labels, 
        cex.names = .5,
        cex.axis = .5,
        col = rev(unname(species_colors[species_labels])),
        horiz = TRUE,
        las = 1,
        xlab = "Bloodmeal counts",
        main = "Locations WNV (-)",
        xlim = xlim_use)

## Panel B: WNV detected (loc_positives = 1)
barplot(height = counts1,
        names.arg = species_labels, 
        cex.names = .5,
        cex.axis = .5,
        col = rev(unname(species_colors[species_labels])),
        horiz = TRUE,
        las = 1,
        xlab = "Bloodmeal counts",
        main = "Locations WNV (+)",
        xlim = xlim_use)

par(op)

## Keep the colors mapping for reuse elsewhere
host_species_colors <- species_colors
```

## Fill in 2nd analysis/plot e.g. generalized linear model

Fill in here… Explain that you tested whether the presence or number of house finch blood meals predicts whether a site had WNV-positive pools (binary) or a higher WNV positivity rate (numeric). Mention that this statistical test lets you formally evaluate the relationship suggested by the barplots.

\`\`\`{r} \# second-analysis-or-plot, glm with house finch alone against binary +/\_ glm1 \<- glm(loc_positives \~ host_House_finch, data = counts_matrix, family = binomial) summary(glm1)

#glm with house-finch alone against positivity rate glm2 \<- glm(loc_rate \~ host_House_finch, data = counts_matrix) summary(glm2) \`\`\`

# DISCUSSION {#discussion}



## Interpretation of 1st analysis (e.g. barplots)



## Interpretation of 2nd analysis (e.g. generalized linear model)



# CONCLUSION {#conclusion}



# REFERENCES {#references}

Fill in here… List all sources you cited in your background and throughout the report. Use a consistent style.a conclusion you can draw from your analysis.

1.  Komar N, Langevin S, Hinten S, Nemeth N, Edwards E, Hettler D, Davis B, Bowen R, Bunning M. Experimental infection of North American birds with the New York 1999 strain of West Nile virus. Emerg Infect Dis. 2003 Mar;9(3):311-22. <https://doi.org/10.3201/eid0903.020628>

2.  Google Gemini. Google, version Sep 2025. Used as a reference for functions such as plot() and to correct syntax errors. Accessed `r Sys.Date()`.
