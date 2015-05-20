# Getting and Cleaning Data Course Project

## Summary
The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected. 

One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained:

[link]:http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

Here are the data for the project:

[link]:https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

You should create one R script called run_analysis.R that does the following: merges the training and the test sets to create one data set, extracts only the measurements on the mean and standard deviation for each measurement, uses descriptive activity names to name the activities in the data set, appropriately labels the data set with descriptive variable names, from the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

# Description of run_analysis.R script work

Script that extracts data , cleans, merges it and creates as an output file with tidy data set.

The data should be downloaded from https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

A full description is available at the site: http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

The data will be proceeded in 5 consecutive steps described in the comments above code lines.

Primarily load archive with data and unzip it in your working directory.

Retrieve data from files into data.frames:
```{r}
activity <- read.table("./UCI HAR Dataset/activity_labels.txt", header=FALSE) #Activity Types
features <- read.table("./UCI HAR Dataset/features.txt", header=FALSE) # Features
```
Retrieve training data:
sTrain <- read.table("./UCI HAR Dataset/train/subject_train.txt", header=FALSE) # Train Subjects
xTrain <- read.table("./UCI HAR Dataset/train/X_train.txt", header=FALSE) # Training Set
yTrain <- read.table("./UCI HAR Dataset/train/y_train.txt", header=FALSE) # Training Labels

Retriev test data:
sTest <- read.table("./UCI HAR Dataset/test/subject_test.txt", header=FALSE) # Test Subjects
xTest <- read.table("./UCI HAR Dataset/test/X_test.txt", header=FALSE) # Test Set
yTest <- read.table("./UCI HAR Dataset/test/y_test.txt", header=FALSE) # Test Labels

Assign proper column names to retrieved data:
colnames(activity) <- c("activityID", "activityType")
colnames(sTrain) <- "subjectID"
colnames(sTest) <- "subjectID"
colnames(xTrain) <- features[,2]
colnames(xTest) <- features[,2]
colnames(yTrain) <- "activityID"
colnames(yTest) <- "activityID"

Bind sTrain, yTrain and xTrain to create training data set:
Training <- cbind(yTrain, sTrain, xTrain)

Bind sTest, yTest and xTest to create test data set:
Test <- cbind(yTest, sTest, xTest)

Step 1: Merge the training and the test sets:
Data <- rbind(Training, Test)

Step 2: Extract only the measurements on the mean and standard deviation for each measurement:

Create logical vector with TRUE values for entries containing mean or standard deviation values:
ms_features <- grepl("subjectID|activityID|mean|std", names(Data))

Subset united Data set to keep only necessary columns:
Data <- Data[ms_features == TRUE]

Step 3: Use descriptive activity names to name the activities in the data set:
Data <- merge(activity, Data, by="activityID", all.x=TRUE)

Step 4: Appropriately label the data set with descriptive var names:

Create the data column names vector:
cnames <- colnames(Data)

Adjust initial variable names in the column name vector:
for (i in 1:length(cnames)) {
cnames[i] <- gsub("\\()", "", cnames[i])
cnames[i] <- gsub("-mean", "Mean", cnames[i])
cnames[i] <- gsub("-std", "StdDev", cnames[i])
cnames[i] <- gsub("^(f)", "freq", cnames[i])
cnames[i] <- gsub("^(t)", "time", cnames[i])
}

Reassign more descriptive names to the data set:
colnames(Data) <- cnames

Step 5: create a second tidy data set with the average of each variable for each activity and each subject:

Remove activity type column from the data set:
Data <- Data[,-2]

Count averages and in new tidy data set:
Tidy <- aggregate(Data[,-(1:2)], by=list(activityID=Data$activityID, subjectID=Data$subjectID), mean)

Merge the tidy data set with the activity data set:
Tidy <- merge(activity, Tidy)

Write the final data into file:
write.table(Tidy, file="fin_data.txt", row.names=FALSE, sep="\t")
