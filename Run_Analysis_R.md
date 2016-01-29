getwd()
"C:/Users/patelmon/Desktop/Coursera/Specdata"
setwd("C:/Users/patelmon/Desktop/Coursera/GettingCleaningDataAssignment/UCI HAR Dataset/test")
getwd()
"C:/Users/patelmon/Desktop/Coursera/GettingCleaningDataAssignment/UCI HAR Dataset/test"
setwd("C:/Users/patelmon/Desktop/Coursera/GettingCleaningDataAssignment/UCI HAR Dataset")
#First load test data
DATAsubject_test <- read.table("test/subject_test.txt", header = FALSE)
DATAX_test <- read.table("test/X_test.txt", header = FALSE)
DATAy_test <- read.table("test/y_test.txt", header = FALSE)
#Second load training data
DATAsubject_train <- read.table("train/subject_train.txt", header = FALSE)
DATAX_train <- read.table("train/X_train.txt", header = FALSE)
DATAy_train <- read.table("train/y_train.txt", header = FALSE)

#Third load Activity Files
DATAActivityTest  <- read.table("test/Y_test.txt",header = FALSE)
DATAActivityTrain  <- read.table("train/Y_train.txt",header = FALSE)

#Load Subject files
DATASubjectTest  <- read.table("test/subject_test.txt",header = FALSE)
DATASubjectTrain  <- read.table("train/subject_train.txt",header = FALSE)

#Load Features Files
DATAFeaturesTest  <- read.table("test/X_test.txt",header = FALSE)
DATAFeaturesTrain  <- read.table("train/X_train.txt",header = FALSE)

#Combine the tables
DataSubject <- rbind(DATASubjectTrain, DATASubjectTest)
DataFeatures <- rbind(DATAFeaturesTrain, DATAFeaturesTest)
DataActivity <- rbind(DATAActivityTrain, DATAActivityTest)

#Name Variables
names(DataSubject)<-c("Subject")
names(DataActivity)<- c("Activity")
dataFeaturesNames <- read.table ("features.txt",head=FALSE)
names(DataFeatures)<- dataFeaturesNames$V2

DataCombine <- cbind(DataSubject, DataActivity)
DATA <- cbind(DataFeatures, DataCombine)

#Mean and St Dev
FeaturesNames <- dataFeaturesNames$V2[grep("mean\\(\\)|std\\(\\)", dataFeaturesNames$V2)]

SelectedNames <-c(as.character(FeaturesNames), "Subject", "Activity" )

DaTA<-subset(DATA,select=SelectedNames)

#DEscriptive Activty Names from file Activity_Labels.txt
ActivityLabels <- read.table("activity_labels.txt",head=FALSE)

DataActivity <- as.character(DataActivity)
DataActivity[DataActivity == 1] <- "Walking"
DataActivity[DataActivity == 2] <- "Walking Upstairs"
DataActivity[DataActivity == 3] <- "Walking Downstairs"
DataActivity[DataActivity == 4] <- "Sitting"
DataActivity[DataActivity == 5] <- "Standing"
DataActivity[DataActivity == 6] <- "Laying"
DataActivity <- as.factor(DataActivity)

#Descriptive Labels 
names(DaTA)<-gsub("^t", "time", names(DaTA))
names(DaTA)<-gsub("^f", "frequency", names(DaTA))
names(DaTA)<-gsub("Acc", "Accelerometer", names(DaTA))
names(DaTA)<-gsub("Gyro", "Gyroscope", names(DaTA))
names(DaTA)<-gsub("Mag", "Magnitude", names(DaTA))
names(DaTA)<-gsub("BodyBody", "Body", names(DaTA))

#Second Data Set
DataSet2<-aggregate(. ~Subject + Activity, DaTA, mean)
DataSet2<-DataSet2[order(DataSet2$Subject,DataSet2$Activity),]
write.table(DataSet2, file = "tidydata.txt",row.name=FALSE)