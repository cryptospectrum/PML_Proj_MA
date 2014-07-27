PML_Proj_MA
===========

class project

Human Activity Recognition: Prediction with Caret
========================================================================
## Mark Aiello, 7/27/2014
### Practical Machine Learning
###Data Science Specialization, Coursera



Our task is to identify proper weightlifting technique from bio-sensor data.  We use machine learning algorithms to train with data as described in Velloso et. al.  
Just as is the case with many real world data sets the data is not ideal.  A large number of missing values are dealt with by
using nearZeroVar. First lets load the data.

```{r chunkLabel}
# R code
pmlD <- read.csv("pml-training.csv",na.strings=c("#DIV/0!",NA))
pmlDtesting<- read.csv("pml-testing.csv")
summary(pmlD)
str(pmlD)
```

The target variable to be predicted is the categorical variable assign a quality of performance to each observation.
Consider the variance of the accelerometer in the arm as a histogram of the different users.
```{r chunkLabel}
qplot(var_accel_arm, data=pmlD, geom="density", fill=user_name, alpha=I(.5),
   main="Variance in Arm Accelerometer", xlab="User",
   ylab="Density")
```
the then the same data by index.
```{r chunkLabel}
qplot(X, var_accel_arm, data=pmlD, geom=c("point", "smooth"),
method="lm", formula=y~x,
main="Variance in Arm Accelerometer",
xlab="Index", ylab="Variance")
```

The Following code for caret usage
```{r chunkLabel}
 pmlDnzv2 <-nearZeroVar(pmlD, freqCut = 95/5, uniqueCut = 10, saveMetrics = FALSE)
 pmlDsubZ <- pmlD[, -pmlDnzv2]
 pmlFit <- train(classe~.,data=pmlDsubZ,method="rf")
 predict(pmlFit,newdata=pmlDtesting)

```

Many different errors with many different version occurred, when a I got it working an atttempted ploting function call crashed Rstudio and I haven't been able to replicate my previous results as of 7/27 4:00

References
1)
Velloso, E.; Bulling, A.; Gellersen, H.; Ugulino, W.; Fuks, H. Qualitative Activity Recognition of Weight Lifting Exercises. Proceedings of 4th International Conference in Cooperation with SIGCHI (Augmented Human '13) . Stuttgart, Germany: ACM SIGCHI, 2013.

2)
#http://groupware.les.inf.puc-rio.br/har
