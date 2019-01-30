
# Reproducible Research: Peer Assessment 1




### Loading and preprocessing the data

Loading the data and transforming the data into a format suitable for analysis:

<div class="chunk" id="unnamed-chunk-1"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">fileUrl</span> <span class="hl kwb">&lt;-</span> <span class="hl str">&quot;https://d396qusza40orc.cloudfront.net/repdata/data/activity.zip&quot;</span>
<span class="hl kwd">download.file</span><span class="hl std">( fileUrl,</span><span class="hl kwc">destfile</span><span class="hl std">=</span><span class="hl str">&quot;activity.zip&quot;</span> <span class="hl std">)</span>
<span class="hl kwd">system</span><span class="hl std">(</span> <span class="hl str">&quot;unzip -o activity.zip&quot;</span> <span class="hl std">)</span>
<span class="hl std">activity</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">read.csv</span><span class="hl std">(</span><span class="hl str">&quot;activity.csv&quot;</span><span class="hl std">)</span>
</pre></div>
</div></div>
<div class="chunk" id="unnamed-chunk-2"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">( dplyr )</span>
<span class="hl std">activity</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">tbl_df</span><span class="hl std">( activity )</span>
</pre></div>
</div></div>



### Mean total number of steps taken per day

I ignore the missing values in the dataset for this part of the assignment.

1. Calculating the total number of steps taken per day: 
<div class="chunk" id="unnamed-chunk-4"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">plot_data</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">group_by</span><span class="hl std">( activity, date )</span> <span class="hl opt">%&gt;%</span>
    <span class="hl kwd">summarize</span><span class="hl std">(</span> <span class="hl kwc">steps_sum</span> <span class="hl std">=</span> <span class="hl kwd">sum</span><span class="hl std">(steps,</span><span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">) )</span>
</pre></div>
</div></div>
2. Making a histogram of the total number of steps taken each day:
<div class="chunk" id="unnamed-chunk-5"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com">#png( file=&quot;plot_sum_date.png&quot;,width = 480,height = 480 )</span>
<span class="hl kwd">with</span><span class="hl std">(plot_data,</span> <span class="hl kwd">barplot</span><span class="hl std">( plot_data</span><span class="hl opt">$</span><span class="hl std">steps_sum,</span> <span class="hl kwc">names.arg</span><span class="hl std">=plot_data</span><span class="hl opt">$</span><span class="hl std">date,</span> <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;Date&quot;</span><span class="hl std">,</span> <span class="hl kwc">ylab</span><span class="hl std">=</span><span class="hl str">&quot;Steps&quot;</span><span class="hl std">,</span> <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Total steps each day&quot;</span> <span class="hl std">))</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-5-1.png" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" class="plot" /></div>
<div class="source"><pre class="knitr r"><span class="hl com">#dev.off()</span>
</pre></div>
</div></div>
3. Calculating and reporting the mean and median of the total number of steps taken per day:
<div class="chunk" id="unnamed-chunk-6"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">steps_mean</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">mean</span><span class="hl std">( plot_data</span><span class="hl opt">$</span><span class="hl std">steps_sum,</span><span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span> <span class="hl std">)</span>
<span class="hl kwd">print</span><span class="hl std">(</span> <span class="hl kwd">paste</span><span class="hl std">(</span><span class="hl str">&quot;Mean of total number of steps taken per day:&quot;</span><span class="hl std">,steps_mean ) )</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] &quot;Mean of total number of steps taken per day: 9354.22950819672&quot;
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl std">steps_median</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">median</span><span class="hl std">( plot_data</span><span class="hl opt">$</span><span class="hl std">steps_sum,</span><span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span> <span class="hl std">)</span>
<span class="hl kwd">print</span><span class="hl std">(</span> <span class="hl kwd">paste</span><span class="hl std">(</span><span class="hl str">&quot;Median of total number of steps taken per day:&quot;</span><span class="hl std">,steps_median ) )</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] &quot;Median of total number of steps taken per day: 10395&quot;
</pre></div>
</div></div>

