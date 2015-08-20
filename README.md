# PROJECT INFORMATION

This is Course prokect for Getting and Cleaning Data Course on Coursera.org. The instructions given by lectors are given below. The data were obtained via the course, for detailed info about its origin and origin of variables see CodeBook.md. This file contains mainly the script with thorough comments describing what and why was done in each step.

## INSTRUCTIONS 

You should create one R script called run_analysis.R that does the following: 

1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement. 
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names.
5. From the data set in step 4, creates a second, independent tidy data set with the average
      of each variable for each activity and each subject. #summarise_each(oldDF(funs(mean))


## SCRIPT


### 1. Merging
*1. Merges the training and the test sets to create one data set.*

set working directory with downloaded data - data should be downloaded and extracted prior to analysis
	
	setwd("D:/moje/Coursera/Data Cleaning/project")

load train sets and merge them together
	
	subject_train<-read.table("./train/subject_train.txt")
	X_train<-read.table("./train/X_train.txt")
	y_train<-read.table("./train/y_train.txt")

	trainset<-cbind(X_train, subject_train, y_train)

load test sets and merge them together
	
	subject_test<-read.table("./test/subject_test.txt")
	X_test<-read.table("./test/X_test.txt")
	y_test<-read.table("./test/y_test.txt")

	testset<-cbind(X_test, subject_test, y_test)

merge testset and trainset together to create working dataset called "a"
	
	a<-rbind(trainset, testset)

name the working dataset so we know what is in each collumn
	
	features<-read.delim("features.txt", header=F, sep=" ", stringsAsFactors=FALSE) 

*thanks to stringsAsFactors=FALSE the features are as characters and not as factors*

	names(a)<-c(features$V2, "test_subject", "activity_code")


get rid of unnecessary files to clear memory and Global environment

	rm(subject_test, subject_train, X_test, X_train, y_test, y_train, testset, trainset)




## 2. Mean and std 
*2. Extracts only the measurements on the mean and standard deviation for each measurement.*

I choose only the means and stds of measurements not everything with mean in name
	
	a_mean<-a[, grep(pattern="mean()", x=features$V2, fixed=T)]
	a_std<-a[, grep(pattern="std()", x=features$V2, fixed=T)]
	a_rest<-a[,562:563] #to save columns with activity and test subject id

	a_extract <- cbind(a_rest, a_mean, a_std) #put all the sets together

get rid of unnecessary files

	rm(a, features, a_mean, a_std, a_rest)





######################## 3. Activities ################### 
##3. Uses descriptive activity names to name the activities in the data set

activity_labels<-read.table("activity_labels.txt", header=F) 
a_label<-merge(a_extract, activity_labels, by.y= "V1", by.x="activity_code")

library(dplyr) #it is easier to use dplyr package then try to rename column without it
a_label<-rename(a_label, activity=V2)

#get rid of unnecessary files
a_label<-a_label[,-1] #no need for "activity_code" anymore,
rm(a_extract, activity_labels)




######################## 4. Labeling ################### 
##4. Appropriately labels the data set with descriptive variable names.

# My changes are based on post by ly in 
#https://class.coursera.org/getdata-031/forum/thread?thread_id=185#comment-780
#as I find his solution being quite comprehensive and readable.

l<-names(a_label)
l<-sub("BodyBody", l, replacement = "Body") #remove typo

#t -> time & f -> freq
l<-sub("tBody", l, replacement = "timeBody")
l<-sub("tGr", l, replacement = "timeGr")
l<-sub("fBody", l, replacement = "freqBody")

#Acc -> Linear & Gyro -> AngularVelocity & Mag -> Magnitude
l<-sub("Acc", l, replacement = "_Acceleration")
l<-sub("Gyro", l, replacement = "_AngularVelocity")
l<-sub("Mag", l, replacement = "_Magnitude")

# std, mean, X, Y, Z
l<-sub("-std()-X", l, replacement = "_X_std", fixed=T)
l<-sub("-std()-Y", l, replacement = "_Y_std", fixed=T)
l<-sub("-std()-Z", l, replacement = "_Z_std", fixed=T)
l<-sub("-mean()-X", l, replacement = "_X_mean", fixed=T)
l<-sub("-mean()-Y", l, replacement = "_Y_mean", fixed=T)
l<-sub("-mean()-Z", l, replacement = "_Z_mean", fixed=T)
l<-sub("-mean()", l, replacement = "_mean", fixed=T)
l<-sub("-std()", l, replacement = "_std", fixed=T)

names(a_label)<-l

#get rid of unnecessary files
rm(l)



######################## 5. Tidy dataset ################### 
##5. From the data set in step 4, creates a second, independent tidy data set 
##      with the average of each variable for each activity and each subject. 

a_tbl<-as.tbl(a_label)

a_fin<-a_tbl %>% group_by(activity, test_subject) %>% summarise_each(funs(mean))

#get rid of unnecessary files
rm(a_label, a_tbl)


## Optional: Export the tidy dataset from R
write.table(a_fin, "tidy_dataset.txt", row.name=F)
