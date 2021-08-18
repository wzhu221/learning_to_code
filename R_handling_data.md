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

# The Use of Generalised Additive Model (GAM) with `R`

## Load packages
```r
library('tidyverse')  # data handling and cleanup
library('readxl')	  # data reading from Excel spreadsheet
library('mgcv')		  # GAM implementation in R
library('tidymv')	  # better visualisation of GAM fittings
```

## Read data and implement GAM
```r
calib <- read_excel('Data/gam.xlsx', sheet='foo')
calib <- data[1:27, 1:2] # pick only the signal intensity 
						 # and the corresponding concentrations from the calibration trials

gam_compound <- gam(concentration~#s(monomer, k=-1) + # uncomment this if monomer also present
                        s(dimer, k=-1), 
                    data=calib)
					
summaryplot <- plot(gam_compound, pages=1, 
                    rug=TRUE, residuals=TRUE, 
                    pch=1, cex=1, shade=TRUE, seWithMean=TRUE,
                    shift=coef(gam_compound)[1])	# get a preview of the fitting plot (base R, looks crappy)
					
summary.gam(gam_compound)	# get a statistical overview of the model, 
							# e.g., the p value of each input variable, 
							# and the degrees of polynomial
```

## Make predictions of experimental data using the GAM we just built
```r
expdata <- cbind(foo[2:6, 26], 
                 foo[2:6, 18]) # acquire the new data, the first column as monomer, the second column as dimer

names(expdata)[1] <- names(calib)[1] # rename the first column to match that of the calibration data
names(expdata)[2] <- names(calib)[2] # rename the second column to match that of the calibration data

expdata$monomer <- as.numeric(newdata$monomer)
expdata$dimer <- as.numeric(newdata$dimer) # make the two columns as numeric value

pred <- as.data.frame(predict.gam(gam_compound, expdata)) # make predictions and output it as dataframe
write.csv(pred, 'foo.csv') # export the results as csv for further use in Excel
```

## Make a nice-looking fitting plot using `ggplot2`
```r


pred <- as.data.frame(predict.gam(gam_compound, se.fit=TRUE))
pred <- cbind(data$dimer, pred)

pred1 <- data
colnames(pred1)[2] <- 'fit'
pred1 <- cbind(pred1, pred$se.fit)
colnames(pred1)[3] <- 'se.fit'

colnames(pred)[1] <- 'dimer'
colnames(pred)[2] <- 'fit'
pred <- cbind(pred, pred1$concentration)

pred %>% ggplot(aes(dimer, fit)) + 
    geom_smooth_ci(color='#A8497A', ci_alpha=0, size=1.2) +
    geom_point(size=2.2, color='darkgrey', shape=15) +
    geom_point(data=pred1, aes(x=dimer, y=concentration), size=2.2, color='black') +
    scale_y_continuous(guide='prism_minor', # to add minor ticks
                       limits=c(0, 2500),
                       breaks=seq(0, 2500, 500),
                       minor_breaks=seq(0, 2500, 100)) +
    # set x axis limits, major and minor tick marks
    scale_x_continuous(guide='prism_minor', # to add minor ticks
                       limits=c(0, 30000),
                       breaks=seq(0, 30000, 5000),
                       minor_breaks=seq(0, 32000, 1000)) +
    theme_linedraw(base_rect_size=1.5,
                   base_line_size=1.5) + xlab('Signal intensity (a.u.)') + ylab('Concentration (ppb)') +
    theme(text=element_text(size=11, family='Helvetica', face='bold'), # set font
          #panel.border=element_rect(size=0.7),
          panel.grid.major.y=element_line(colour='gray', linetype='dotted', size=0.35), # set major gridlines
          panel.grid.major.x=element_line(colour='gray', linetype='dotted', size=0.35), # set major gridlines
          panel.grid.minor=element_blank(), # disable minor gridlines
          panel.background=element_blank(), # disable minor gridlines
          prism.ticks.length.x=unit(1.5, 'pt'), # set x axis minor tick size
          prism.ticks.length.y=unit(1.5, 'pt'), # set y axis minor tick size
          legend.position=c(0.15, 0.83), # move legend box to inside the plot
          legend.background = element_rect(fill='white', color='gray', size=0.5), # set legend box aesthetics
          legend.box.margin=margin(0,0,0,0), # set legend box margins
          axis.title.x=element_text(margin=margin(t=8, unit='pt')), # set offset between x axis and label
          # set offset between y axis and label
          axis.title.y=element_text(margin=margin(r=10, unit='pt')))


```
