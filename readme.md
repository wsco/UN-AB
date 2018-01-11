# Design And Analyze A/B Testing
#### Wesley Scoggin - January 2018

### Experiment Design
#### Metric Choice
>List which metrics you will use as invariant metrics and evaluation metrics here. (These should be the same metrics you chose in the "Choosing Invariant Metrics" and "Choosing Evaluation Metrics" quizzes.)

##### Invariant Metrics:
+ Number of Cookies
+ Number of Clicks
##### Evaluation metrics
+ Gross Conversion
+ Net Conversion

>For each metric, explain both why you did or did not use it as an invariant metric and why you did or did not use it as an evaluation metric. Also, state what results you will look for in your evaluation metrics in order to launch the experiment.

**Number of Cookies:** The number of unique cookies issued indicates unique visitors to the course overview page. This metric is chosen as one of the invariant metrics since cookies are issued during the page load before users are exposed to experimental factors and is consequentially independent of those factors.

**Number of User-Id's** Number of users that enroll in a free trial. This would not be the same for a control group and experimental group so cannot be used as an evaluation metric. It is also dependent on the experimental factors lead to enrollment in the free trial so this metric is also ruled out as an invariant metric.

**Number of Clicks:** Number of users indicated as unique from cookie data that click 'start free trial' button. This metric is also collected before users are exposed to experimental factors and will also be used for its independence from those factors.

**Click-through probability:** The number of unique cookies to click "Start Free Trial" button divided by the number of unique cookies to view the course overview page. If this were the only metric available it may suffice as an invariant metric due to the fact that it is calculated with values collected before experimental factors are in play. Since the metrics that this calculation is based on are also available this is somewhat redundant.

**Gross Conversion:** The number of user-id's to complete a checkout and enroll in the free trial divided by number of unique cookies to click the 'Start Free Trial' button. This has been selected as a valid evaluation metric since it is a dependent on effects of the experiment directly in ratio to a selected Invariant metric. This metric should describe success or failure of experimental factors.

**Retention:** The number of user-id's to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of user-ids to complete a checkout. The experimental factors in this A/B test will directly contribute to a number of control group cancelations since it involves informing only the experimental users of required time to complete the course.

**Net Conversion:** The number of user-id's to remain enrolled past the 14-day boundary (and make at least one payment) divided by the number of unique cookies to click the 'Start free Trial' button. Again this metric will directly measure the difference between informing users in the experimental group of the time commitment minimums required to finish the course while control group participants will not have that information making this a viable choice for an evaluation metric.

Expectations during experimentation of implementing the new screening information will be observing gross conversion decreases with practical significance indicating lowering costs, while maintaining net conversions with statistical significance.

### Measuring Standard Deviation
>List the standard deviation of each of your evaluation metrics. (These should be the answers from the "Calculating standard deviation" quiz.)

The values to calculate standard deviation are found in the Base Table provided with this course.

|               | Value |
| ------------- | ------------- |
| Unique cookies to view page per day | 40000 |
| Unique cookies to click "Start free trial" per day | 3200 |
| Enrollments per day | 660 |
| Click-through-probability on "Start free trial"       | 0.08 |
| Probability of enrolling, given click     | 0.20625  |
| Probability of payment, given enroll  | 0.53    |
| Probability of payment, given click  | 0.1093125 |

* **Gross Conversion**
    * standard deviation with 3200 page clicks and 40,000 page views is sqrt( .20625*(1 - .20625 )/3200) = **.0072**
    * and scaled down to a 5,000 cookie sample the new standard error is 0.0072*sqrt( 40,000 / 5,000) = **0.0202**

