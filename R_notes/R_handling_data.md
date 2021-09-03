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

## Subset a dataframe (pipeline compatible)

When you have one criterion as 'foo' in 'column1':
```r
new_dataframe <- subset(dataframe, column1=='foo')
```
When you have more then one criteria as 'foo' and 'bar', change the argument `column1=='foo'` to `which(column1=='foo' & column2=='bar')`.

When you have either  criterion 'foo' or criterion 'bar', change the argument `column1=='foo'` to `which(column1=='foo' | column2=='bar')`.







