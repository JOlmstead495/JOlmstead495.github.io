---
layout: post
title: The Quick Run Down On Binning In MatPlotLib
---

### Quick Intro
```
Bin There Done That
```
That's how I'm starting out my blog, with a bad pun in a quote that doesn't have anything to do with my main topic - this is my first blog give me a break. For my first blog, I will be giving a how-to on binning values into discrete intervals, and when it can be a useful tool.

### Cut Method
```
When the grass is cut, the snakes will show - Jay Z
```
I guess I'm going with the bad quote theme. Sorry.
The tool we have in our pandas repertoire to bin values is `pandas.cut`. Cut is a method that takes a few different parameters; the two parameters we'll focus on are the only ones you need to add to do a successful cut.
```
x - 1st parameter you pass in. This must be a 1 dimensional array (list or series will work). This is the input array you want to bin.
bins - 2nd parameter you pass in. This can be an int, a scaling sequence, or an exact 1 dimensional array.
    - Int - This defines the number of equal-width bins in the range of x.
        - Ex- If x = [1,2,3,4,10,9,8,1] and bins=5. You'll have 5 bins (1:2),(3:4) to (9:10).
    - Scaling Sequence - A 1 dimensional array that is created by iterating over a minumum and maximum number.
        - Ex - np.linspace(1,10,2) - would act similar to the above Int example.
    - 1 Dimensional Array - An already set 1 dimensional array
        - Ex - ['1-2','3-4','5-6','7-8','9-10'] would act similarly to the above example.
```
[Pandas has further documentation on this method.](https://pandas.pydata.org/pandas-docs/version/0.23.4/generated/pandas.cut.html). I used this to bin data that had an input type set to 0-24, representing hours within a day.

### Code Example
```
Oh, Harold? You see what I'm getting at? If Santiago didn't have anything on you... why did you give him a Code Red? - Tom Cruise as Dan Kafee in A Few Good Men
```
Here is how I used the .cut method to bin hours.
`df` is my data frame.
`HOURS_BINNED` is the new column created to bin hours.
`HOURS` is the old column of hours (0-24) that I'm binning.
```
df[HOURS_BINNED]=pandas.cut(df[HOURS],[0,4,8,12,16,20,24])
```
What this does is create 7 bins and will sort each value in `df[HOURS]` into those bins.


### Conclusion
```
A conclusion is simply the place where you got tired of thinking. - Dan Chaon
```
To conclude, binning can be used to turn quantitative data into categorical data, and can be useful for agregating times/data. Hopefully you have learned something from this blog!