* **Net Conversion**
    * standard deviation at sample of 5,000 based on 40,000 page views per day is:
    * sqrt( ( 0.1093125 * ( 1 - 0.1093125 ) / 3200 ) * sqrt( 40,000 / 5,000 ) = **.0156**


>For each of your evaluation metrics, indicate whether you think the analytic estimate would be comparable to the the empirical variability, or whether you expect them to be different (in which case it might be worth doing an empirical estimate if there is time). Briefly give your reasoning in each case.

The unit of diversion in this case between control groups and experimental groups is number of unique cookies, 3200. Both Gross conversion and net conversion's denominators share this as their denominator. Empirical variability in this case can be considered comparable to the analytic estimate since the unit of analysis and unit of diversion are equal.

### Sizing
#### Number of Samples vs. Power
>Indicate whether you will use the Bonferroni correction during your analysis phase, and give the number of pageviews you will need to power you experiment appropriately. (These should be the answers from the "Calculating Number of Pageviews" quiz.)

Gross conversion and Net conversion are highly correlated, meaning Bonferroni correction will not be used since it is too conservative to use with metrics with a relationship this strong.

With the use of [Evan Millers Sample size calculator](http://www.evanmiller.org/ab-testing/sample-size.html) setting alpha to 0.05 and beta to 0.2, for Probability of Payment (10.93125% base conversion rate and .75 minimum detectable effect) requires a higher sample rate of 27,413 while 'Probability of Enrolling' (20.625% base conversion rate at 1% minimum detectable effect) only would require 25,835 samples. So to test both page views is calculated to be 27,413 /.08 (from percent of pageviews that lead to a click) = 342662.5 for each group. When doubled the total number required becomes **685325**.

#### Duration vs. Exposure
>Indicate what fraction of traffic you would divert to this experiment and, given this, how many days you would need to run the experiment. (These should be the answers from the "Choosing Duration and Exposure" quiz.)

At 40,000 page views per day dedicating 85% of traffic to the experiment would divert 34,000. At that rate, the experiment could be completed in 21 days.

>Give your reasoning for the fraction you chose to divert. How risky do you think this experiment would be for Udacity?

For an experiment like this, I would argue that the risks are fairly low. Users that have already paid are unlikely to affected. This high of a diversion rate would return results in a much faster time than only diverting half or less of the traffic allowing for quicker adoption if the results of a change prove to be beneficial

### Experiment Analysis
#### Sanity Checks
>For each of your invariant metrics, give the 95% confidence interval for the value you expect to observe, the actual observed value, and whether the metric passes your sanity check. (These should be the answers from the "Sanity Checks" quiz.)

**Number of Cookies**

| Control Group Total | Experiment Group Total | SE | Margin Error | Lower | Upper |
|---|---|---|---|---|---|
| | | sqrt(.5*.5/(345543+344660)) | 1.96*.0006018 | 0.5-0.0011797 | 0.5 +0.0011797 |
| 345543 | 344660 | 0.0006018 | 0.0011797 | 0.4988 | 0.5012 |

Observed value of 345543/(345543+344660) = **0.5006** which is between the interval [0.4988,0.5012] **Pass Sanity Check**

**Number of Clicks**

| Control Group Total | Experiment Group Total | SE | Margin Error | Lower | Upper |
|---|---|---|---|---|---|
| | | sqrt(.5*.5/(28378+28325)) | 1.96*.0.0041 | 0.5-0.0041 | 0.5 +0.0041 |
| 28378 | 28325 | 0.0021 | 0.0041 | 0.4959 | 0.5041 |

Observed value of 28378/(28378+28325) = **0.5005** which is between the interval [0.4959,0.5041] **Pass Sanity Check**

### Result Analysis
#### Effect Size Tests
>For each of your evaluation metrics, give a 95% confidence interval around the difference between the experiment and control groups. Indicate whether each metric is statistically and practically significant. (These should be the answers from the "Effect Size Tests" quiz.)

**Gross Conversion**

|  | Control | Experiment |
|-|-|-|
| CLicks | 17293 | 17260 |
| Enrollment | 3785 | 3423 |
| Gross Conversion | 0.2189 | 0.1983 |
| SE <td colspan=2>0.004371
| M <td colspan=2>0.008568
| Pooled P <td colspan=2>0.2086
| d <td colspan=2>-0.02055
| CI <td colspan=2>[-0.0291,-0.0120]

The confidence interval does not span 0 or the dmin value so the result **is both practically and statistically significant** for Gross conversion.

**Net Conversion**

|  | Control | Experiment |
|-|-|-|
| CLicks | 17293 | 17260 |
| Enrollment | 2033 | 1945 |
| Gross Conversion | 0.1176 | 0.1127 |
| SE <td colspan=2>0.003434
| M <td colspan=2>0.0067
| Pooled P <td colspan=2>0.2086
| d <td colspan=2>-0.0049
| CI <td colspan=2>[-0.0116,-0.0018]

The confidence interval does span 0 and the dmin value so the result **is neither practically or statistically significant** for Gross conversion.

#### Sign Tests
>For each of your evaluation metrics, do a sign test using the day-by-day data, and report the p-value of the sign test and whether the result is statistically significant. (These should be the answers from the "Sign Tests" quiz.)

For **Gross Conversion** there is in increase in 4 out of the 23 trials at a probability of 0.5 the two tailed p value is 0.0026. This pvalue is less than the 0.05 alpha indicating this value is statistically significant.

For **Net Conversion** there is in increase in 10 out of the 23 trials at a probability of 0.5 the two tailed p value is 0.6776. This pvalue is greater than the 0.05 alpha indicating this value is not statistically significant.

#### Summary
>State whether you used the Bonferroni correction, and explain why or why not. If there are any discrepancies between the effect size hypothesis tests and the sign tests, describe the discrepancy and why you think it arose.

Since the Bonferroni correction is used to mitigate type 1 errors when using multiple metric comparisons that do not correlate with each other it was not used. With the sanity checks and sign tests confirming gross conversion is statistically and practically significant, while net conversion is not, the use of both metrics is justified. Bonferroni could be used if only one of these metrics were used.

### Recommendation
>Make a recommendation and briefly describe your reasoning.

The results may indicate that revenue may decrease if this change is adopted. While gross conversion did decrease for in the experimental group indicating less prepared students were not converted, the net conversion confidence interval included negative values. Without paid users increasing as an end result of warding off unprepared students in the gross conversion numbers I would not launch this change.


### Follow-Up Experiment
>Give a high-level description of the follow up experiment you would run, what your hypothesis would be, what metrics you would want to measure, what your unit of diversion would be, and your reasoning for these choices.

An extended trial period would be an interesting A/B test that may prove valuable for students learning completely new skills that they haven't been exposed to in any way before. There can be a steep learning curve if for instance, an new trial user has little to no experience with object oriented programming or scripting. In a two week trial, much of that time may be spent on becoming familiar with higher level concepts. With an extended trial period, a new user may have an a-ha moment where things start coming together in concepts that they are learning just after the two week mark, thereby increasing their likelihood to continue and ultimately pay for a Udacity subscription.

In this case the unit of diversion would remain user-id's, as in this experiment, unique users enrolled in the free trial have already created and started learning.

A valid Invariant Metric would be number of users, as this would not change between control and experimental groups.

Retention would serve well as the evaluation metric. Users that continue and make a payment to stay enrolled divided by the number of users that signed up for the free trial would prove if an an extended trial period would increase conversion to a paid from a free customer.

### Resources:
[Evan Millers Sample size calculator](http://www.evanmiller.org/ab-testing/sample-size.html)
