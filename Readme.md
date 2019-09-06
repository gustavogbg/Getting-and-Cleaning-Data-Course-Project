Readme
================

## Introduction

I wrote this code as a Course Project for Getting and Cleaning Data, by
JHU on Coursera. This script consists on downloading a dataset generated
by the accelerometers installed on Samsung S smartphone, cleaning and
manipulating the data to make it tidy.

### How it works

This script consists on downloading the complete given dataset,
unzipping the downloaded archive to a specific folder and running all
the analysis. As output, it will create a folder named **“UCI HAR
Dataset”** that will hold all the original dataset. Besides, it will
create a folder named **“Analysis”** that will have two TXT files.

`*/Analysis/analysis_DataSet.txt`\* will contain the prepared dataset.  
`*/Analysis/analysis_summaryTable.txt`\* will have all the summaries
calculated, which consists on the average of all the variables, grouped
by subject and activity.

## The Original Questions - By JHU (on Coursera)

All the text below this line was written by the instructors of that
course and the author of the present script is not responsible for that.
It is transcripted here so as to collaborate to the understanding of
this project’s code.

-----

The purpose of this project is to demonstrate your ability to collect,
work with, and clean a data set. The goal is to prepare tidy data that
can be used for later analysis. You will be graded by your peers on a
series of yes/no questions related to the project. You will be required
to submit: 1) a tidy data set as described below, 2) a link to a Github
repository with your script for performing the analysis, and 3) a code
book that describes the variables, the data, and any transformations or
work that you performed to clean up the data called CodeBook.md. You
should also include a README.md in the repo with your scripts. This repo
explains how all of the scripts work and how they are connected.

One of the most exciting areas in all of data science right now is
wearable computing - see for example this article . Companies like
Fitbit, Nike, and Jawbone Up are racing to develop the most advanced
algorithms to attract new users. The data linked to from the course
website represent data collected from the accelerometers from the
Samsung Galaxy S smartphone. A full description is available at the site
where the data was
obtained:

<http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones>

Here are the data for the
project:

<https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip>

You should create one R script called run\_analysis.R that does the
following.

Merges the training and the test sets to create one data set. Extracts
only the measurements on the mean and standard deviation for each
measurement. Uses descriptive activity names to name the activities in
the data set Appropriately labels the data set with descriptive variable
names. From the data set in step 4, creates a second, independent tidy
data set with the average of each variable for each activity and each
subject.

A full description is available at the site where the data was obtained:
<http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones>

Here are the data for the project:
<https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip>

-----