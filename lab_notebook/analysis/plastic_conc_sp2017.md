# Analysis of the concentration of microplastic in the ponds
## Metadata

### File created 

* 26 May 2017

### Modified

## Description

These analyses are to evaluate the concentration of microplastic in LPP and DP 

The description of the experiment can be found in [the lab notes](https://github.com/KennyPeanuts/pond_microplastic/tree/master/lab_notebook/lab_notesd)

## Import Data

    plastic.raw <- read.table("./data/pond_microplastic_sp2017.csv", header = T, sep = ",")
    plastic <- read.table("./data/pond_microplastic_means_sp2017.csv", header = T, sep = ",")

## Data Summaries

### Count Summaries

    tapply(plastic.raw$total, plastic.raw$pond, summary) 

## Analysis

### Create vector of pond factors (i.e., remove data from Blank)


    

