# Khalil-Ali
R version 4.0.0 (2020-04-24) -- "Arbor Day"
Copyright (C) 2020 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

[Workspace loaded from ~/.RData]

Welcome to your Lab Sandbox! To get started, please reference the README.rd file on the right side of your screen.
> X_train <- read.table("~/UCI HAR Dataset/train/X_train.txt", quote="\"", comment.char="")
>   View(X_train)
> View(features)
> View(measurements)
> View(subject_test)
> View(subject_train)
> View(activity_labels)
> View(measurements)
> # Merge the training and test sets
> X_merged <- rbind(X_train, X_test)
> 
> y_merged <- rbind(y_train, y_test)
> subject_merged <- rbind(subject_train, subject_test)
> complete_data <- cbind(subject_merged, y_merged, X_merged)
> 
> colnames(subject_merged) <- "subject"
> colnames(y_merged) <- "activity"
> 
> View(features)
> # Read features.txt
> features <- read.table("features.txt", col.names = c("index", "feature_name"))
Error in file(file, "rt") : cannot open the connection
In addition: Warning message:
In file(file, "rt") :
  cannot open file 'features.txt': No such file or directory
> 
> View(X_test)
> load("~/UCI HAR Dataset/features.txt")
Error in load("~/UCI HAR Dataset/features.txt") : 
  bad restore file magic number (file may be corrupted) -- no data loaded
In addition: Warning message:
file ‘features.txt’ has magic number '1 tBo'
  Use of save versions prior to 2 is deprecated 
> # Read features.txt
> features <- read.table("features.txt", col.names = c("index", "feature_name"))
Error in file(file, "rt") : cannot open the connection
In addition: Warning message:
In file(file, "rt") :
  cannot open file 'features.txt': No such file or directory
> 
> load("~/UCI HAR Dataset/features_info.txt")
Error in load("~/UCI HAR Dataset/features_info.txt") : 
  bad restore file magic number (file may be corrupted) -- no data loaded
In addition: Warning message:
file ‘features_info.txt’ has magic number 'Featu'
  Use of save versions prior to 2 is deprecated 
> features <- read.table("~/UCI HAR Dataset/features.txt", quote="\"", comment.char="")
>   View(features)
> # Read features.txt
> features <- read.table("features.txt", col.names = c("index", "feature_name"))
Error in file(file, "rt") : cannot open the connection
In addition: Warning message:
In file(file, "rt") :
  cannot open file 'features.txt': No such file or directory
> 
> View(features)
> # Use grep to get indices of features with mean() or std()
> desired_features <- grep("-(mean|std)\\(\\)", features$feature_name)
> 
> # Extract the names of the selected features
> selected_feature_names <- features$feature_name[desired_features]
> 
> # Clean up feature names (remove parentheses, dashes, etc.)
> selected_feature_names <- gsub("\\(\\)", "", selected_feature_names)
> selected_feature_names <- gsub("-", "_", selected_feature_names)
> 
> # Since X_merged had all features, use those indices to subset the columns
> # But in complete_data, the measurements start from column 3
> # So we need to shift the indices by 2 to account for subject and activity
> selected_columns <- desired_features + 2
> 
> # Now subset the data
> tidy_data <- complete_data[, c(1, 2, selected_columns)]
> 
> # Set proper column names
> colnames(tidy_data) <- c("subject", "activity", selected_feature_names)
> 
> # Read activity labels file
> activity_labels <- read.table("activity_labels.txt", col.names = c("activity_id", "activity_name"))
Error in file(file, "rt") : cannot open the connection
In addition: Warning message:
In file(file, "rt") :
  cannot open file 'activity_labels.txt': No such file or directory
> 
> View(activity_labels)
> # Read activity labels file
> activity_labels <- read.table("activity_labels.txt", col.names = c("activity_id", "activity_name"))
Error in file(file, "rt") : cannot open the connection
In addition: Warning message:
In file(file, "rt") :
  cannot open file 'activity_labels.txt': No such file or directory
> 
> View(activity_labels)
> library(readr)
> activity_labels <- read_csv("UCI HAR Dataset/activity_labels.txt")
Parsed with column specification:
cols(
  `1 WALKING` = col_character()
)
> View(activity_labels)
> # Convert activity column in tidy_data to a factor using activity_labels
> tidy_data$activity <- factor(tidy_data$activity,
+                              levels = activity_labels$activity_id,
+                              labels = activity_labels$activity_name)
Warning messages:
1: Unknown or uninitialised column: `activity_id`. 
2: Unknown or uninitialised column: `activity_name`. 
> 
> # These were already cleaned earlier in Step 4
> # selected_feature_names <- gsub(...) etc.
> # They are already assigned to tidy_data like this:
> # colnames(tidy_data) <- c("subject", "activity", selected_feature_names)
> 
> names(tidy_data) <- gsub("^t", "Time", names(tidy_data))
> names(tidy_data) <- gsub("^f", "Frequency", names(tidy_data))
> names(tidy_data) <- gsub("Acc", "Accelerometer", names(tidy_data))
> names(tidy_data) <- gsub("Gyro", "Gyroscope", names(tidy_data))
> names(tidy_data) <- gsub("Mag", "Magnitude", names(tidy_data))
> names(tidy_data) <- gsub("BodyBody", "Body", names(tidy_data))
> names(tidy_data) <- gsub("mean", "Mean", names(tidy_data))
> names(tidy_data) <- gsub("std", "STD", names(tidy_data))
> names(tidy_data) <- gsub("[\\(\\)-]", "", names(tidy_data))  # Remove parentheses and dashes
> 
> library(dplyr)

Attaching package: ‘dplyr’

The following objects are masked from ‘package:stats’:

    filter, lag

The following objects are masked from ‘package:base’:

    intersect, setdiff, setequal, union

> 
> final_tidy_data <- tidy_data %>%
+     group_by(subject, activity) %>%
+     summarise_all(mean)
> write.table(final_tidy_data, "tidy_data.txt", row.name = FALSE)