### Average daily activity pattern

1. Making a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis):
<div class="chunk" id="unnamed-chunk-7"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">plot_data</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">group_by</span><span class="hl std">( activity, interval )</span> <span class="hl opt">%&gt;%</span>
    <span class="hl kwd">summarize</span><span class="hl std">(</span> <span class="hl kwc">steps_sum</span>    <span class="hl std">=</span>    <span class="hl kwd">mean</span><span class="hl std">(steps,</span> <span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">))</span>
<span class="hl com">#png( file=&quot;plot_sum_time.png&quot;,width = 480,height = 480 )</span>
<span class="hl kwd">with</span><span class="hl std">(plot_data,</span> <span class="hl kwd">plot</span><span class="hl std">( interval, steps_sum,</span> <span class="hl kwc">type</span><span class="hl std">=</span><span class="hl str">&quot;l&quot;</span><span class="hl std">,</span> <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;5-min interval&quot;</span><span class="hl std">,</span> <span class="hl kwc">ylab</span><span class="hl std">=</span><span class="hl str">&quot;Steps&quot;</span><span class="hl std">,</span> <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Total steps each interval&quot;</span> <span class="hl std">))</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-7-1.png" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" class="plot" /></div>
<div class="source"><pre class="knitr r"><span class="hl com">#dev.off()</span>
</pre></div>
</div></div>
2. Getting 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps:
<div class="chunk" id="unnamed-chunk-8"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">max_int</span> <span class="hl kwb">&lt;-</span> <span class="hl std">plot_data[</span> <span class="hl kwd">which</span><span class="hl std">( plot_data</span><span class="hl opt">$</span><span class="hl std">steps_sum</span> <span class="hl opt">==</span> <span class="hl kwd">max</span><span class="hl std">(plot_data</span><span class="hl opt">$</span><span class="hl std">steps_sum)), ]</span>
<span class="hl kwd">print</span><span class="hl std">(</span> <span class="hl kwd">paste</span><span class="hl std">(</span><span class="hl str">&quot;5-minute interval contains the maximum number of steps is&quot;</span><span class="hl std">,max_int</span><span class="hl opt">$</span><span class="hl std">interval) )</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] &quot;5-minute interval contains the maximum number of steps is 835&quot;
</pre></div>
</div></div>

### Imputing missing values

This dataset contains a number of days/intervals where there are missing values (coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

1. Calculating and reporting the total number of missing values in the dataset:
<div class="chunk" id="unnamed-chunk-9"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">print</span><span class="hl std">(</span><span class="hl kwd">paste</span><span class="hl std">(</span><span class="hl str">&quot;Total number of missing values in the dataset is&quot;</span><span class="hl std">,</span><span class="hl kwd">sum</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(activity</span><span class="hl opt">$</span><span class="hl std">steps))))</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] &quot;Total number of missing values in the dataset is 2304&quot;
</pre></div>
</div></div>
2. Replacing all of the missing values in the dataset with the mean for that 5-minute interval:
<div class="chunk" id="unnamed-chunk-10"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">activity_f</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">group_by</span><span class="hl std">( activity, interval )</span>
<span class="hl std">activity_f</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">mutate</span><span class="hl std">( activity_f,</span> <span class="hl kwc">steps_m</span> <span class="hl std">=</span> <span class="hl kwd">mean</span><span class="hl std">(steps,</span><span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">))</span>
</pre></div>
</div></div>
3. Creating a new dataset that is equal to the original dataset but with the missing data filled in:
<div class="chunk" id="unnamed-chunk-11"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">activity_f</span> <span class="hl kwb">&lt;-</span> <span class="hl std">activity_f</span> <span class="hl opt">%&gt;%</span>
    <span class="hl kwd">mutate</span><span class="hl std">(</span> <span class="hl kwc">steps</span> <span class="hl std">=</span> <span class="hl kwd">if_else</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(steps),</span> <span class="hl kwd">as.integer</span><span class="hl std">(steps_m), steps) )</span>
