# US Tech Job Market Analysis

## Introduction

In recent years, the tech job market has experienced significant shifts, driven by a variety of economic and industry-specific factors. This analysis provides an in-depth look at the current state of the tech job market, highlighting trends in jobs and skills, the challenging environment for junior professionals, and emerging patterns in employment.

### Current Trends in the Tech Job Market

1. **High Layoff Rates**: The tech industry has witnessed a notable increase in layoffs, particularly since the onset of economic uncertainties and changing business priorities after the COVID-19 hiring boom. For instance, a 2024 report by [TechCrunch](https://shorturl.at/57ZTW) indicates that major tech companies have laid off tens of thousands of employees in the past year, with companies like Meta, Amazon, and Google leading the list. The [Layoffs.fyi](https://layoffs.fyi) tracker, which monitors layoffs across tech fields, shows a sharp rise in job cuts starting from 2023.

2. **Challenges for Junior Professionals**: The job market for entry-level and junior tech roles has become increasingly competitive. According to a [LinkedIn report](https://www.linkedin.com/business/talent/blog/talent-strategy/the-state-of-the-job-market-for-recent-grads), many junior roles are being consolidated or automated, leading to fewer opportunities for new graduates. Companies are seeking candidates with more experience and specialized skills, making it harder for juniors to break into the field.

3. **Emerging Job Trends**: Despite these challenges, certain areas within tech still continue to grow. Roles in cybersecurity, artificial intelligence (AI), and cloud computing are seeing increased demand. The [Gartner](https://shorturl.at/HG4UC) 2024 report highlights these sectors as critical areas for future job growth, driven by the ongoing digital transformation and heightened focus on security.

4. **Remote Work and Flexibility**: The remote work trend, which gained momentum during the COVID-19 pandemic, remains prevalent. Many tech companies are adopting hybrid or fully remote work models, offering greater flexibility to employees. This shift is reflected in the [Buffer's State of Remote Work](https://buffer.com/state-of-remote-work) report, which shows that remote work continues to be a major factor in job satisfaction and employee retention.

These resources offer a comprehensive view of the current job market landscape and can be useful for understanding the broader context of the data analyzed in this project.

## 1. What are the most demanded skills for the 3 most popular data jobs?

As a recent Data Science graduate looking for full-time roles, I was curious to find information about the most in-demand skills for popular data science jobs. To answer this question, I looked into the top 5 skills for the top 3 most popular data jobs in the US in 2023. This query highlights not only what the most popular data jobs are, but also shows me what skills I should highlight when applying to these roles.

View my notebook with detailed steps here:
[Skill_Demand.ipynb](notebooks/Skill_Demand.ipynb)

### Visualize Data 

```python
#Create plot
fig, ax = plt.subplots(len(job_titles), 1)
sns.set_style(style='ticks')

#Visualize plot
for i, job_title in enumerate(job_titles):
    df_plot = df_US_skills_perc[df_US_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count')
    #Label/Format plot
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].legend().remove()
    ax[i].set_xlim(0, 78)
    
    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{(v)}%', va='center')
        
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout()
plt.show()
```

### Results

![Visualization of Top Skills for Most Popular Data Jobs](images/skills_for_top_data_roles.png)

*Three separate bar charts showcasing the top 5 most in-demand skills for the top 3 most popular data jobs in the US in 2023*

### Insights

* The visualization indicates that Python and SQL are highly valued skills across Data Analyst, Data Engineer, and Data Scientist roles. Their prominence is particularly notable in Data Scientist and Data Engineer positions, reflecting the technical nature of these roles.

* Data visualization skills, especially with tools like Tableau, are critical for Data Analysts and Data Scientists. This emphasis is consistent with the need for these roles to effectively communicate data-driven insights to business stakeholders.

* For Data Engineers, technical skills related to cloud computing platforms and big data such as Azure and Spark are in high demand. This aligns with the responsibilities of Data Engineers, who are tasked with managing extensive data pipelines and integrating various storage systems.

## 2. How are skills trending for Data Scientists in the US?

As someone who is most interested in Data Scientist roles, I was curious to see how demand for popular skills trends over time. In terms of methodology, I needed to do the following:

1. Aggregate skill counts per month
2. Calculate skill percentage based on total of jobs
3. Plot the monthly skill demand

Once I've cleaned the data and visualized it on a plot, I'd be able to see how demand for popular skills trends over time!

View my notebook with detailed steps here:
[Skills_Trend.ipynb](notebooks/Skills_Trend.ipynb)

### Visualize Data

```python
#Get top 5 most popular skills
df_plot = df_DS_US_perc.iloc[:, 0:5]

#Create plot
sns.set_theme(style='ticks')
sns.lineplot(data=df_plot, dashes=False)
sns.despine()

#Label plot
plt.title('Trending Top Skills for Data Scientists in the US in 2023')
plt.ylabel('Liklihood in Job Posting')
plt.xlabel('Months (2023)')
plt.legend().remove()

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter())

for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i])

plt.show()
```

### Results

![Visualization of Demand Trend of Popular Data Scientist Skills in the US in 2023](images/demand_trend_of_ds_skills.png)

*Line chart visualizing the demand of top skills for Data Scientists in the US in 2023*

### Insights

* Out of all the skills, Python seems to remain the most popular throughout the year and stayed in steady demand.

* SQL and R follow closely after which highlights the importance of knowing a programming language for this role.

* It is also interesting to note that SQL had a significant increase in demand near the end of the year, which might suggest that more companies are moving away from older technologies like Excel, and shifting towards more modern data storage systems which require the need for Data Scientists to have experience with querying languages.

* Lastly, Tableau and SAS are in the lower demand of skills but remain at a steady rate. This indicates that these skills may only be important for certain industries/companies.

## 3. How well do Data jobs play in the US?

As someone early in their Data Science career, I was curious to see what roles within Data pay the most. This would give me an insight as to what roles are most in demand right now, what roles are growing in popularity, and what roles I might think about pivoting into.

View my notebook with detailed steps here:
[Salary_Analysis.ipynb](notebooks/Salary_Analysis.ipynb)

### Visualize Data

```python
#Create plot
sns.set_theme(style='ticks')

#Visualize plot
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order = job_order)

#Label plot
plt.title('Salary Distribution for Top 6 Data jobs in the US in 2023')
plt.xlabel('Yearly Salary ($USD)')
plt.ylabel('')
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)

plt.show()
```

### Results

![Visualization of Salary Range for Top 6 Data jobs in the US in 2023](images/salary_distribution_data_jobs.png)

*Salary Range for Top 6 Data jobs in the US in 2023*

### Insights

* From my visualization, I'm able to see that Data Scientists/Data Engineers get paid the most in the data field. It's also important to note that there is a significant variation in salary for Data Scientist/Data Engineer jobs with some salaries reaching $1 million! This goes to show that these jobs may vary in salary based on years of experience and level of technical skills.

* On the other hand, Data Analyst jobs are on the lower paying end of the spectrum relative to other Data jobs. However, their salary ranges are much more consistent, with fewer outliers. Typically this role needs less technical skills and thus may have a lower base pay but less variance in salary range.

* Lastly, Senior roles, especially Senior Data Scientists and Senior Data Engineers get paid the most in the industry with a median annual salary of $155,000 and $150,000 respectively in the US in 2023. This further supports the idea that experience and technical skills are associated with pay.

## 4. How well do companies pay Data Scientists for certain skills?

During my analysis, I was also curious to see what technical skills were the most popular, as well as what skills were paid the highest. This will assist me in my development as a Data Scientist by providing a clearer understanding of what new skills to acquire.

View my notebook with detailed steps here:
[Salary_Analysis.ipynb](notebooks/Salary_Analysis.ipynb)

### Visualize Data

```python
#Create plot
fig, ax = plt.subplots(2, 1)
sns.set_theme(style='ticks')

#Visualize plot
sns.barplot(data=df_DS_US_top_paid_skills, x='median_salary', y=df_DS_US_top_paid_skills.index, hue='median_salary', ax=ax[0], palette='dark:g_r')
sns.barplot(data=df_DS_US_top_popular_skills, x='median_salary', y=df_DS_US_top_popular_skills.index, hue='median_salary', ax=ax[1], palette='dark:g_r')

#Label plot
ax[0].set_title('Top 10 Highest Paid Skills for Data Scientists')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))
ax[0].legend().remove()

ax[1].set_title('Top 10 Most In-Demand Skills for Data Scientists')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary ($USD)')
ax[1].set_xlim(ax[0].get_xlim()) # Set the same x-axis as the first plot
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))
ax[1].legend().remove()

plt.tight_layout()
plt.show()
```

### Results

 ![Visualization of How Well Jobs Pay for Top Skills](images/pay_distribution_for_ds_skills.png)
 
*Two separate bar graphs visualizing the highest-paid skills and the most in-demand skills for Data Scientists in the US in 2023*

* The top graph shows specialized technical skills like `Airtable`, `Unreal`, `Ruby`, and `Redhat`. However, there are also very popular business/project management platforms such as `Notion`, `Slack`, and `Asana`. These skills are correlated with high salaries, some of which reach up to $250,000! This indicates that having a fluent understanding of business/project management tools and specific technical experience can lead to an increase in earning potential.

* The bottom graph highlights the most in-demand skills which showcases the foundational skills that are staples across the industry. These skills include popular programming languages like `Python` and `SQL` but also include popular ML packages like `Tensorflow` as well as cloud/big data technologies such as `Spark` and `Azure`. The variety of skills here describes the need to have a solid foundation in programming, machine learning, and cloud computing to stay competitive in today's market.

* Analyzing the graphs together reveals a distinct disparity between the most popular skills and the highest-paid skills. Data Scientists aiming to maximize their salary potential should focus on building a solid foundation in core skills while also becoming proficient in business and project management tools to effectively collaborate with stakeholders. Additionally, gaining expertise in specialized skills could enhance career prospects to those who are looking to break into specific industries.

## 5. What is the most optimal skill to learn as a Data Scientist?

Building upon the previous question, now that I know what the most popular and highest-paid skills are, I was curious to find what the most "optimal" skills would be. Where "optimal" skills are determined by skills that are both highly in-demand AND highly paid. To find this, I needed to do the following: 

1. Group skills to determine median salary and likelihood of being in a job posting
2. Visualize median salary vs percent of demand for skills
3. Determine if certain technologies are more prevalent

View my notebook with detailed steps here:
[Optimal_Skills.ipynb](notebooks/Optimal_Skills.ipynb)

### Visualize Data

```python
#Visualize plot
sns.scatterplot(
    data=df_plot,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)

#Prepare texts for adjustText
texts = []
for i, txt in enumerate(df_DS_skills_high_demand.index):
    texts.append(plt.text(df_DS_skills_high_demand['skill_percent'].iloc[i], 
                          df_DS_skills_high_demand['median_salary'].iloc[i],
                          txt))
    
#Adjust text to avoid overlap
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

#Label plot
plt.title('Most Optimal Skills for Data Scientists in the US')
plt.xlabel('Percent of Data Scientist Jobs')
plt.ylabel('Median Yearly Salary ($USD)')
plt.legend(title='Technology')

sns.despine()
sns.set_theme(style='ticks')

ax=plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.tight_layout()
plt.show()
```

### Results

 ![Visualization of Most Optimal Data Scientist Skills in the US in 2023](images/most_optimal_ds_skills.png)
 
*Most optimal skills for Data Scientists in the US in 2023*

### Insights

* The scatter plot above shows that most of the `programming` skills are both highly in-demand AND highly paid.

* Machine Learning packages such as `Tensorflow` appear as one of the highest-paid skills but appear in less than 10% of job postings which indicates that it is more of a specialized skill that is highly paid for certain companies/industries. 

* Cloud/big data-related technologies seem to rank second in terms of most optimal tools. Skills such as `Apache Spark`, `Azure`, and `Hadoop` are all skills related to big data and cloud computing that seem to be in demand and pay well.

* Finally, the distribution of skills, particularly the scarcity of skills in the upper right corner of the graph (which represents both high demand and high compensation), highlights that hardly any skills are simultaneously highly sought after and highly rewarded for Data Scientists. This suggests that the skillsets required to become a successful Data Scientist are not standardized and can vary significantly across different jobs, companies, and industries. As a result, it can be challenging for Data Scientists to identify a definitive set of "optimal" skills to acquire.

## Conclusion

This analysis provides a comprehensive overview of the current tech job market, focusing on the skillsets required for Data Scientists, Data Engineers, and Data Analysts. The findings offer valuable insights into the demand for various skills and their relationship to compensation and popularity.

### Key Observations

1. **Versatility of Core Skills**: Python and SQL are identified as essential and versatile skills across Data Analyst, Data Engineer, and Data Scientist roles. Their widespread use underscores their fundamental importance in the tech industry.

2. **Importance of Data Visualization**: Proficiency in data visualization tools, particularly Tableau, is crucial for Data Analysts and Data Scientists. This skill is vital for effectively communicating data insights to business stakeholders.

3. **Specialized Technical Skills**: Data Engineers demonstrate a strong demand for cloud computing technologies such as Azure and Spark, highlighting the importance of expertise in managing complex data pipelines.

4. **Disparity in Skill Valuation**: The analysis reveals a gap between the most popular skills and those offering the highest compensation. This indicates that skills that are both highly sought after and highly rewarded are limited. Consequently, the skillsets for Data Scientists are diverse and vary across different roles, companies, and industries, making it challenging to identify a definitive set of "optimal" skills.

Overall, this project highlights the need for Data Scientists to develop a robust set of core skills while also exploring specialized and business-related competencies to enhance career prospects. The variability in skill requirements across different sectors emphasizes the importance of continuous learning and adaptability in the evolving tech landscape.

