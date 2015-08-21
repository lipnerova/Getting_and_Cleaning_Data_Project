# PROJECT INFORMATION

Table of content:
* General info
* Instructions given by lectors
* Walkthrough script with explanations what, how and why were things done
	* Merging
	* Mean and std
	* Activities
	* Labeling
	* Tidy dataset


## General info

This is Course project for Getting and Cleaning Data Course on Coursera.org. The instructions given by lectors are given below. The data were obtained via the course, for detailed info about its origin and origin of variables see CodeBook.md. This file contains mainly the script with thorough comments describing what and why was done in each step. For quick look at script without too many comments look at run_analysis.R file.



## Instructions 

You should create one R script called run_analysis.R that does the following: 

1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement. 
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names.
5. From the data set in step 4, creates a second, independent tidy data set with the average
      of each variable for each activity and each subject. #summarise_each(oldDF(funs(mean))


## Walkthrough script

Each part corresponds to one of the instruction orders, that are given below each section headline. Then the script with comments continues.

### 1. Merging
*1. Merges the training and the test sets to create one data set.*

Set working directory with downloaded data. Data have to be downloaded from attached link and extracted prior to analysis.
(https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip)
	
	setwd("D:/moje/Coursera/Data Cleaning/project")

Load the train sets (find them in the "train" subdirectory) and merge them together. There are three files in train set, all of them with same number of rows, so they can be easily combined together by collumns to create new dataframe called trainset.
	
	subject_train<-read.table("./train/subject_train.txt")
	X_train<-read.table("./train/X_train.txt")
	y_train<-read.table("./train/y_train.txt")

	trainset<-cbind(X_train, subject_train, y_train)

Same process with the test dataset. Load the test sets (find them in the "test" subdirectory) and merge them together. There are three files in test set, all of them with same number of rows, so they can be easily combined together by collumns to create new dataframe called testset.
	
	subject_test<-read.table("./test/subject_test.txt")
	X_test<-read.table("./test/X_test.txt")
	y_test<-read.table("./test/y_test.txt")

	testset<-cbind(X_test, subject_test, y_test)

Merge testset and trainset together to create working dataset called "a". As order of train sets and test sets for combining them was the same (see above), the collumns in both trainset and testset are corresponding, I can use rbind() to just add rows from both datasets together. I choose to name newly created dataset just "a", because it is easy and quick to type.
	
	a<-rbind(trainset, testset)

Name the working dataset collumns so we know what is in each of them. Collumn names for X_test.txt and X_train.txt are in separate file called features.txt, that can be found in main directory of downloaded data. Thanks to stringsAsFactors=FALSE the features are as characters and not as factors.
	
	features<-read.delim("features.txt", header=F, sep=" ", stringsAsFactors=FALSE) 

I have to add also names for subject_test collumn and for activity code collumn. These two collumns are the last two of the dataframe a.

	names(a)<-c(features$V2, "test_subject", "activity_code")


Last step of first part is to get rid of unnecessary files to clear memory and Global environment. This is completely optional, thought I like my global environment uncluttered. Such removing is present at the end of each part.

	rm(subject_test, subject_train, X_test, X_train, y_test, y_train, testset, trainset)




### 2. Mean and std 
*2. Extracts only the measurements on the mean and standard deviation for each measurement.*

I choose only the means and stds of measurements not everything with mean in name. As there were 33 measurements, I end up with 33 collumns for means of them and 33 collumns of std of them. I use fixed=T in grep arguments so only exact match is found, e.g. no "Mean" or "meanFreq()". First I extract collumns for mean (a_mean), then for std (a_std) and then two last collumns to preserve collumns with test subject ids and activity codes(a_rest). Then I bind together all extracted collumns in dataframe called a_extract.
	
	a_mean<-a[, grep(pattern="mean()", x=features$V2, fixed=T)]
	a_std<-a[, grep(pattern="std()", x=features$V2, fixed=T)]
	a_rest<-a[,562:563] #to save columns with activity and test subject id

	a_extract <- cbind(a_rest, a_mean, a_std) #put all the sets together

