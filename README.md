# Prophet-forecasting-system
-

In this project, I am trying to work with a data frame downloaded from library prophet. 


```{r eval=FALSE, include=TRUE}
library(prophet)
#define the datafrome
co2.df = data.frame(ds=zoo::as.yearmon(time(co2)),y=co2)
#define the model
model = prophet(df = co2.df, weekly = TRUE, daily = TRUE)
#define the future value
future.df = make_future_dataframe(model, periods=8, freq="quarter")
#predict the future value
pred.df = predict(model, future.df)
#plot the graph
plot(model,pred.df)
```

After we run the results, we have the following graph. 


```{r, echo=FALSE}
htmltools::img(src = knitr::image_uri("QMlogo.png"), 
               alt = 'logo', 
               style = 'position:absolute; top:0; right:0; padding:10px; width:30%;')

co2.df = data.frame(ds=zoo::as.yearmon(time(co2)),y=co2)
model = prophet::prophet(df = co2.df, weekly = TRUE, daily = TRUE)
future.df = prophet::make_future_dataframe(model, periods=8, freq="quarter")
pred.df = predict(model, future.df)
plot(model,pred.df)


```


CO2 is a dataset whose description is given in the following paragraph. 

Atmospheric concentrations of CO2 are expressed in parts per million (ppm) and reported in the preliminary 1997 SIO manometric mole fraction scale.
Format

A time series of 468 observations; monthly from 1959 to 1997.


The values for February, March and April of 1964 were missing and have been obtained by interpolating linearly between the values for January and May of 1964.

Source

Keeling, C. D. and Whorf, T. P., Scripps Institution of Oceanography (SIO), University of California, La Jolla, California USA 92093-0220.
https://scrippsco2.ucsd.edu/data/atmospheric_co2/.
Note that the data are subject to revision (based on recalibration of standard gases) by the Scripps institute, and hence may not agree exactly with the data provided by R.

References

Cleveland, W. S. (1993) Visualizing Data. New Jersey: Summit Press.

The purpose of the project is to find the relationship between the time and the co2 concentration. Also to find the trend and the seasonality of this regresstion model.

There is a clear upward trend, indicating that CO2 concentrations have been rising over the period represented. There are regular fluctuations within each year, suggesting a seasonal pattern. This is characteristic of CO2 measurements, which often have higher concentrations in the winter and lower during the summer due to the uptake of CO2 by plants during their growing season.The slope of the trend appears to be getting steeper as time progresses, implying that the rate of increase in CO2 concentration is accelerating.

The earliest number on record is about 313 and the highest is around 367. On average, the numbers go up from the 320s in the early years to about 350s in the later years. Most of the time, the numbers fall somewhere in the middle around 335.
 ds             y        
 Min.   :1959   Min.   :313.2  
 1st Qu.:1969   1st Qu.:323.5  
 Median :1978   Median :335.2  
 Mean   :1978   Mean   :337.1  
 3rd Qu.:1988   3rd Qu.:350.3  
 Max.   :1998   Max.   :366.8 


Now we can run some linear regression on the Co2 Data. 



```{r eval=FALSE, include=TRUE}
co2.df = data.frame(ds=zoo::as.yearmon(time(co2)),y=co2)
time <- unlist(co2.df["ds"])
yy <- as.numeric(unlist(co2.df["y"]))
co2.newdf = data.frame(time, yy)
model2 = lm(yy ~ time)
summary(model2)
plot (time , yy)
points(time , fitted (model2), type ="l")
```


```{r, echo=FALSE}
co2.df = data.frame(ds=zoo::as.yearmon(time(co2)),y=co2)
time <- unlist(co2.df["ds"])
yy <- as.numeric(unlist(co2.df["y"]))
co2.newdf = data.frame(time, yy)
model2 = lm(yy ~ time)
summary(model2)
plot (time , yy)
```



We then calculate the trend by taking the average of the data. When we substract the trend from the data we get the seasonality and the noise. 

```{r eval=FALSE, include=TRUE}
n=12
a=rep(1/n, n)
movingaverage = stats::filter(yy, a)
plot.new()
frame()
plot (time , movingaverage)
s = yy - movingaverage
plot(time, s)
```


```{r, echo=FALSE}
n=12
a=rep(1/n, n)
movingaverage = stats::filter(yy, a)
plot.new()
frame()
plot (time , movingaverage)
s = yy - movingaverage
plot(time, s)
```



```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
