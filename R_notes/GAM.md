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
pred <- as.data.frame(predict.gam(gam_compound, se.fit=TRUE)) # make predictions on the calibration data
colnames(pred)[1] <- 'dimer'
colnames(pred)[2] <- 'fit' # rename both columns accordingly as 'dimer' and 'fit'

ggplot(pred, aes(dimer, fit)) + 
    geom_smooth_ci(color='#A8497A', ci_alpha=0, size=1.2) + # this layer plots the fitting curve
    # geom_point(size=2.2, color='darkgrey', shape=15) + # this layer plots the fitted points
    geom_point(data=calib, aes(x=dimer, y=concentration), size=2.2, color='black') + # this layer plots the original concentration points
    scale_y_continuous(guide='prism_minor', # to add minor ticks
                       limits=c(0, 2500),
                       breaks=seq(0, 2500, 500),
                       minor_breaks=seq(0, 2500, 100)) +
    # set x axis limits, major and minor tick marks
    scale_x_continuous(guide='prism_minor', # to add minor ticks
                       limits=c(0, 30000),
                       breaks=seq(0, 30000, 5000),
                       minor_breaks=seq(0, 32000, 1000)) +
    coord_flip() +
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
