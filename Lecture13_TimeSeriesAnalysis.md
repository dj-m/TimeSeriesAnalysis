# Lecture 13: Time Series Analysis #
Notes from Jordan Kern's <a href = "https://www.youtube.com/watch?v=Prpu_U5tKkE" target = "_blank">Lecture 13</a> video.

Definition of Time Series: An _ordered_ sequence of values of a variable at equally spaced time intervals.

## Time Series has _some things_ in common with probability & regression ##

- Mean & Standard Deviation
  - The mean and/or variance (standard deviations) change through time series processes despite the parts being described, in many cases, through randomized variable with statistical moments.
  
If you sampled/pulled from a time series distribution, would you be able to plot out the data points in a similar fashion to the image on the left, below:

 ![x](images/timeseries_distribution.png)

The answer is **no**
- If the value of x(t) depends in any significant way on the value of x(t-1).
  - These a hints of what's called "memory" in a time series. 
- Trends tend to persist in a time series, but overall there is no trend.
  - Another words for this is **autocorrelation**
  
- Regression
  - time series analysis is focused on identifying underlying trends and patterns, describing them mathematically, and ultimtely making a prediction or forecast about what will happen next.
  - But where regression aims to quantify the specific impacts of of specific underlying independent variables (Y = b1x1 + b2x2 + b3x3 ...), time series modeling allows for the replication of every element in the process by combining them into **signals** (e.g. year-on-year growth in electrivity demand, etc.) and **noise** (random probabilistic processes) which adds some level of uncertainty.

| ![x](images/signal_noise.png) |
| :-: |
| In the bottom plot, you see some indication of a trend/signal, but with the added noise or uncertainty around it. |
  
Common terms describing the nature of both signal **and** noise of a time series process:

- Statistical moments
  - Mean & Standard Deviation
- Stationary vs. non-stationary
  - Trends in mean and/or standard deviation (whether things change over time)
- Seasonality
  - Periodic patterns
- Autocorrelation
  - Degree to which time series values in period (t) are related to time series values in periods (t+1, t+2, ...)
  
## Pre-processing & Filtering ##

A big part of time series analysis involves filtering (changing attributes of a time series or deconstructing it into its component parts). Often, you'll do quite a bit of time-series before building a model to simulate the underlying process.

Filters tha can be applied to time series data:
- Detrending 
  - Non-stationarity (accounting for a change in mean over time)
  - Seasonality (things changing on a cyclic nature/periodic trends)
- Autocorrelation (memory in the system)
- Outliers (trying to et rid of all the signal and leave behind the noise)
- 'Low Pass' filters
  - Smoothing (getting rid of the noise inthe system and only allowing big changes to be seen in the system)
    - Moving average
	- Exponential

### Non-stationarity ###

| ![x](images/nonstationarity.png) |
| :-: |
| The standard deviation nor the seasonality is changing just the mean (on an upward trend) |
| :-: |
| ![x](images/detrended.png) |
| :-: |
| Given a constant and time, we can estimate what the concetrations of CO2 may be. Given that model, we can subtract what the CO2 concentration is and we're left with a detrended CO2 data. Detrended in the sense of year to year. There's still a seasonality trend here, but the mean is centered around zero.<br><br>It's clearly not white noise. The mean, centered around 0, can have it's residuals described around a probaility distribution but, there's autocorrelation. The value of CO2 concentration from one month to the next is very closely related to one another. |

### Differencing ###

If the series has a long-runnign trend and tends to revert to the trend line following a distrubance, it be possible to stationarize it by de-trending (e.g. by fitting a trend line and subtracting it out prior to fitting a model). Such a series is said to be **trend-stationary**.

However, sometimes even de-trending is not sufficient to make the series stationary. In this scenario, it may ne necessary to transform it into a series of period-to-period and/or season-to-season **differences**.

If the mean, variance and autocorrelations of the original series are not constant in time, even after detrending, perhaps statistics of the _changes_ in the series between periods or between seasons _will_ be constant. Such a series is said to be ** difference-stationary**.

| ![x](images/differencing_1.png) |
| :-: |
| The points in the right plot are the differences between two consecutive days of the left plot. What we get is that the mean is roughly 0 and has less nonstationarity. The variance or standard deviation for the right plot is not stationary though. |

If the first difference of Y is stationary and also _completely random_ (not autocorrelated), then Y is described by a **random walk model**: each value is a random step away from the previous value. 

| ![x](images/randomwalk.png) |
| :-: |
| We all start at 0 but each individual simulation, which is a different color, the evolution of the W value is a random value. There's memory in the system. The value that we get today is directly dependent on the value we got yesterday, but how we got from yesterday to today's value is a random value. |

