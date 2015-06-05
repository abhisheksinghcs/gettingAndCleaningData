---
title: "CodeBook.md"
output: html_document
---

###Purpose
This code book accompanies "run_analysis.R" and describes the variables, the data, and any transformations or work that was performed to clean up the data.

###Description of R variables used in the script


**Variables related to dataset**

* testdata ## Dataframe containing the test data
* testdata.raw (test data as backup)
* trainingdata ## Dataframe containing the test data
* train.raw (training data as backup)
* mergeddataset ## Dataframe containing merged dataset
* meanandstddataset ## Dataframe containing only Mean and Standard Deviations
* mmeanandstddataset ## Dataframe that is melted on User and Activity to compute means for measurements
* finaldataset ## Final Dataset with the means of each feature


**Variables names related to features**

* features ## DF with 561 features
* tidycolnames ## Character vector of tidy features
* duplicatefeatures ## Vector of duplicate col names
* duplicatepatterns ## Vector of patterns that are duplicated in colnames
* tidyfeaturelist ## Character vector of tidy features with modified duplicate feature names
* patternlist        	## List of patterns and their descriptive replacements for * descriptive column names
* descriptivefeaturenames	## Descriptive column names for the dataset with mean and standard deviation



**Variables names related to activities**

* activitylabels  ## Data frame of the activities and their codes
* testdatalabels ## Data frame of the activity codes for each observation of the test dataset
* trainingdatalabels ## Data frame of the activity codes for each observation of the training dataset
* tidytestdatalabels ## Character vector tidy version of the activities of test dataset; colname(tidytestdatalabels)<- "activitylabels"

**Variables names related to subjects**

* testsubject ## list of subjects for each observation of test dataset;  colname(testsubject)<-"subject"
* trainingsubject ## list of subjects for each observation of training dataset;  colname(testsubject)<-"subject"


**Functions defined**

* tidyfeatures(pattern, string) ## Tidy the feature names from the features.txt and returns a character vector
* findduplicateindexes(pattern,names) ## This function finds the index of duplicate cols the function returns a list of indexes for each duplicate column name
* renameduplicatefeatures(l = list(),features) ## This function renames the duplicate feature names and returns the tidy feature character vector 
* describecolumnnames(patternlist,mergeddatasetcolumnnames) ## This function transforms the column names to be more descriptive based on the descriptions in features_info.txt


###Steps performed during transformation

1. Download the dataset
2. Unzip the dataset
3. Set working directory as the unzipped dataset directory `r setwd()`
4. Read the information related to test dataset:
        + Read the test dataset `r read.table("./test/X_test.txt", colClasses = "numeric", stringsAsFactors = FALSE)`
        + Read the test labels (list of activity code for the test observation)
`r testdatalabels<-read.table("./test/y_test.txt", colClasses = "numeric", stringsAsFactors = FALSE)`
        + Read test subjects `r trainingsubject<-read.table("./test/subject_train.txt", colClasses = "numeric", stringsAsFactors = FALSE)`
        + Read activities
`r activitylabels<-read.table("./test/y_test.txt", colClasses = "numeric", stringsAsFactors = FALSE)`
5. Tidy the features
        + Identify duplicate column names
        + Modify the duplicate column names by appending *dup* at end
        
6. Prepare the test dataset
        + Convert Test Data Labels (y_test.txt) to actual descriptions
        + Combining the activity and subject with the test dataset
7. Perform step 5 and 6 for training dataset as well
8. Merges the training and the test sets to create one data set
        `r mergeddataset<-rbind(trainingdata,testdata)`
9. Extract only the measurements on the mean and standard deviation for each measurement
`r meanandstddataset<-select(mergeddataset,matches("(mean|std|user|activity)"))`
10. Appropriately labels the data set with descriptive variable names. 
```{r}
mergeddatasetcolumnnames<-colnames(mergeddataset)
patternlist<-list(pattern=c("^t","^f","acc","jerk","mag","gyro","body","user","activity"),transform = c("timed","fourier","acceleration","jerksignal","magnitude","angularvelocity","linear","user","activity"))
descriptivefeaturenames<-describecolumnnames(patternlist,names(meanandstddataset))
colnames(meanandstddataset)<-descriptivefeaturenames
```
11. From the data set in step 10, creates a second, independent tidy data set with the average of each variable for each activity and each subject
        + melt the dataframe on user and activity
        + dcast the melted dataframe on means of all readings
12. Save the final dataset as finaldataset.txt

###Features (variables) in the final dataset

**_The final dataset contains the average values of the features described below for user and activity_**

The features selected for this database come from the accelerationelerometer and angularvelocityscope 3-axial raw signals timedaccelerationelerationxyz and timedangularvelocityxyz. These time domain signals were captured at a constant rate of 50 Hz. Then they were filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove noise. Similarly, the acceleration signal was then separated into linear and gravity acceleration signals (timedlinearaccelerationxyz and timedgravityaccelerationxyz) using another low pass Butterworth filter with a corner frequency of 0.3 Hz. 

Subsequently, the linear linear accelerationeleration and angular velocity were derived in time to obtain Jerk signals (timedlinearaccelerationjerksignalxyz and timedlinearangularvelocityjerksignalxyz). Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm (timedlinearaccelerationmagnitude, timedgravityaccelerationmagnitude, timedlinearaccelerationjerksignalmagnitude, timedlinearangularvelocitymagnitude, timedlinearangularvelocityjerksignalmagnitude). 

Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing fouriertransformedlinearaccelerationxyz, fouriertransformedlinearaccelerationjerksignalxyz, fouriertransformedlinearangularvelocityxyz, fouriertransformedlinearaccelerationjerksignalmagnitude, fouriertransformedlinearangularvelocitymagnitude, fouriertransformedlinearangularvelocityjerksignalmagnitude.  

These signals were used to estimate variables of the feature vector for each pattern:  
'xyz' is used to denote 3-axial signals in the X, Y and Z directions e.g. **timedlinearangularvelocitymeanx**.

* timedimedlinearaccelerationxyz
* timedgravityaccelerationxyz
* timedlinearaccelerationjerksignalxyz
* timedlinearangularvelocityxyz
* timedlinearangularvelocityjerksignalxyz
* timedlinearaccelerationmagnitude
* timedgravityaccelerationmagnitude
* timedlinearaccelerationjerksignalmagnitude
* timedlinearangularvelocitymagnitude
* timedlinearangularvelocityjerksignalmagnitude
* fouriertransformedlinearaccelerationxyz
* fouriertransformedlinearaccelerationjerksignalxyz
* fouriertransformedlinearangularvelocityxyz
* fouriertransformedlinearaccelerationmagnitude
* fouriertransformedlinearaccelerationjerksignalmagnitude
* fouriertransformedlinearangularvelocitymagnitude
* fouriertransformedlinearangularvelocityjerksignalmagnitude

The set of variables that were estimated from these signals are: 

* mean(): mean value
* std(): Standard deviation

Additional vectors obtained by averaging the signals in a signal window sample. These are used on the angle() variable e.g. **anglexgravitymean**:

* gravitymean
* timedlinearaccelerationmean
* timedlinearaccelerationjerksignalmean
* timedlinearangularvelocitymean
* timedlinearangularvelocityjerksignalmean
