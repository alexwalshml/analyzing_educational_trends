# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 1: Civic data analysis

## Problem Statement

The Mayor of Naperville would like to improve the quality of education that the city's school offers. They would like to know how they currently compare to the rest of the nation, and what can be done to improve their metrics. The ACT and SAT are two standardized national tests used to indicate a student's performance. I will attempt to identify correlating factors to high ACT/SAT scores to give the Mayor an idea of how to shape education policy. Additionally, it is widely believed that a school's performance is related to its funding. Because simply giving the schools more money may not be an option to the Mayor, I will try to draw relationships between test scores and income, as increasing the potency of the local economy may both indirectly increase school performance *and* sit well with the voters.

## Datasets

* act_\<year>.csv: ACT data by state for years 2017-2022. Years 2017-2019 provided. ([*2020 source*](https://www.act.org/content/dam/act/unsecured/documents/2020/2020-Average-ACT-Scores-by-State.pdf)), ([*2021 source*](https://www.act.org/content/dam/act/unsecured/documents/2021/2021-Average-ACT-Scores-by-State.pdf)), ([*2022 source*](https://www.act.org/content/dam/act/unsecured/documents/2022/2022-Average-ACT-Scores-by-State.pdf))
* sat_\<year>.csv: SAT data by state for years 2017-2022. Years 2017-2019 provided. ([*2020 source*](https://ipsr.ku.edu/ksdata/ksah/education/6ed16.pdf)), ([*2021 source*](https://www.number2.com/average-sat-score/)), ([*2022 source*](https://blog.prepscholar.com/average-sat-scores-by-state-most-recent))
* us_income.csv: Total income, income per capita, and population by state and region for years 1929-2021. ([*source*](https://apps.bea.gov/iTable/?reqid=70&step=1&acrdn=2#eyJhcHBpZCI6NzAsInN0ZXBzIjpbMSwyNCwyOSwyNSwzMSwyNiwyNywzMF0sImRhdGEiOltbIlRhYmxlSWQiLCI2MDAiXSxbIkNsYXNzaWZpY2F0aW9uIiwiTm9uLUluZHVzdHJ5Il0sWyJNYWpvcl9BcmVhIiwiMCJdLFsiU3RhdGUiLFsiMCJdXSxbIkFyZWEiLFsiMDAwMDAiXV0sWyJTdGF0aXN0aWMiLFsiLTEiXV0sWyJVbml0X29mX21lYXN1cmUiLCJMZXZlbHMiXSxbIlllYXIiLFsiLTEiXV0sWyJZZWFyQmVnaW4iLCItMSJdLFsiWWVhcl9FbmQiLCItMSJdXX0=))
* naperville.csv: SAT data for the two high schools present in Naperville, years 2019, 2021, and 2022. Compiled from several documents. ([*source*](https://www.isbe.net/ilreportcarddata))

## Data Dictionaries

### Main Dataframe (full_df)
This dataframe contains all data from the ACT, SAT, and income `.csv` files after it has been cleaned and formatted. It contains data for all 50 US states, plus the District of Columbia and a national weighted average. The bulk of the analysis will be on data contained in this dataframe. This dataframe is stored in `full_df`, and has a variant called `no_national_df` which is identical sans the national point.

|Feature|Type|Dataset|Description|
|:---|:---|:---|:---|
|**state**|*object*|ACT, SAT, Income|The US state the test results are associated with, including District of Columbia.|
|**act_2017_english**|*float*|act_2017|The average score of all students on the English portion of the ACT in a given state in 2017. Individual scores range 1-36, rounded to the nearest integer.|
|**act_2017_math**|*float*|act_2017|The average score of all students on the math portion of the ACT in a given state in 2017. Individual scores range 1-36, rounded to the nearest integer.|
|**act_2017_reading**|*float*|act_2017|The average score of all students on the reading portion of the ACT in a given state in 2017. Individual scores range 1-36, rounded to the nearest integer.|
|**act_2017_science**|*float*|act_2017|The average score of all students on the science portion of the ACT in a given state in 2017. Individual scores range 1-36, rounded to the nearest integer.|
|**act_20XX_participation**|*float*|ACT|The fraction of students who participated in the ACT. Years 2017-2022.|
|**act_20XX_composite**|*float*|ACT|The average composite score of all students, taken as the average of their scores on the four subject tests. Individual scores range 1-36, rounded to the nearest integer. Years 2017-2022.|
|**sat_20XX_participation**|*float*|SAT|The fraction of students who participated in the SAT. Years 2017-2022.|
|**sat_20XX_ebrw**|*int*|SAT|The average score of all students on the evidence-based reading and writing portion of the SAT. Individual scores range 200-800, rounded to the nearest integer. Years 2017-2022.|
|**sat_20XX_math**|*int*|SAT|The average score of all students on the math portion of the SAT. Individual scores range 200-800, rounded to the nearest integer. Years 2017-2022.|
|**sat_20XX_total**|*int*|SAT|The average combined score of all students, calculated as the sum of EBRW and math scores. Individual scores range 400-1600, rounded to the nearest integer. Years 2017-2022.|
|**income_20YY**|*float*|Income|Income per capita, defined as total income for the state divided by its population. Years 2017-2021.|
|**income_2022_projected**|*float*|None|Estimated income per capita for the year 2022.|
|**population_20YY**|*int*|Income|Number of people living in each US state. Note: The "National" population here refers to average population per state, not total population. Years 2017-2021.|
|**population_2022_projected**|*int*|None|Estimated population for the year 2022.|

### Descriptive Satistics Dataframe (descriptive_df)
This dataframe contains descriptive statistics for all of the features contained in `full_df`.

|Feature|Type|Description|
|:---|:---|:---|
|**feature**|*object*|The feature from `full_df` to which the descriptive statistics apply (numeric features only).|
|**mean**|*float*|The unweighted average of the feature.|
|**median**|*float*|The median of the feature.|
|**std**|*float*|The standard deviation of the feature.|
|**minimum**|*float*|The minimum value of the feature.|
|**minimum_states**|*float*|The states which correspond to the minimum value of the feature.|
|**maximum**|*float*|The maximum value of the feature.|
|**maximum_states**|*float*|The states which correspond to the minimum value of the feature.|
|**25%**|*float*|The value of the 25th percentile, which is the value for which 25% of the feature points have a value less than or equal to this number.|
|**50%**|*int*|The value of the 50th percentile.|
|**75%**|*int*|The value of the 75th percentile.|

### Local Dataframe (naperville_df)
This dataframe contains values relevant to the analysis for my local city, Naperville. This dataframe is small, as most publicly available data for my city was not related to any other quantities present.

|Feature|Type|Dataset|Description|
|:---|:---|:---|:---|
|**year**|*int*|Naperville|The year the exam was taken. Values 2019, 2021, and 2022.|
|**sat_participation**|*float*|Naperville|The fraction of students who participated in the SAT.|
|**sat_math**|*int*|Naperville|The average score of all students on the math portion of the SAT.|
|**sat_ebrw**|*int*|Naperville|The average score of all students on the evidence-based reading and writing portion of the SAT.|
|**sat_total**|*int*|Naperville|The average combined score of all students.|

## Analysis

This analysis consisted of two major portions. The first was to examine how well the city of Naperville is performing compared to the rest of the nation. The second was to attempt to find a relationshipe between standardized test performance and local income.

<p align="center">
  <img width="600" height="500" src="https://git.generalassemb.ly/alexwalshml/project_1/blob/main/readme_img/act_yearly.png">
</p>

Naperville did not have publicly available ACT data, but the trend is still worth examining. The national average score is decreasing over time, but Illinois is largely improving in this metric. Additionally, Illinois is one of the better perfroming states for average composite ACT scores. We now contrast this with the SAT data:

<p align="center">
  <img width="600" height="500" src="https://git.generalassemb.ly/alexwalshml/project_1/blob/main/readme_img/sat_yearly.png">
</p>

For average SAT total, the national average is once again decreasing, as is the Illinois average. This time, Illinois scores are below the national average, indicating that students on average are much worse at the SAT than the ACT in the state. The city of Naperville, on the other hand, was far above the national average for SAT scores, but it too was worsening over time.

<p align="center">
  <img width="600" height="500" src="https://git.generalassemb.ly/alexwalshml/project_1/blob/main/readme_img/act_trends.png">
</p>

The analysis revealed that higher rates of testing were associated with lower scores. As such, any other relationships would also reflect this fact if it were not taken into account. By grouping together states that have similar participation rates for the ACT, we see that higher income per capita is associated with higher ACT scores.

<p align="center">
  <img width="600" height="500" src="https://git.generalassemb.ly/alexwalshml/project_1/blob/main/readme_img/sat_trends.png">
</p>

The same is true of SAT scores, which without grouping would appear to have the opposite trend. For both ACT and SAT data, higher local wealth appears to be beneficial for student performance.

## Conclusions and Recommendations

As a nation, students are performing worse on both the ACT and the SAT by the year. In the state of Illinois specifically, ACT scores are improving, but the more relevant SAT scores are worsening. The city of Naperville is doing well in comparison, by some metrics. Our SAT scores are considerably higher than the national average, but they too are decreasing. There is evidence in the data to show that mandated testing leads to worse performance on that same test, and in the state of Illinois this is shown by relatively high ACT scores, and very low SAT scores. Furthermore, increases in local wealth are correlated with better test scores. In light of this evidence, I propose three courses of action:

1. Switch back to the ACT as the primary test for Naperville. The ACT seems to show a strong relationship between wealth and test score, and as a wealthy city within a wealthy state, if this relationship is causal our students would likely see a great increase in their test scores. This, if nothing else, would improve their odds at a post-secondary education, which is another metric of success this study did not examine.

2. Stop mandating standardized tests. Higher rates of participation are associated with lower test scores. This study was not able to determine a reason why this is, but if there are other ways to measure a school's success they should be utilized more.

3. Bolster the local economy. Schools perform better with additional funding, but if that funding isn't available a similar result can be achieved by increasing the average wealth of the locals. This can be accomplished in a number of ways, such as increasing the minimum wage, or by attracting new businesses to the city. The effect of this would be two-fold: the schools could be given for funding from the increase in collected taxes, and students would have a higher quality of life and thus would perform better.

One and two are not mutually exclusive options, as the city of Naperville can recommend students take the ACT over the SAT without requiring it. I believe that any and all of these options have potential to improve student performance if implemented. Most of all, I strongly recommend a secondary analysis featuring more insight into national and state-level education policies, the effects of the COVID-19 pandemic on education, and more metrics of student success besides standardized tests.

## Other Sources

1. https://www.chicagotribune.com/news/ct-illinois-chooses-sat-met-20160211-story.html
2. https://www.edweek.org/policy-politics/student-outcomes-does-more-money-really-matter/2019/06
3. https://bmcmededuc.biomedcentral.com/articles/10.1186/s12909-015-0476-1
4. https://collegereadiness.collegeboard.org/sat
5. https://www.act.org/content/act/en.html
