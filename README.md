# LastHW

> rm(list=ls())
> setwd("C:/Coursera/DataScientist/PracticalML/")
Error in setwd("C:/Coursera/DataScientist/PracticalML/") : 
  cannot change working directory
> getwd()
[1] "C:/Users/Kahila/Documents"
> setwd( "C:/Users/Kahila/Documents"





+ setwd( "C:/Users/Kahila/Documents"> > > > > 

> > 
> setwd( "C:/Users/Kahila/Documents"


+ > > 
> setwd( "C:/Users/Kahila/Documents")
> library(caret)
Loading required package: lattice
Loading required package: ggplot2
> Urla <- "https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv"
> Urlb <- "https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv"
> training <- read.csv (url(Urla), na.strings = c("NA" , "#DIV/0!", ""))
> testing <- read.csv(url(Urlb), na.strings = c("NA", "#DIV/0!", ""))
> dim(training)
[1] 19622   160
> dim(testing)
[1]  20 160
> table(training$classe)

   A    B    C    D    E 
5580 3797 3422 3216 3607 
> NA_Count = sapply (1:dim(training)[2],function (x) sum(is.na(training[,x])))
> NA_list= which (NA_Count >0)
> colnames(training [,c(1:7])
Error: unexpected ']' in "colnames(training [,c(1:7]"
> colnames(training [,c(1:7)])
[1] "X"                    "user_name"            "raw_timestamp_part_1"
[4] "raw_timestamp_part_2" "cvtd_timestamp"       "new_window"          
[7] "num_window"          
> training=training[,-NA_list]
> training=training[,-c(1:7)]
> training$classe=factor(training$classe)
> testing=testing[,-NA_list]
> testing=testing[,-c(1:7)]
> set.seed(1234)
> cv3=trainControl(method="cv", number=3, allowParallel=TRUE, verboseIter=TRUE)
> modrf=train(classe~.,data=training,method="rf",trControl=cv3)
+ Fold1: mtry= 2 
- Fold1: mtry= 2 
+ Fold1: mtry=27 
- Fold1: mtry=27 
+ Fold1: mtry=52 
- Fold1: mtry=52 
+ Fold2: mtry= 2 
- Fold2: mtry= 2 
+ Fold2: mtry=27 
- Fold2: mtry=27 
+ Fold2: mtry=52 
- Fold2: mtry=52 
+ Fold3: mtry= 2 
- Fold3: mtry= 2 
+ Fold3: mtry=27 
- Fold3: mtry=27 
+ Fold3: mtry=52 
- Fold3: mtry=52 
Aggregating results
Selecting tuning parameters
Fitting mtry = 27 on full training set
> modtree=train(classe~.,data=training,method="rpart",trControl=cv3)
+ Fold1: cp=0.03568 
- Fold1: cp=0.03568 
+ Fold2: cp=0.03568 
- Fold2: cp=0.03568 
+ Fold3: cp=0.03568 
- Fold3: cp=0.03568 
Aggregating results
Selecting tuning parameters
Fitting cp = 0.0357 on full training set
> prf=predict(modrf,training)
> ptree=predict(modtree,training)
> table(prf,training$classe)
   
prf    A    B    C    D    E
  A 5580    0    0    0    0
  B    0 3797    0    0    0
  C    0    0 3422    0    0
  D    0    0    0 3216    0
  E    0    0    0    0 3607
> table(ptree,training$classe)
     
ptree    A    B    C    D    E
    A 5080 1581 1587 1449  524
    B   81 1286  108  568  486
    C  405  930 1727 1199  966
    D    0    0    0    0    0
    E   14    0    0    0 1631
> prf=predict(modrf,testing)
> ptree=predict(modtree,testing)
> table(prf,ptree)
   ptree
prf A B C D E
  A 7 0 0 0 0
  B 3 0 5 0 0
  C 0 0 1 0 0
  D 0 0 1 0 0
  E 1 0 2 0 0
> answers=predict(modrf,testing)
> pml_write_files=function(x){

+ > pml_write_files=function(x){n=length(x) for(i in 1:n){ filename=paste0("problem_id_", i, ".txt") write.table(x[i], file=filename, quote=FALSE, row.names=FALSE,col.names=FALSE)}}answers
Error: unexpected 'for' in "pml_write_files=function(x){n=length(x) for"
> answers=predict(modrf,testing)
> pml_write_files = function(x){
+   n = length(x)
+   for(i in 1:n){
+     filename = paste0("problem_id_",i,".txt")
+     write.table(x[i],file=filename,quote=FALSE,row.names=FALSE,col.names=FALSE)
+   }
+ }
> answers
 [1] B A B A A E D B A A B C B A E E A B B B
Levels: A B C D E
