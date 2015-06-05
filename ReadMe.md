---
title: "ReadMe"
output: html_document
---

## run_analysis.R does the following. 

  1. Merges the training and the test sets to create one data set.
        
  2. Extracts only the measurements on the mean and standard deviation for each measurement.
    
  3. Uses descriptive activity names to name the activities in the data set.
    
  4. Appropriately labels the data set with descriptive variable names. 

  5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject
  
  **Location of the dataset**
  
  The dataset contains the accelerometers readings from the Samsung Galaxy S smartphone. 
  30 users (subjects) performed 6 activities (activity_labels.txt) and measurements were taken ( as explained in features_info.txt)
  
  https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip
  
  
### How to run the script
  
  Once you download the dataset using 
  
```{r}        
        
        if(!file.exists()) { 
                download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip",method="auto", mode="wb")
        }
        
```  
  run_analysis.R should be run from the unzipped dataset directory.
  
  The final dataset is provided in the repository as finaldataset.txt
  
  