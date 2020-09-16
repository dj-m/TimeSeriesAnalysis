# Lecture 13: Time Series Analysis #
Notes from Jordan Kern's <a href = "https://www.youtube.com/watch?v=Prpu_U5tKkE" target = "_blank">video</a>

Definition of Time Series: An _ordered_ sequence of values of a variable at equally spaced time intervals.

Time Series has _some things_ in common with probability & regression

- Mean & Standard Deviation
  - The mean and/or variance (standard deviations) change through time series processes despite the parts being described, in many cases, through randomized variable with statistical moments.
  
If you sampled/pulled from a time series distribution, would you be able to plot out the data points in a similar fashion to the image on the left, below:

![x](/images/timeseries_distribution.png)

The answer is **no**
- If the value of x(t) depends in any significant way on the value of x(t-1).
  - These a hints of what's called "memory" in a time series. 
- Trends tend to persist in a time series, but overall there is no trend.
  - Another words for this is **autocorrelation**
  
- Regression
  - time series analysis is focused on identifying underlying trends and patterns, describing them mathematically, and ultimtely making a prediction or forecast about what will happen next.
