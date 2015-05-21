# CodeBook

This is the Code Book that describes data, variables and procedures.

## Data Source
Description: http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 
Archive: https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

## Information about the data set
The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain.

For each record it is provided:
- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 
- An identifier of the subject who carried out the experiment.

## Data set content description
- 'README.txt': File with data description
- 'features_info.txt': Shows information about the variables used on the feature vector.
- 'features.txt': List of all features.
- 'activity_labels.txt': Links the class labels with their activity name.
- 'train/X_train.txt': Training set.
- 'train/y_train.txt': Training labels.
- 'test/X_test.txt': Test set.
- 'test/y_test.txt': Test labels.
The following files are available for the train and test data. Their descriptions are equivalent. 
- 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 
- 'train/Inertial Signals/total_acc_x_train.txt': The acceleration signal from the smartphone accelerometer X axis in standard gravity units 'g'. Every row shows a 128 element vector. The same description applies for the 'total_acc_x_train.txt' and 'total_acc_z_train.txt' files for the Y and Z axis. 
- 'train/Inertial Signals/body_acc_x_train.txt': The body acceleration signal obtained by subtracting the gravity from the total acceleration. 
- 'train/Inertial Signals/body_gyro_x_train.txt': The angular velocity vector measured by the gyroscope for each window sample. The units are radians/second. 

## run_analysis.R script work description

This script works in five main steps: merges the training and the test sets to create one data set, extracts only the measurements on the mean and standard deviation for each measurement, uses descriptive activity names to name the activities in the data set, appropriately labels the data set with descriptive variable names, from the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

First of all we retrieve data from text files and save it into proper data frames. I have used 8 data frames: one containing activity IDs and names (from activity_labels.txt), another with features list (features.txt) and three equivalent for training and test data sets (test and training sets from X_test.txt and X_train.txt, test and training labels from y_test.txt and y_train.txt, test and training subjects from subject_test.txt and subject_train.txt).
Second I have assigned proper column titles for each data set except features (which became a list of column names for Train and Test sets data frames).
I have bound columns of training labels, subjects and set create training data set and then did the same thing with test data frames.
Now I was ready to accomplish the first step and merge the training and the test sets using row bind function.
To accomplish second step I have created logical vector with TRUE values for entries containing mean or standard deviation values and used it to get a subset of the data with necessary columns.
To accomplish third step I have merged the activity data set with the proceeding data set by activityID column.
To accomplish fourth step I have retrieved the data column titles into vector and adjusted them inside of this vector using for loop and gsub function with regular expressions. Then I have assigned derived vector to be the data set column names.
To accomplish fifth step I have removed activity type column from the proceeding data set, then counted proper grouped averages and merged obtained data frame with the activity data set.
Finally, I have written the obtained tidy data set into test file named "fin_data.txt".

## Tidy data file
The final data set consists of 180 rows. Each row corresponds to a particular subject (there are totally 30 of them) combined with one of the six experiment activities he made.
First three columns of the final data set contain respectively activity ID, activity name and subject ID values. All the other columns contain averaged measures for mean and standard deviation of each experimental feature.
