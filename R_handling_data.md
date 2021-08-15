# Use `R` to Handle Data

----
## Read data from a folder
```r
### foo, bar, baz are just placeholders!

library('tidyverse')  # module for  manipulating data
library('readxl')     # module for reading Excel spreadsheet

# Data is read from a file called "foo.xlsx" and the sheet "bar" of that file.

data <- read_excel('foo.xlsx', sheet='bar')
```
-----
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
-----
## Different `ggplot` layers

- Layer names normally start with `geom_`.
- Scatter plot: `geom_points`.
- Bar plot: `geom_bar`.
- Linear regression: `geom-smooth`.
- X, Y and grouping variables are declared in `aes()` function.

### Example
```r
ggplot(data=summary, # indicate the source of data
	   aes(x=Ethanol_content, y=concentration) # indicate the column of x and y variables
	   ) +
    geom_point(colour='black', # draw a scatter plot, make the scatter points black in colour
               size=3 # indicate the size of scatter points
               )+
    geom_smooth(method='lm', # draw a linear regression
			    se=FALSE, # disable showing the confidence interval
			    colour='red', linetype='dashed', size=1 # make the colour red, line as dashed, and line width as 1 pt
			    ) +
    scale_x_continuous(guide='prism_minor',
                       limits=c(0.06, 0.16),
                       breaks=seq(0.06, 0.16, 0.02),
                       minor_breaks=seq(0.06, 0.16, 0.004),
                       labels=scales::percent) +
    scale_y_continuous(guide='prism_minor',
                       limits=c(250, 450),
                       breaks=seq(250, 450, 50),
                       minor_breaks=seq(250, 450, 10)) +
    geom_errorbar(aes(x=Ethanol_content, ymin=concentration-se, ymax=concentration+se), width=0.001, size=0.75) +
    theme_linedraw(base_rect_size=1.5,
                   base_line_size=1.5) + xlab('Ethanol Content') + ylab('Apparent Concentration') +
    theme(text=element_text(size=11, family='Helvetica', face='bold'), # set font
          #panel.border=element_rect(size=0.7),
          panel.grid.major.y=element_line(colour='gray', linetype='dotted', size=0.35), # set major gridlines
          panel.grid.major.x=element_blank(), # set major gridlines
          panel.grid.minor=element_blank(), # disable minor gridlines
          panel.background=element_blank(), # disable minor gridlines
          prism.ticks.length.x=unit(1.5, 'pt'), # set x axis minor tick size
          prism.ticks.length.y=unit(1.5, 'pt'), # set y axis minor tick size
          legend.position=c(0.9, 0.73), # move legend box to inside the plot (use for paper)
          #legend.position=c(0.83, 0.39), # move legend box to inside the plot (use for thesis)
          legend.background=element_rect(fill='white', color='gray', size=0.5), # set legend box aesthetics
          legend.box.margin=margin(0,0,0,0), # set legend box margins
          #axis.title.x=element_text(margin=margin(t=8, unit='pt')), # set offset between x axis and label
          axis.title.y=element_text(margin=margin(r=10, unit='pt')))
#axis.text.x=element_text(angle=45, vjust=1, hjust=1)) # set offset between y axis and label	
```
