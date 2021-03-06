#Codebook
This document describes the code inside `run_analysis.R`.

The code is splitted (by comments) in some sections:

* Helper functions
* Constants
* Downloading and loading data
* Manipulating data
* Writing final data to CSV

## Helper functions

Some functions to avoid code repetition and make the rest of code more clean.

### fileJoin

Used to build file paths, it concatenates strings using a slash as separator.


### downloadToDataDir

Downloads the given url to the given destiny file. It also creates `data` dir if it doesn't exist.

### extractUciHarFile

Extract a file from the zipped UCI HAR file.

### uciHarDataFile

Read dataset files from UCI HAR to given name and prefix. Know names are "train" and "test". Known prefixes are "X", "y" and "subject".

Examples:

* `UCI HAR Dataset/train/X_train.txt`
* `UCI HAR Dataset/train/y_train.txt`
* `UCI HAR Dataset/train/subject_train.txt`

### loadUciHarData

Loads data, labels and subjects from UCI HAR dataset to a `data.frame`.
The returned `data.frame` contains a column `Activity` with labels integer codes, a column `Subject` with subjects integer codes and all other columns from data.

## Constants

Some fixed values like `dataDir`, `outputDir` and `outputFile` used in the other parts of the code.

## Downloading and loading data

* Downloads the UCI HAR zip file if it doesn't exist
* Reads the activity labels to `activityLabels`
* Reads the column names of data (a.k.a. features) to `features`
* Reads the test `data.frame` to `testData`
* Reads the trainning `data.frame` to `trainningData`

## Manipulating data

* Merges test data and trainning data to `allData`
* Indentifies the mean and std columns (plus Activity and Subject) to `meanAndStdCols`
* Extracts a new `data.frame` (called `meanAndStdData`) with only those columns from `meanAndStdCols`.
* Summarizes `meanAndStdData` calculating the average for each column for each activity/subject pair to `meanAndStdAverages`.
* Transforms the column Activity into a factor.
* Uses `activityLabels` to name levels of Activity factor.

At this point the final data frame `meanAndStdAverages` looks like this:

 > head(meanAndStdAverages[, 1:5], n=3)
 Activity Subject tBodyAcc.mean...X tBodyAcc.mean...Y tBodyAcc.mean...Z
 1 WALKING 1 0.2773308 -0.01738382 -0.1111481
 2 WALKING 2 0.2764266 -0.01859492 -0.1055004
 3 WALKING 3 0.2755675 -0.01717678 -0.1126749


## Writing final data to CSV

Creates the output dir if it doesn't exist and writes `meanAndStdAverages` data frame to the ouputfile.
