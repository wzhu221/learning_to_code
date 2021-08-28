# Use `R` to Handle Data

## Read data from a folder

```r
### foo, bar, baz are just placeholders!

library('tidyverse')  # module for  manipulating data
library('readxl')     # module for reading Excel spreadsheet

# Data is read from a file called "foo.xlsx" and the sheet "bar" of that file.

data <- read_excel('foo.xlsx', sheet='bar')
```

## Obtain summary of data

```r
# We create a summary of raw data. 

# "%>%" is used as a pipeline function that transfers the result of the previous step as the input for the following step.

# Standard error can also be calculated by dividing the standard deviation by the square root of the number of replicates.

summary <- as.data.frame( # transform the result as dataframe
	    data %>%
        group_by(baz, bar) %>% # sort the data according to sorting variables, can be more than one sorting variable
        summarize(sd=sd(foo), concentration=mean(foo)) %>%
        distinct())
```

## Subset a dataframe

When you have one criterion as 'foo':
```r
new_dataframe <- dataframe[dataframe$column1=='foo', ]
```
When you have more then one criteria as 'foo' and 'bar':
```r
new_dataframe <- dataframe[which(dataframe$column1=='foo' & dataframe$column2=='bar'), ]
```
When you have criterion 'foo' or criterion 'bar', use vertical separator | to separate the two (or more) criteria:
```r
new_dataframe <- dataframe[which(dataframe$column1=='foo' | dataframe$column2=='bar'), ]
```

## Run ANOVA and _post-hoc_ pair-wise tests (Tukey's HSD)

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






