# Metadata for the pond microplastic sampling from LPP and DP 

## File

    pond_microplastic_sp2017.csv
    
## Description

* Collected by: KF 

* Collected on: LPP - 16 March 2017, DP 23 March 2017

* Affiliation: Longwood University

* Location: Longwood University

## Modified

These data were collected as part of the survey of pond microplastic conducted in the Spring of 2017. 

Details about the sampling and sample processing can be found in the [pond_microplastic_lab_notebook](https://github.com/KennyPeanuts/pond_microplastic/tree/master/lab_notebook/lab_notes).


## Variable Descriptions

## Data Entry
### Count Data
    pond <- c(rep("LPP", 9), rep("DP", 6), rep("Blank", 3))
    rep <- c(rep("A", 3), rep("B", 3), rep("C", 3), rep("A", 3), rep("B", 3), rep("A", 3))
    count_rep <- rep(c("i", "ii", "iii"), 6)    
    fiber <- c(108, 127, 143, 111, 121, 89, 94, 100, 96, 63, 49, 61, 14, 26, 32, 3, 2, 3)
    blue.red <- c(2, 4, 0, 3, 2, 3, 0, 0, 1, 2, 1, 0, 4, 1, 3, 1, 0, 2)
    other <- c(1, 1, 3, 1, 0, 1, 1, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0)
    total <- c(111, 132, 146, 115, 123, 93, 95, 100, 97, 65, 51, 62, 18, 27, 35, 4, 2, 5)
    
### Sampling Data
    mesh <- c(rep(153, 15), rep(NA, 3))
    tow_number <- c(rep(10, 9), rep(25, 6), rep(NA, 3))    
    tow_dist <- c(rep(4, 15), rep(NA, 3))
    vol_sampled <- c(rep(1.20, 9), rep(3.00, 6), rep(NA, 3))

## Calculations

### Calculate the number of plastic particles in a sample 
    
Each count is from a 2 ml sub-sample out of a sample that was diluted to 20 ml so dividing by 2 normalizes to a ml and then multiplying by 20 totals for the whole sample.
    
    fiber_samp <- (fiber / 2) * 20
    blue.red_samp <- (blue.red / 2) * 20     
    other_samp <- (other / 2) * 20
    total_samp <- (total / 2) * 20
    
These values represent the estimate of the total number of plastic particles collected in the net.
    
### Determine pond concentration 
    
    fiber_m3 <- fiber_samp / vol_sampled
    blue.red_m3 <- blue.red_samp / vol_sampled
    other_m3 <- other_samp / vol_sampled 
    total_m3 <- total_samp / vol_sampled

    fiber_L <- fiber_m3 / 1000 
    blue.red_L <- blue.red_m3 / 1000
    other_L <- other_m3 / 1000
    total_L <- total_m3 / 1000
    
## Create Data Frame
    
    plastic <- data.frame(pond, rep, mesh, tow_dist, tow_number, vol_sampled, count_rep, fiber_m3, blue.red_m3, other_m3, total_m3, fiber_L, blue.red_L, other_L, total_L)

## Write Table
    
    write.table(plastic, "./data/pond_microplastic_sp2017.csv", row.names = F, quote = F, sep = ",")