### Seasonality ###

If seasonality (periodic fluctuations) is present, it must be incorporated into a time series model. **How do we detect it?**

- One of the simplest ways is using box plots.

| ![x](images/seasonality.png) |
| :-: |
| Here, we're starting with residuals, centered around 0, and trying to remove the seasonality and understand how CO2 concentrations change over time if we take out year-to-year trends.<br><br>In the bottom, what we have is something that looks like a random walk. It kind of looks like white noise but there's some memory left in the system (values stay above 0 for a while, then below 0 for a while), therefore it can't be replicated by sampling random values and doesn't represent a random process. |

### Autocorrelation ###

One of the fundamental differences between many time series and a random sampling from a single, known distribution, is that time series often have 'memory' i.e. values in one time period influence values in subsequent time periods.

We quantify this memory as **autocorrelation**

The autocorrelation function can be used for the following two purposes:
- To detect non-randomness in data
- To identify an appropriate time series model if the data are not random

| ![x](images/autocorrelation.png) |
| :-: |
| The formula for correlation between random values in a distribution and autocorrelation is very similar, except it's looking at the values in different time-shifts. Here you're taking the sum of Y, at time period i, minus the mean of Y, times the value of Y, at time period i plus k minus the mean of Y... then dividing by Yi minus the mean of Y, squared. |

If we have a strong correlation between every 10 time periods, then that gives us _periodicity_ or when certain values will happen again or about the same.

Autocorrelation can be tested at as many lags as we want, depending on the length of the time series. 
- Then we can plot autocorrelation as a function of lag. This tests how much memory, or correlation is exhibited between different points, changes over how far apart they are, in time. 
- If you get far enough away from each other, in terms of correlation, you can start to consider those values as independent from each other.

### White Noise ###

| ![x](images/whitenoise.png) |
| :-: |
| Once rid of all signal, what you have left is a sort of random variable, like the bottom left, and you can directly sample from it and plot the leftover time series process (bottom right) |

### Smoothing ###

It removes the importance of high-frequency spectral components. So, noise, basically.

Electrical engineers call this a _low-pass filter_ because the low frequency variations are allowed to **pass through** the filter. In a low-pass filter, the low frequency (long-period) waves are barely affected by smoothing.


A _statistical filter_ or digital filter, is a **series of weights** that when cumulatively multiplied by consecutive values of a time series give you a filtered series. The series of weights is sometimes called the filtering function, or simply the filter.

| ![x](images/statisticalfilter.png) |
| :-: |
| In this example, the filtered values represents the summation of the weight (Filter) * values in the Time Series column. The value for the Tear 3 row is (0.25 * 17) + (0.5 * 10) + (0.25 * 22)  = 14.75 |

Smoothing filters can be applied in different ways:

- Linear decay takes a weight that changes as it progresses through the time series in a linear fashion.
- Moving average takes a weight that will change based on the mean values it comes across as it progresses through the time searies.

| ![x](images/smoothing.png) |
| :-: |
| As your filter is being applied, you're often taking in several values and outputting just one which means that at the beginning, you have less values making up the single value you u=output, and at the end as well. Whether you modify or get rid of that data depends on what problems you're trying to answer. |

**The goal of smoothing is to bring out the signal and get rid of some of the noise.** You want to retain high amplitude events that don't occur very often and get rid of events that occur very frequently.

| ![x](images/frequency.png) |
| :-: |
| What we're left with, looking at the _Filtered_ line, are the high amplitude, low frequency events that really are important for driving the system. Some information is lost though, as information above 0.1 is basically removed from the plot. |

Smoothing allows you to more easily evaluate and describe the time series without giving too much importance to the more random events that happen around the signal that we care about.


## Resources ##

- <a href = "https://www.itl.nist.gov/div898/handbook/pmc/section4/pmc4.htm" target = "_blank">Engineering Statistics Handbook</a> from National Institute of Standards and Technology (NIST)
- <a href = "https://www.ltrr.arizona.edu/~dmeko/geos585a.html" target = "_blank">Course Materials</a> for GEOS 585A, Applied Time Series Analysis (Laboratory of Tree-Ring Research, University of Arizona)
- <a href = "http://people.duke.edu/~rnau/411diff.htm" target = "_blank">Stationarity & Differencing</a> section of Robert Nau's <a href = "http://people.duke.edu/~rnau/411home.htm" target = "_blank">Statistical Forecasting</a> notes (Fuqua School of Business, Duke University).