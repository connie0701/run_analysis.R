 ## Download UCI HAR dataset manually then manually choose it by file.choose() while reading files
  x_train<-read.table(file.choose())
  y_train<-read.table(file.choose())
  sub_train<-read.table(file.choose())
  x_test<-read.table(file.choose())
  y_test<-read.table(file.choose())
  sub_test<-read.table(file.choose())
  features<-read.table(file.choose())
  actLabels=read.table(file.choose())
  
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
> TidySet2<-aggregate(.~subjectID+activityID,ActNames,mean)
> TidySet2<-TidySet2[order(TidySet2$subjectID,TidySet2$activityID),]
> write.table(TidySet2,"TidySet.txt",row.name=F)
