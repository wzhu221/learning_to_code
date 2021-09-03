# Run ANOVA and _post-hoc_ pair-wise tests (Tukey's HSD)

The ANOVA can be easily run through a `R` base function: `aov`. 

Suppose that you have a dataframe `df` that is organised into the long format with the independent variable called `factor` and the dependent variable called `result`, then simply run:

```r
df$factor <- as.factor(df$factor) # run this if the factor variable is in the formate of numbers
aov(result ~ factor, data=df)
```

Note that you don't have to make a summary of the data before you run ANOVA. `aov` requires the input of the replicates _as is_.

To obtain pair-wise _post-hoc_ results, you can use:

```r
aov(result ~ factor, data=df) %>% TukeyHSD()
```

Alternatively, if you would like to have the result similarities indicated in a, b, c labels, you can use:
```r
library('emmeans')
library('multcomp')
# direct print in the console
cld(glht(aov(result ~ factor, data=df), linfct=mcp(factor='Tukey')))
# or output to csv
write.csv(as.data.frame(cld(glht(aov(result ~ factor, data=df), linfct=mcp(factor='Tukey')))), 'anova_tukey.csv')
```
