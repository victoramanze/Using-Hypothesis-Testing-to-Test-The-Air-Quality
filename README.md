# Using-Hypothesis-Testing-to-Test-The-Air-Quality
You work for an environmental think tank called Repair Our Air (ROA). ROA is formulating policy recommendations to improve the air quality in America, using the Environmental Protection Agency’s Air Quality Index (AQI) to guide their decision making. An AQI value close to 0 signals “little to no” public health concern, while higher values are associated with increased risk to public health.

They’ve tasked you with leveraging AQI data to help them prioritize their strategy for improving air quality in America.

ROA is considering the following decisions. For each, construct a hypothesis test and an accompanying visualization, using your results of that test to make a recommendation:

import pandas as pd
import numpy as np
from scipy import stats
pd.read_csv("c4 epa air dataset.csv")

ads = pd.read_csv("c4 epa air dataset.csv")
show a sample of data “Use describe() to summarize AQI”For a more thorough examination of observations by state use values_counts()”)

print(ads.head())
print(ads.describe(include='all'))
print(ads['state_name'].value_counts())



From data exploration above, what do you recognize?¶ You have county-level data for the first hypothesis. Ohio and New York both have a higher number of observations to work with in this dataset.

Before you proceed, recall the following steps for conducting hypothesis testing:

Formulate the null hypothesis and the alternative hypothesis. Set the significance level. Determine the appropriate test procedure. Compute the p-value. Draw your conclusion.

HYPOTHESIS 1
ROA is considering a metropolitan-focused approach. Within California, they want to know if the mean AQI in Los Angeles County is statistically different from the rest of California

ca_la = ads[ads['county_name']=='Los Angeles']
ca_other = ads[(ads['state_name']=='California') & (ads['county_name']!='Los Angeles')]
Formulate your null and alternative hypotheses:

𝐻0 : There is no difference in the mean AQI between Los Angeles County and the rest of California. 𝐻𝐴 : There is a difference in the mean AQI between Los Angeles County and the rest of California.

we will use 95% as the significant level

significance_level = 0.95
significance_level

Determine the appropriate test procedure: Here, you are comparing the sample means between two independent samples. Therefore, you will utilize a two-sample 𝑡-test.

Compute the P-value

stats.ttest_ind(a=ca_la['aqi'], b=ca_other['aqi'], equal_var=False)

Question 2. What is your P-value for hypothesis 1, and what does this indicate for your null hypothesis? With a p-value (0.049) being less than 0.05 (as your confidence level is 95%), reject the null hypothesis in favor of the alternative hypothesis.

Therefore, a metropolitan strategy may make sense in this case.

Hypothesis 2:
With limited resources, ROA has to choose between New York and Ohio for their next regional office. Does New York have a lower AQI than Ohio?¶

ny = ads[ads['state_name']=='New York']
ohio = ads[ads['state_name']=='Ohio']
Formulate your hypothesis: Formulate your null and alternative hypotheses:

𝐻0 : The mean AQI of New York is greater than or equal to that of Ohio. 𝐻𝐴 : The mean AQI of New York is below that of Ohio.

significance level = 95%

Determine the appropriate test procedure: Here, you are comparing the sample means between two independent samples in one direction. Therefore, you will utilize a two-sample 𝑡-test.

Compute the P-value¶

tstat, pvalue = stats.ttest_ind(a=ny['aqi'], b=ohio['aqi'], alternative='less')
print(tstat)
print(pvalue)

Question 3.
What is your P-value for hypothesis 2, and what does this indicate for your null hypothesis? With a p-value (0.030) being less than 0.05 (as your confidence level is 95%) and a t-statistic < 0 (-2.02), reject the null hypothesis in favor of the alternative hypothesis.

Therefore, you can conclude with 95% confidence that New York has a lower mean AQI than Ohio.

Hypothesis 3:
A new policy will affect those states with a mean AQI of 10 or greater. Can you rule out Michigan from being affected by this new policy?

michigan = ads[ads['state_name']=='Michigan']
Formulate your hypothesis: Formulate your null and alternative hypotheses here:

𝐻0 : The mean AQI of Michigan is less than or equal to 10. 𝐻𝐴 : The mean AQI of Michigan is greater than 10.

significance level remains at 95%

Determine the appropriate test procedure: Here, you are comparing one sample mean relative to a particular value in one direction. Therefore, you will utilize a one-sample 𝑡-test.

tstat, pvalue = stats.ttest_1samp(michigan['aqi'], 10, alternative='greater')
print(tstat)
print(pvalue)

Question 4.
What is your P-value for hypothesis 3, and what does this indicate for your null hypothesis? With a p-value (0.060) being greater than 0.05 (as your confidence level is 95%) and a t-statistic < 0 (-1.73), fail to reject the null hypothesis.

Therefore, you cannot conclude with 95% confidence that Michigan’s mean AQI is below 10.

Step 4. Results and Evaluation Now that you’ve completed your statistical tests, you can consider your hypotheses and the results you gathered.

Question 5.
Did your results show that the AQI in Los Angeles County was statistically different from the rest of California? Yes, the results indicated that the AQI in Los Angeles County was in fact different from the rest of California.

Question 6.
Did New York or Ohio have a lower AQI? Using a 95% significance level, you can conclude that New York has a lower AQI than Ohio based on the results.

Question 7:
Will Michigan be affected by the new policy impacting states with a mean AQI of 10 or greater? Based on the tests, you would fail to reject the null hypothesis, meaning you can’t conclude that the mean AQI is below 10. Thus, it is likely that Michigan would be affected by the new policy.

Conclusion
What are key takeaways from this lab?

Even with small sample sizes, the variation within the data is enough to allow you to make statistically significant conclusions. You identified with 95% confidence that the Los Angeles mean AQI was stastitically different from the rest of California, and that New York does have a lower mean AQI than Ohio. However, you were unable to conclude with 95% confidence that Michigan’s mean AQI was below 10.

What would you consider presenting to your manager as part of your findings?
For each test, you would present the null and alternative hypothesis, then describe your conclusion and the resulting p-value that drove that conclusion. As the setup of t-test’s have a few key configurations that dictate how you interpret the result, you would specify the type of test you chose, whether that tail was one-tail or two-tailed, and how you performed the t-test from stats.

What would you convey to external stakeholders?
In answer to the research questions posed, you would convey the degree of confidence (95%) and your conclusion. Additionally, providing the sample statistics being compared in each case will likely provide important context for stakeholders to quickly understand the difference between your results.

DATA SCIENCE . HYPOTHESIS TESTING. STATISTICS
