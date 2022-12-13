# PullIng_bacdive_data

```
#install if necessary
install.packages("BacDive", repos="http://R-Forge.R-project.org")

#load BacDive package
library(BacDive)
library(plyr)
library(dplyr)
library(tidyverse)

#read in list of all species on BacDive (from downloads section)
 bacdiv_sp<-read.csv("./advsearch_bacdive_2022-12-13.csv", header=T)

#connect to bacdive
bacdiv<-open_bacdive('patrick.kearns@umb.edu', 'Sebbyisanebby1@3')
print(bacdiv)

#test bacdiv out on one species just in case
test<-as.data.frame(BacDive::fetch(bacdiv, bacdiv_sp[1,1]))

#all BacDive data
#first split data into blocks of 100 (that's the maximum)
#will give error, but that cool because the dataset isn't perfectly dividable by 100
d <- split(bacdiv_sp,rep(1:922,each=100))

#funtion to get the information for all IDs in the dataframe loaded above
#can take a while if a lot of IDs (~5 min on Woodhams lab Mac)
d_bacdiv<-lapply(d, function(x) BacDive::fetch(bacdiv, x$ID))

#make all data a dataframe
test2<-lapply(d_bacdiv, function(x) as.data.frame(rbind(x$results)))
test3<-lapply(test2, function(x) as.data.frame(t(x)))
df <- ldply(test3, data.frame)
```