At the and of the part I again get rid of already unnecessary files. 

	rm(a, features, a_mean, a_std, a_rest)





### 3. Activities
*3. Uses descriptive activity names to name the activities in the data set*

Descriptive activity names are stored in separate file called activity_labels.txt. After loading the file in R as a table, I merged both dataframes using corresponding activity_code collumn in a_extract. Merged dataframe is callad a_label.

	activity_labels<-read.table("activity_labels.txt", header=F) 
	a_label<-merge(a_extract, activity_labels, by.y= "V1", by.x="activity_code")

Merge creates new variable in a_label, called V2, where activity names are filled in based on activity codes of sorresponding rows. To rename this new variable, V2, I find it is easier to use dplyr package then to try rename column without it.

	library(dplyr)
	a_label<-rename(a_label, activity=V2)

Again at the end of the part I get rid of already unnecessary files.

	rm(a_extract, activity_labels)

Also there is no need for "activity_code" collumn in a_label anymore, so get rid of it in this quest for tidy data.

	a_label<-a_label[,-1] 
	




### 4. Labeling
*4. Appropriately labels the data set with descriptive variable names.*

This part calls for renaming the collumns of dataset. My changes are based on [post by ly on course discussion forum](https://class.coursera.org/getdata-031/forum/thread?thread_id=185#comment-780) as I find his solution being quite comprehensive and readable.

First, load the names to separate vector for easier manipulation. There is a typo with two Body in name, so get rid of it at the beginning before any other change.

	l<-names(a_label)
	l<-sub("BodyBody", l, replacement = "Body") #remove typo

Next, change t -> time and f -> freq for all collumn names.

	l<-sub("tBody", l, replacement = "timeBody")
	l<-sub("tGr", l, replacement = "timeGr")
	l<-sub("fBody", l, replacement = "freqBody")

Next, change Acc -> Linear and Gyro -> AngularVelocity (see link above for explanation of Gyro substitution) and Mag -> Magnitude. Also add this underscore character to make variable names more readable.

	l<-sub("Acc", l, replacement = "_Acceleration")
	l<-sub("Gyro", l, replacement = "_AngularVelocity")
	l<-sub("Mag", l, replacement = "_Magnitude")

Next, reformate the std, mean, X, Y, and Z notion to something more readable for me. X, Y, and Z are in position before the mean or std. The mean and std strings without X, Y, or Z are substitued at the end so that their substution does not interfere with substitutions of those strings with X, Y, or Z.

	l<-sub("-std()-X", l, replacement = "_X_std", fixed=T)
	l<-sub("-std()-Y", l, replacement = "_Y_std", fixed=T)
	l<-sub("-std()-Z", l, replacement = "_Z_std", fixed=T)
	l<-sub("-mean()-X", l, replacement = "_X_mean", fixed=T)
	l<-sub("-mean()-Y", l, replacement = "_Y_mean", fixed=T)
	l<-sub("-mean()-Z", l, replacement = "_Z_mean", fixed=T)
	l<-sub("-mean()", l, replacement = "_mean", fixed=T)
	l<-sub("-std()", l, replacement = "_std", fixed=T)

Finally all names in vector "l" are loaded back to names of a_label.

	names(a_label)<-l

Again I get rid of already unnecessary files.

	rm(l)



### 5. Tidy dataset
*5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.* 

This instructions calls for rather complicated grouping. However there is dplyr package and its function summarise_each that deals exactly with this kind of problem. First, I convert the dataframe "a_label" to tbl format used by dplyr functions to table called a_tbl. Then I set the a_tbl to be grouped by activity and then by test subject (activities first, then subjects inside each activity). Then I let the summarise_each count the mean for such grouped data and store the resulted table in a_fin, that is the final tidy dataset.

	a_tbl<-as.tbl(a_label)
	a_fin<-a_tbl %>% group_by(activity, test_subject) %>% summarise_each(funs(mean))

Again I get rid of yet unnecessary files.

	rm(a_label, a_tbl)


Finally I export the tidy dataset from R with argument row.name=F as required by upload instructions.

	write.table(a_fin, "tidy_dataset.txt", row.name=F)
