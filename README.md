## Download and unzip dataset
file<-"https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(file,"./data/Dataset.zip")
unzip(zipfile="./data/Dataset.zip",exdir="./data")

## Reading files
  x_train<-read.table(file="C:/Users/conni/Documents/UCI HAR Dataset/train/X_train.txt")
  y_train<-read.table(file="C:/Users/conni/Documents/UCI HAR Dataset/train/y_train.txt")
  sub_train<-read.table(file="C:/Users/conni/Documents/UCI HAR Dataset/train/subject_train.txt")
  x_test<-read.table(file="C:/Users/conni/Documents/UCI HAR Dataset/test/X_test.txt")
  y_test<-read.table(file="C:/Users/conni/Documents/UCI HAR Dataset/test/y_test.txt")
  sub_test<-read.table(file="C:/Users/conni/Documents/UCI HAR Dataset/test/subject_test.txt")
  features<-read.table(file="C:/Users/conni/Documents/UCI HAR Dataset/features.txt")
  actLabels=read.table(file="C:/Users/conni/Documents/UCI HAR Dataset/activity_labels.txt")
  
 ## Assign column names
  colnames(x_train)<-features[,2]
  colnames(y_train)<-"activityID"
  colnames(sub_train)<-"subjectID"
  colnames(x_test)<-features[,2]
  colnames(y_test)<-"activityID"
  colnames(sub_test)<-"subjectID"
  colnames(actLabels)<-c('activityID','activityType')
  
 ## Merge all data
  mrg_train<-cbind(y_train,sub_train,x_train)
  mrg_test<-cbind(y_test,sub_test,x_test)
  Set<-rbind(mrg_train,mrg_test)
  
 ## Read column names
  colNames<-colnames(Set)
  
 ## Extract measurements on mean and standard deviation
  mean_and_std<-(grepl("activityID",colNames)|grepl("subjectID",colNames)|grepl("mean..",colNames)|grepl("std..",colNames))
  setForMeanStd<-Set[,mean_and_std==T]
  
 ## Use descriptive activity names to name activities in the dataset
  ActNames<-merge(setForMeanStd,actLabels,by='activityID',all.x=T)
  
 ## Create a second tidy data set 
TidySet2<-aggregate(.~subjectID+activityID,ActNames,mean)
TidySet2<-TidySet2[order(TidySet2$subjectID,TidySet2$activityID),]
 write.table(TidySet2,"TidySet.txt",row.name=F)
