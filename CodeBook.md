##General info
Data and information obtained via Coursera Getting and Cleaning Data Course. Below are the original comments together with appropriate citation of the source. From the original dataset I choosed just some of the variables and changed their names. In the text below the names of variables are changed so they correspond with the final tidy dataset and omitted variables are omitted form the text too.

Data used in project:
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

------
Human Activity Recognition Using Smartphones Dataset
Version 1.0
--
Jorge L. Reyes-Ortiz, Davide Anguita, Alessandro Ghio, Luca Oneto.
Smartlab - Non Linear Complex Systems Laboratory
DITEN - Università degli Studi di Genova.
Via Opera Pia 11A, I-16145, Genoa, Italy.
activityrecognition@smartlab.ws
www.smartlab.ws

--
## Citation:
**Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. Human Activity Recognition on Smartphones using a Multiclass Hardware-Friendly Support Vector Machine. International Workshop of Ambient Assisted Living (IWAAL 2012). Vitoria-Gasteiz, Spain. Dec 2012**

This dataset is distributed AS-IS and no responsibility implied or explicit can be addressed to the authors or their institutions for its use or misuse. Any commercial use is prohibited.

Jorge L. Reyes-Ortiz, Alessandro Ghio, Luca Oneto, Davide Anguita. November 2012.

==================================================================

## Experiment setting

The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' in downloaded files for more details. 

For each record it is provided:
- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 
- An identifier of the subject who carried out the experiment.


##Feature Selection 


The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals time_Acceleration_XYZ and time_AngularVelocity_XYZ. These time domain signals were captured at a constant rate of 50 Hz. Then they were filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove noise. Similarly, the acceleration signal was then separated into body and gravity acceleration signals (timeBody_Acceleration_XYZ and timeGravity_Acceleration_XYZ) using another low pass Butterworth filter with a corner frequency of 0.3 Hz. 

Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals (timeBody_AccelerationJerk_XYZ and timeBody_AngularVelocityJerk_XYZ). Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm (timeBody_Acceleration_Magnitude, timeGravity_Acceleration_Magnitude, timeBody_AccelerationJerk_Magnitude, timeBody_AngularVelocity_Magnitude, timeBody_AngularVelocityJerk_Magnitude). 

Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing freqBody_Acceleration_XYZ, freqBody_AccelerationJerk_XYZ, freqBody_AngularVelocity_XYZ, freqBody_AccelerationJerk_Magnitude, freqBody_AngularVelocity_Magnitude, freqBody_AngularVelocityJerk_Magnitude. (Note the 'freq' to indicate frequency domain signals). 

These signals were used to estimate variables of the feature vector for each pattern: '_XYZ' is used to denote 3-axial signals in the X, Y and Z directions.
The set of variables that were estimated from these signals are: 

mean(): Mean value
std(): Standard deviation


Finaly the means for each activity (see below) and each test subject (30 people) were calculated and are reported in final tidy dataset (tidy_dataset.txt). 


1 activity
2 test_subject
3 timeBody_Acceleration_X_mean
4 timeBody_Acceleration_Y_mean
5 timeBody_Acceleration_Z_mean
6 timeGravity_Acceleration_X_mean
7 timeGravity_Acceleration_Y_mean
8 timeGravity_Acceleration_Z_mean
9 timeBody_AccelerationJerk_X_mean
10 timeBody_AccelerationJerk_Y_mean
11 timeBody_AccelerationJerk_Z_mean
12 timeBody_AngularVelocity_X_mean
13 timeBody_AngularVelocity_Y_mean
14 timeBody_AngularVelocity_Z_mean
15 timeBody_AngularVelocityJerk_X_mean
16 timeBody_AngularVelocityJerk_Y_mean
17 timeBody_AngularVelocityJerk_Z_mean
18 timeBody_Acceleration_Magnitude_mean
19 timeGravity_Acceleration_Magnitude_mean
20 timeBody_AccelerationJerk_Magnitude_mean
21 timeBody_AngularVelocity_Magnitude_mean
22 timeBody_AngularVelocityJerk_Magnitude_mean
23 freqBody_Acceleration_X_mean
24 freqBody_Acceleration_Y_mean
25 freqBody_Acceleration_Z_mean
26 freqBody_AccelerationJerk_X_mean
27 freqBody_AccelerationJerk_Y_mean
28 freqBody_AccelerationJerk_Z_mean
29 freqBody_AngularVelocity_X_mean
30 freqBody_AngularVelocity_Y_mean
31 freqBody_AngularVelocity_Z_mean
32 freqBody_Acceleration_Magnitude_mean
33 freqBody_AccelerationJerk_Magnitude_mean
34 freqBody_AngularVelocity_Magnitude_mean
35 freqBody_AngularVelocityJerk_Magnitude_mean
36 timeBody_Acceleration_X_std
37 timeBody_Acceleration_Y_std
38 timeBody_Acceleration_Z_std
39 timeGravity_Acceleration_X_std
40 timeGravity_Acceleration_Y_std
41 timeGravity_Acceleration_Z_std
42 timeBody_AccelerationJerk_X_std
43 timeBody_AccelerationJerk_Y_std
44 timeBody_AccelerationJerk_Z_std
45 timeBody_AngularVelocity_X_std
46 timeBody_AngularVelocity_Y_std
47 timeBody_AngularVelocity_Z_std
48 timeBody_AngularVelocityJerk_X_std
49 timeBody_AngularVelocityJerk_Y_std
50 timeBody_AngularVelocityJerk_Z_std
51 timeBody_Acceleration_Magnitude_std
52 timeGravity_Acceleration_Magnitude_std
53 timeBody_AccelerationJerk_Magnitude_std
54 timeBody_AngularVelocity_Magnitude_std
55 timeBody_AngularVelocityJerk_Magnitude_std
56 freqBody_Acceleration_X_std
57 freqBody_Acceleration_Y_std
58 freqBody_Acceleration_Z_std
59 freqBody_AccelerationJerk_X_std
60 freqBody_AccelerationJerk_Y_std
61 freqBody_AccelerationJerk_Z_std
62 freqBody_AngularVelocity_X_std
63 freqBody_AngularVelocity_Y_std
64 freqBody_AngularVelocity_Z_std
65 freqBody_Acceleration_Magnitude_std
66 freqBody_AccelerationJerk_Magnitude_std
67 freqBody_AngularVelocity_Magnitude_std
68 freqBody_AngularVelocityJerk_Magnitude_std



## Types of activities
1 WALKING
2 WALKING_UPSTAIRS
3 WALKING_DOWNSTAIRS
4 SITTING
5 STANDING
6 LAYING


 