<span class="hl std">activity_f</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">select</span><span class="hl std">(activity_f,</span> <span class="hl opt">-</span><span class="hl std">steps_m)</span>
</pre></div>
</div></div>
4. Making a histogram of the total number of steps taken each day and calculating and reporting the mean and median total number of steps taken per day:
<div class="chunk" id="unnamed-chunk-12"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">plot_data</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">group_by</span><span class="hl std">( activity_f, date )</span> <span class="hl opt">%&gt;%</span>
    <span class="hl kwd">summarize</span><span class="hl std">(</span> <span class="hl kwc">steps_sum</span>    <span class="hl std">=</span>    <span class="hl kwd">sum</span><span class="hl std">(steps))</span>
<span class="hl com">#png( file=&quot;plot_sum_date_na.png&quot;,width = 480,height = 480 )</span>
<span class="hl kwd">with</span><span class="hl std">(plot_data,</span> <span class="hl kwd">barplot</span><span class="hl std">( plot_data</span><span class="hl opt">$</span><span class="hl std">steps_sum,</span> <span class="hl kwc">names.arg</span><span class="hl std">=plot_data</span><span class="hl opt">$</span><span class="hl std">date,</span> <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;Date&quot;</span><span class="hl std">,</span> <span class="hl kwc">ylab</span><span class="hl std">=</span><span class="hl str">&quot;Steps&quot;</span><span class="hl std">,</span> <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Total steps each day after imputing missing values&quot;</span> <span class="hl std">))</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-12-1.png" title="plot of chunk unnamed-chunk-12" alt="plot of chunk unnamed-chunk-12" class="plot" /></div>
<div class="source"><pre class="knitr r"><span class="hl com">#dev.off()</span>
</pre></div>
</div></div>
<div class="chunk" id="unnamed-chunk-13"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">steps_mean</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">mean</span><span class="hl std">( plot_data</span><span class="hl opt">$</span><span class="hl std">steps_sum,</span><span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span> <span class="hl std">)</span>
<span class="hl kwd">print</span><span class="hl std">(</span> <span class="hl kwd">paste</span><span class="hl std">(</span><span class="hl str">&quot;Mean of total number of steps taken per day after imputing missing values:&quot;</span><span class="hl std">,steps_mean ) )</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] &quot;Mean of total number of steps taken per day after imputing missing values: 10749.7704918033&quot;
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl std">steps_median</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">median</span><span class="hl std">( plot_data</span><span class="hl opt">$</span><span class="hl std">steps_sum,</span><span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span> <span class="hl std">)</span>
<span class="hl kwd">print</span><span class="hl std">(</span> <span class="hl kwd">paste</span><span class="hl std">(</span><span class="hl str">&quot;Median of total number of steps taken per day after imputing missing values:&quot;</span><span class="hl std">,steps_median ) )</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] &quot;Median of total number of steps taken per day after imputing missing values: 10641&quot;
</pre></div>
</div></div>
These values are different from the data from the first part of the assignment. The imputing missing data gives us a more smooth dataset of the total daily number of steps.


### Difference in activity patterns between weekdays and weekends

I use the dataset with the filled-in missing values for this part.

1. Creating a new factor variable in the dataset with two levels - “weekday” and “weekend” indicating whether a given date is a weekday or weekend day:
<div class="chunk" id="unnamed-chunk-14"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">activity_f</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">mutate</span><span class="hl std">( activity_f,</span> <span class="hl kwc">day</span> <span class="hl std">=</span> <span class="hl kwd">weekdays</span><span class="hl std">(</span> <span class="hl kwd">as.Date</span><span class="hl std">(date) ) )</span>

<span class="hl std">activity_f</span> <span class="hl kwb">&lt;-</span> <span class="hl std">activity_f</span> <span class="hl opt">%&gt;%</span>
    <span class="hl kwd">mutate</span><span class="hl std">(</span> <span class="hl kwc">weekend</span> <span class="hl std">=</span> <span class="hl kwd">if_else</span><span class="hl std">( ( (day</span> <span class="hl opt">==</span> <span class="hl str">&quot;Sunday&quot;</span><span class="hl std">)</span> <span class="hl opt">|</span> <span class="hl std">(day</span> <span class="hl opt">==</span> <span class="hl str">&quot;Saturday&quot;</span><span class="hl std">) ),</span> <span class="hl str">&quot;weekend&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;weekday&quot;</span><span class="hl std">) )</span>

