JHU - Getting and Cleaning Data Project - Code Book
================

## CodeBook

This codebook was written as part of a project to complete the “Getting
and Cleaning Data” course taught by JHU on the Coursera learning
platform. Its purpose is to describe the code for data acquisition,
processing, manipulation and output. It was written in R and the data
were obtained from sensors installed on a Samsung Galaxy S smartphone
during laboratory experiments.

### Loaded packages

The only package loaded to RStudio environment was *“tidyverse”*.

``` r
library(tidyverse)
```

### Downloading and Loading the Datasets

The dataset ZIP archive was downloaded to the projetct’s root directory
and it was used the “unzip” to place the original files to a
subdirectory called **“UCI HAR
Dataset”**.

``` r
fileURL = "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"

if(!file.exists ("Dataset.zip")) { 
  download.file(fileURL, destfile = "Dataset.zip", method = "curl")
  unzip("Dataset.zip", exdir = ".")
}

setwd("./UCI HAR Dataset")
```

At this point, *`fileURL`* is used only as a link storage to download
the dataset.

### Activity and Training/Test Labels (or IDs)

The *“./activity\_labels.txt”* file contains the names (or labels) for
the activities that were recorded during the experiment.

It is compared to the *“Training” (“y\_train.txt”)* and the *“Test”
(“y\_test.txt”)* IDs as showed below.

`1 WALKING`  
`2 WALKING_UPSTAIRS`  
`3 WALKING_DOWNSTAIRS`  
`4 SITTING`  
`5 STANDING`  
`6 LAYING`

``` r
## Activity Labels
activityFiles = "activity_labels.txt"

actLabels <- activityFiles %>% 
  read_delim(trim_ws = TRUE, col_names = FALSE, delim = " ") %>% 
  select(X2) 
```

To make the data readable and understandable, one of the asked questions
was to show the activities labels so as it was descriptive ones. To do
so, we used a combination of *“transmute”* and *“case\_when”* functions.

``` r
## Training/Test labels (IDs)
trainingFiles <- c("./test/y_test.txt", 
                   "./train/y_train.txt")

trainingID <- trainingFiles %>% 
  lapply(function(x) { read_tsv(file = x, col_names = "Training")}) %>% 
  reduce(rbind) %>% 
  transmute(Activity = case_when(
    Training == 1 ~ actLabels$X2[1],
    Training == 2 ~ actLabels$X2[2],
    Training == 3 ~ actLabels$X2[3],
    Training == 4 ~ actLabels$X2[4],
    Training == 5 ~ actLabels$X2[5],
    Training == 6 ~ actLabels$X2[6]
    )
  )
```

Both *“y\_test.txt”* and *“y\_train.txt”* files contains the resulting
data registered by the Samsung S sensors, Accelerometer and Gyroscope.
Those data was recorded in both time and frequency domains.

Regarding the variables, *`activityFiles`* and *`trainingFiles`* were
used only as a link storage to the desired data. *`actLabels`* and
*`trainingID`* were both tibbles (like data frames) that contains
activities labels. The first described them, and the last one pointed
which one was being recorded at the moment.

*`actLabels`* results on 6 observations of 1 variable, while
*`trainingID`* has 10.299 obs. of 1 variable.

### Features

*`featuresFile`* collects the *“features.txt”* file, which contains the
names (or labels) for the activities that were recorded during the
experiment.

*`featuresLabels`* loads all the data that was inside the TXT file and
selects only the column named X2. The guide for this project asked to
select only the variables of mean and stardard deviation, so we used the
grep function to get those variables index and stored it at
*`featuresIndex`*.

``` r
## Features (these will be the columns names for the results obtained)
featuresFile = "features.txt"

featuresLabels <- featuresFile %>% 
  read_delim(trim_ws = TRUE, col_names = FALSE, delim = " ") %>% 
  select(X2) 

featuresIndex <- grep("mean\\(\\)|std\\(\\)", featuresLabels$X2)

featuresLabels <- featuresLabels[featuresIndex,]
```

*`featuresLabels`* results on 561 observations of 1 variable before
`grep` function, and has 66 obs. of 1 variable after that.

### Training/Test sets

At this point in the script, we have the collection of Samsung S sensor
data that was recorded in the datasets contained in the *`x_test.txt`*
and *`x_train.txt`* files. After this collection, in order to use only
data related to the mean and standard deviation, the columns are
selected from the application of the indexes collected from the FEATURES
phase. Finally, the names are modified to be readable and
understandable.

``` r
## Training/Test sets
activitySetFiles <- c("./test/X_test.txt", 
                      "./train/X_train.txt")

activitySets <- activitySetFiles %>% 
  lapply(function(x) { read_delim(file = x, col_names = as.character(unlist(featuresLabels)), 
                                  delim = " ", trim_ws = TRUE)}) %>% 
  reduce(rbind)

activitySets <- activitySets[,featuresIndex]

names(activitySets) <- gsub("^t", "time", names(activitySets))
names(activitySets) <- gsub("^f", "frequency", names(activitySets))
names(activitySets) <- gsub("Acc", "Accelerometer", names(activitySets))
names(activitySets) <- gsub("Gyro", "Gyroscope", names(activitySets))
names(activitySets) <- gsub("Mag", "Magnitude", names(activitySets))
names(activitySets) <- gsub("BodyBody", "Body", names(activitySets))
```

*`activitySets`* dimensions are: 10299 observations of 561 variables
before and 10299 obs. of 66 variables after selecting the desired
columns.

## Joining All Data Together

For joining all the cleaned and prepared data, it was used the *`cbind`*
function.

``` r
## Joining all data sets
dfList <- list(subjectID, trainingID, activitySets)
analysisDataSet <- Reduce(function(x,y){ cbind(x,y) }, dfList)
```

## Summarizing

So as that we could have a summary grouped by subject and activity, it
was used both *`group_by`* and *`summarise_at`* functions. This last one
consists on a summarise function that can handle more than one variable
to calculate what is desired.

``` r
##  Grouping Variables and creating a summary dataset
analysisSummary <- analysisDataSet %>% group_by(subject_ID, Activity) 
analysisSummary <- analysisSummary %>% summarise_at(vars(
    "timeBodyAccelerometer-mean()-X":"frequencyBodyGyroscopeJerkMagnitude-std()"), 
                                                    mean, na.rm = TRUE)
```

## Datasets Output

We used the *`write.table`* function to write two TXT files with all the
prepared datasets.

``` r
##  Saving external tables
if(!file.exists ("../Analysis")) { dir.create ("../Analysis") }
write.table(analysisSummary, file = "../Analysis/analysis_summaryTable.txt", row.names = FALSE)
write.table(analysisDataSet, file = "../Analysis/analysis_DataSet.txt", row.names = FALSE)
```
