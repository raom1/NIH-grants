# How is Biomedical Research Diversity Affected by Funding?
This project studies data from the National Institutes of Health. The NIH is one of the main government organizations that fund research in academic research institutions. 

## Why should you care?
Much of what we consume on a regular basis is in some way the product of scientific research. Either the computer you’re reading this on or the allergy medicine you took this morning, we rely on research to help propel our society forward. More poignant examples could include new technologies like CRISPR or Immunotherapy to treat devastating diseases or a deeper understanding of stem cells or viruses to treat multiple sclerosis or food allergies. Considering how much we all rely on scientific research, it would be in everyone’s best interest to have research programs that are broad and deep to give the best chance of finding the next breakthrough discovery. 

Unsurprisingly, one of the biggest limitations to research is having money to do it. Focusing just on the NIH, if appropriations for it are lower then there is less money to fund research. In that scenario it is possible that certain research areas may be ignored in favor of others. These limitations on research could greatly slow innovation and have long lasting effects. 

__Big Picture Question:__ How is research diversity affected by changes in funding to the NIH?

## How does research get funded by the NIH?
_This is a very simplified explanation of how grants are funded. For a more complete explanation read <a href="https://grants.nih.gov/grants/referral-and-review.htm" target="_blank">this</a>._

Money can be awarded to individuals and/or institutions through many different types of grants. The most common type of grant is called an R01 for short. It is typically awarded to a single investigator to fund one research project and lasts for 5 years. For this analysis we will focus on R01s only, since they are the lifeblood of academic research and the most consistent type of grant. 

When an investigator wants money to start or continue a research project in their lab they submit an R01 application to an institute within the NIH. Many others also submit applications for projects they have in their labs. All the grants are collected and reviewed by a panel of other investigators in the field. All the grants are scored and then ranked from best to worst. The NIH then looks at how much money is available and determines how many grants can be funded. Based on that they draw a funding line on the ranked list of grants to determine which grants will be funded and which will not. Therefore, when there is more money available, more grants can be funded, which allows for the possibility for more diverse research topics 

## Methodology
### Data Acquisition
Data on annual appropriations, success rate, and information on funded grants since 1985 or earlier was gathered from the NIH website. Information on annual inflation was also collected from the <a href="https://data.bls.gov/timeseries/CUUR0000SA0L1E?output_view=pct_12mths" target="_blank">Bureau of Labor Statistics</a>. Data were scraped or downloaded. Only information for R01s was retained.

### Cleaning
Research topics were determined using submitted Abstracts. Empty abstracts were filled with appropriate vales or the grant was removed. A few grants had abstracts that were improperly formatted that interfered with downstream analysis. These were identified and removed. Extra characters, labels, or comments were removed.

### Topic classification
Topics were determined using assignments provided by the NIH. For each grant the NIH assigns categories in which the grant belongs. The methodology for assigning categories has changed over time. Originally, they were manually assigned from a list of curated categories. After 2007(?) categories were assigned using synonyms based on the grant title. Category assignments were inconsistent so classifiers were trained for each category. Unique abstracts that had categories assigned were collected and separated into each category. Some abstracts had multiple categories assigned so were present in multiple groups. The groups were not balanced so thresholds were applied to each category to reduce the amount of imbalance. Categories needed at least 100 associated abstracts to be included, which resulted in 180 topics. Subsequently, a maximum of 500 positive examples for each category were combined with negative examples (from the remaining abstracts with categories assigned). The number of positive and negative examples totaled 1000. These were used to train logistic regression models for each topic. The models were then fit to the entire list of abstracts and the probability of inclusion in the topic was calculated. The topic with the highest probability of inclusion was assigned to the abstract, resulting in a single topic label for each abstract.

### Topic clustering
Once topics were assigned to each abstract for each year the relative frequency over time could be calculated. This was done by counting the number of abstracts in a particular category divided by the total number of abstracts for each year. These values were then used to compute a similarity matrix, using Euclidian similarity. The similarity matrix was then used to perform agglomerative clustering to group topics with similar frequencies over time. Once clustered, topics following a particular trend were able to be identified. This was used to identify topics that increased or decreased in relative frequency with changes in funding.

## Findings (in a nut shell)
When the budget for the NIH since 1985 is compared to the budget in 1985 adjusted for inflation each year it’s easy to see that actual funding has not kept pace with inflation. This means that the money allocated to the NIH has effectively decreased since 1985.

![inflation](/images/inflation.png)

The decrease in buying power of the NIH budget correlates with a decrease in success rate of R01s since 1985. Another interesting trend is the budget before and after 2003. Before 2003, the budget grew at a much faster rate than after 2003. When looking at the trend in success rate before and after 2003 we see an interesting correlation. The success decreased at a faster rate after 2003 than before 2003.

![success_rate](/images/success_rate.png)

When the number of topics represented over time is compared to budget we can see that after 2003 the number of topics remained unchanged while there was a general increase in number of topics before 2003.

![topics](/images/budget_number-of-topics.png)

2003 provides an interesting division point. To assess which topics were most affected by any changes that occurred in 2003, the abstracts before and after 2003 were grouped and fold change was calculated between the two time points. When the top 5 increased and decreased categories were displayed we could see that the distinct difference in the relative frequency pattern. 

![top_bottom_fc](/images/top_bottom_fc.png)

This indicated there may be discrete groups of clusters based on changes in relative frequency over time. Categories were clustered based on similarity and clusters that followed trends of increase or decrease after 2003 were selected. This indicates that certain research topics may be negatively or positively affected by constrictions in funding to the NIH

![cluster_11](/images/example_cluster_11.png)
![cluster_23](/images/example_cluster_23.png)

## Caveats

The biggest drawback to this analysis is that no information is available on abstracts that were not funded. The main comparison is over time but including comparisons with unfunded projects would greatly improve the analysis.

The number of topics had a maximum value, 180. Though all the topics were not detected in one year there may have been some affect of saturation. Developing a way to scale the number of topics each year would also benefit the analysis.

Lastly, only one factor was considered: Funding. Many other factors could have influenced changes in relative frequency of research topic, such as advancements in technology, political ideology of congress, alternative funding sources, and many more. Considering multiple factors would again improve the analysis.

## Conclusions, Applications, and Future Directions

Overall there seems to be a correlation between research topic diversity and funding to the NIH. When subdivided into individual categories it seems that changes in funding have mixed effects, with some increasing in frequency and other decreasing. 

The most direct application of this analysis could be to investigators and research institutes. By understanding what topics are affected by funding constrictions institutions can better prepare and individual investigators can modify their research programs. Researchers could also use this information prospectively for planning on when to submit grants or which topics to pursue.

Organizations or companies that are interested in research coming from NIH funded institutions can better prepare for new research findings. This can inform where and how to invest resources.

In the future refining the topic identification procedure as well as a more critical investigation of the correlation between funding and research topic diversity would greatly benefit the current analysis. Beyond this, incorporating additional factors such as new technologies, assessing correlation with factors such as the institution receiving the grant, or more detailed information on topics, such as how established the field is, would be useful. Expanding the analysis to studying other funding agencies, such as the National Science Foundation or the American Heart Association, and research done at pharmaceutical or biotech companies would help to contextualize the current findings.

_This project was done as a final capstone project for the first Data Science Bootcamp at Nashville Software School._
