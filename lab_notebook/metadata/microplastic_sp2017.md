# Metadata for the pond microplastic sampling from LPP and DP 

## Files

* pond_microplastic_sp2017.csv
  
* pond_microplastic_means_sp2017.csv

## Description

* Collected by: KF 

* Collected on: LPP - 16 March 2017, DP 23 March 2017

* Affiliation: Longwood University

* Location: Longwood University

## Modified

These data were collected as part of the survey of pond microplastic conducted in the Spring of 2017. 

Details about the sampling and sample processing can be found in the [pond_microplastic_lab_notebook](https://github.com/KennyPeanuts/pond_microplastic/tree/master/lab_notebook/lab_notes).


## Variable Descriptions

* sample = the unique identifier for each sample

* pond = the id for each pond. LPP = Lancer Park Pond, DP = Daulton Pond, Blank = the blank that was processed in the lab

* mesh = the size of the mesh in the plankton net (um)

* tow_dist = the distance the plankton net was towed (m)

* tow_number = the number of tows per sample

* vol_sampled = the volume of water that was sampled in each sample (m^3)

* count_rep = the replicate 2 ml subsample taken from the 20 ml dilution for counting.

* fiber = the number of plastic fibers in the 2 ml subsample

* blue.red = the number of blue and red plastic fibers per 2 ml subsample 

* other = the number of microplastic fragments that are not fibers per 2 ml subsample 

* total = the total number of microplastic fragments per 2 ml subsample 

* fiber_m3 = the number of plastic fibers per cubic meter

* blue.red_m3 = the number of blue and red plastic fibers per cubic meter. These are separated out because they may be contamination from the lab

* other_m3 = the number of microplastic fragments that are not fibers per cubic meter

* total_m3 = the total number of microplastic fragments per cubic meter

* fiber_L = the number of plastic fibers per L

* blue.red_L = the number of blue and red plastic fibers per L. These are separated out because they may be contamination from the lab

* other_L = the number of microplastic fragments that are not fibers per L

* total_L = the total number of microplastic fragments per L 

**NOTE: In the pond_microplastic_means_sp2017.csv file, these represent the mean value across the subsamples**

## Data Entry
### Count Data
    sample <- c(rep("LPP-A", 3), rep("LPP-B", 3), rep("LPP-C", 3), rep("DP-A", 3), rep("DP- B", 3), rep("Blank-A", 3))
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
    
    plastic <- data.frame(sample, pond, rep, mesh, tow_dist, tow_number, vol_sampled, count_rep, fiber, blue.red, other, total, fiber_m3, blue.red_m3, other_m3, total_m3, fiber_L, blue.red_L, other_L, total_L)

## Write Table for Raw Data
    
    write.table(plastic, "./data/pond_microplastic_sp2017.csv", row.names = F, quote = F, sep = ",")

## Create Data Frame of Means
### Define Function to average across subsamples for each sample
    
    sub.sample.means <- function(x) {
     conc.means <- numeric(6) # creates empty object for values
     
     for (i in as.numeric(plastic$sample))
      conc.means[i] <- mean(x[as.numeric(plastic$sample) == i], na.rm = T)
      # "x" is the value that needs to be averaged (e.g., plastic$total_L)
     
     return(conc.means)
    }

### Create Data Frame of Means across subsamples

**NOTE THIS REUSES VARIABLE NAMES**
    
    pond <- c("Blank", rep("DP", 2), rep("LPP", 3))
    rep <- c("A", "A", "B", "A", "B", "C")
    fiber_m3.means <- sub.sample.means(plastic$fiber_m3)
    blue.red_m3.means <- sub.sample.means(plastic$blue.red_m3)
    other_m3.means <- sub.sample.means(plastic$other_m3)
    total_m3.means <- sub.sample.means(plastic$total_m3)
    fiber_L.means <- sub.sample.means(plastic$fiber_L)
    blue.red_L.means <- sub.sample.means(plastic$blue.red_L)
    other_L.means <- sub.sample.means(plastic$other_L)
    total_L.means <- sub.sample.means(plastic$total_L)
    
    plastic.means <- data.frame(pond, rep, fiber_m3.means, blue.red_m3.means, other_m3.means, total_m3.means, fiber_L.means, blue.red_L.means, other_L.means, total_L.means)
    names(plastic.means) <- c("pond", "rep", "fiber_m3", "blue.red_m3", "other_m3", "total_m3", "fiber_L", "blue.red_L", "other_L", "total_L")

## Write Table for Data Averaged Across Subsamples
    
    write.table(plastic.means, "./data/pond_microplastic_means_sp2017.csv", row.names = F, quote = F, sep = ",")
