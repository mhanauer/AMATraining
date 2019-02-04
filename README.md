---
title: "AMA Project"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Library packages
```{r}
library(caret)
```
```{r}
(2/12)
33000*1.4*.05
33000*1.4*.1
40*.1
```

Simulate some data
```{r}
dat = data.frame(var1 = rnorm(n = 2500, mean = 5, sd = 2), var2 = rnorm(n = 2500, mean = 5, sd = 2), ama = c(rep(1,2500/2), rep(0, 2500/2)))
head(dat)
dat$ama = as.factor(dat$ama)
```
Now try carot model
```{r}
inTrain = createDataPartition(y = dat$ama, p = .50, list = FALSE)
training = dat[inTrain,]
testing = dat[-inTrain,] 
head(testing)
```
Now just set up generic model
```{r}
fitControl <- trainControl(
  method = "repeatedcv",
  number = 10,
  repeats = 10)
```
Now just run generic model
```{r}
set.seed(12345)
library(e1071)
gbmFit1 <- train(ama ~ ., data = training, 
                 method = "gbm", 
                 trControl = fitControl,
                 verbose = FALSE)


gbmFit2 <- train(ama ~ ., data = training, 
                 method = "pls", 
                 trControl = fitControl,
                 verbose = FALSE)

```
Now see what kind of summary statistics you can get
```{r}
gbmFit1
summary(gbmFit1)
gbmFit1
resample_test =  resamples(list(pls = gbmFit2, gbm = gbmFit1))
summary(resample_test)

diff_test = diff(resample_test)
summary(diff_test)
```