<span class="hl std">weekday_data</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">subset</span><span class="hl std">( activity_f, weekend</span> <span class="hl opt">==</span> <span class="hl str">&quot;weekday&quot;</span> <span class="hl std">)</span>
<span class="hl std">weekend_data</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">subset</span><span class="hl std">( activity_f, weekend</span> <span class="hl opt">==</span> <span class="hl str">&quot;weekend&quot;</span> <span class="hl std">)</span>

<span class="hl std">weekday_data</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">group_by</span><span class="hl std">( weekday_data, interval )</span> <span class="hl opt">%&gt;%</span>
    <span class="hl kwd">summarize</span><span class="hl std">(</span> <span class="hl kwc">steps_sum</span>    <span class="hl std">=</span>    <span class="hl kwd">mean</span><span class="hl std">(steps,</span> <span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">))</span>

<span class="hl std">weekend_data</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">group_by</span><span class="hl std">( weekend_data, interval )</span> <span class="hl opt">%&gt;%</span>
    <span class="hl kwd">summarize</span><span class="hl std">(</span> <span class="hl kwc">steps_sum</span>    <span class="hl std">=</span>    <span class="hl kwd">mean</span><span class="hl std">(steps,</span> <span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">))</span>

<span class="hl kwd">names</span><span class="hl std">(weekday_data)</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;interval&quot;</span><span class="hl std">,</span><span class="hl str">&quot;weekday&quot;</span><span class="hl std">)</span>
<span class="hl kwd">names</span><span class="hl std">(weekend_data)</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;interval&quot;</span><span class="hl std">,</span><span class="hl str">&quot;weekend&quot;</span><span class="hl std">)</span>

<span class="hl std">temp_data</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">merge</span><span class="hl std">( weekday_data,weekend_data,</span><span class="hl kwc">by</span><span class="hl std">=</span><span class="hl str">&quot;interval&quot;</span> <span class="hl std">)</span>

<span class="hl kwd">library</span><span class="hl std">(reshape2)</span>

<span class="hl std">plot_data</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">melt</span><span class="hl std">(temp_data,</span><span class="hl kwc">id</span><span class="hl std">=</span><span class="hl str">&quot;interval&quot;</span><span class="hl std">,</span><span class="hl kwc">measure.vars</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;weekday&quot;</span><span class="hl std">,</span><span class="hl str">&quot;weekend&quot;</span><span class="hl std">))</span>
</pre></div>
</div></div>
2. Making a panel plot containing a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis):
<div class="chunk" id="unnamed-chunk-15"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(lattice)</span>
<span class="hl com">#png( file=&quot;plot_sum_week_na.png&quot;,width = 480,height = 480 )</span>
<span class="hl kwd">xyplot</span><span class="hl std">(value</span> <span class="hl opt">~</span> <span class="hl std">interval</span> <span class="hl opt">|</span> <span class="hl std">variable,</span> <span class="hl kwc">data</span> <span class="hl std">= plot_data,</span> <span class="hl kwc">layout</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">1</span><span class="hl std">,</span><span class="hl num">2</span><span class="hl std">),</span><span class="hl kwc">type</span><span class="hl std">=</span><span class="hl str">&quot;l&quot;</span><span class="hl std">,</span><span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;Interval&quot;</span><span class="hl std">,</span><span class="hl kwc">ylab</span><span class="hl std">=</span><span class="hl str">&quot;Number of steps&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-15-1.png" title="plot of chunk unnamed-chunk-15" alt="plot of chunk unnamed-chunk-15" class="plot" /></div>
<div class="source"><pre class="knitr r"><span class="hl com">#dev.off()</span>
</pre></div>
</div></div>
