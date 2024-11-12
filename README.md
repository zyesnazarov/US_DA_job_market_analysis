# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

 I filtered out those positions y which ones were the most pupular, and query highlights the most popular job titles and their tp skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here: [2_Skills_count.ipynb](Python\Projects\2_Skills_count.ipynb)

### Visualize data

```Python
fig, ax = plt.subplots(len(job_titles),1)

for i,job_title in enumerate(job_titles):
    df_plot = df_skills_count[df_skills_count['job_title_short'] == job_title].head(5)
    df_plot.plot(kind = 'barh', x = 'job_skills', y = 'skill_count', ax = ax[i], title = job_title)
    plt.tight_layout()
    ax[i].invert_yaxis()
    ax[i].set_ylabel('')
    ax[i].legend().set_visible(False)

fig.suptitle('Count of top skills in job postings', fontsize = 15)
fig.tight_layout(h_pad=1)
plt.show()
``` 
### Results

![Visualisation of top skills in US](Python/Project_1/Images/Skill_demand_all_roles.png)

### Insights
- Python is the most sought after skills among data roles in US, representing 65% and 72% share in job posting requirements in US for data engineers and data scientist roles, respectively.
- SQL is the second most sough after skill in a job postings required with the share of 51% for data scientists and data analyst roles, witht the highest demand for data engineer role with a share of 68%
- AWS, Azure, Spark are specialized skills for data engineer roles hence they appear only in data engineer job postings with a range between 30% and 45%.
- Visualization Skills Have Moderate Demand: Both Data Analysts and Data Scientists have skills like Tableau (28% for Data Analysts) and visualization tools like SAS (24% for Data Scientists) in the top 5. This suggests that while these skills are valuable, they are secondary compared to core technical skills like SQL and Python.

## 2. How are in-demand skills trending for Data Analysts

### Visualize data

```python
sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')
sns.despine()

plt.legend().remove()
plt.ylabel('Likelihood in job posting')
plt.xlabel('2023')

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

for i in range(5):
    plt.text(11.2, df_plot.iloc[-1,i], df_plot.columns[i])
plt.show()
```

### Results

![Trending top skills for data analyst roles in US](Python/Project_1/Images/Skill_trend_US.png)
*Bar graph reflection top skills required for data analyst roles in US


## 3. How well do jobs and skills pay for data analysts?

### Salary analysis

#### Visualize data

```python
sns.boxplot(data=df_us_top6, x = 'salary_year_avg',  y = 'job_title_short', order=job_order)
sns.set_theme(style='ticks')

plt.title('Salary distribution in the United States')
plt.xlabel('Yearly salary USD')
plt.ylabel('')
plt.xlim(0,600000)
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

#### Results

![Salary distribution for data jobs in US](Python/Project_1/Images/Salary_distribution_US.png)
*Box plot visualizing the salary distributions for the top 6 data job titles.*

#### Insights 
- Higher Median Salaries for Senior Roles: Senior Data Scientist and Senior Data Engineer roles exhibit higher median salaries compared to their non-senior counterparts and other roles. This indicates that senior-level positions in data science and engineering are compensated significantly more.

- Wider Salary Range in Senior Positions: The salary distributions for senior positions, especially Senior Data Scientist and Senior Data Engineer, have wider ranges with several outliers extending to high salary levels, suggesting higher potential earning variability and outlier compensation packages in these roles.

- Data Analyst Roles Have Lower Compensation: Both Data Analyst and Senior Data Analyst roles display lower overall salary distributions compared to engineering and scientist roles. This suggests that data analyst positions generally have lower median and upper-range salaries within the data profession.

- Presence of Outliers Across All Roles: There are outliers present in the salary distributions for all positions, indicating that while most salaries fall within a common range, some individuals in these roles may earn significantly above (or below) the typical salary range.

### Investigate median salary vs skill for data analysts>

#### Visualize data
```python
fig, ax = plt.subplots(2, 1)

sns.set_theme(style = 'ticks')

# Top 10 Highest paid skills for data analysts in US
sns.barplot(data = df_da_us_top_pay, x = 'median', y = df_da_us_top_pay.index, hue = 'median', ax = ax[0], palette = 'dark:b_r')
ax[0].legend().remove()
ax[0].set_title('Top 10 highest paid skills for data analysts in US')
ax[0].set_xlabel('')
ax[0].set_ylabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))


# Top 10 most in-demand skills for data analysts in US
sns.barplot(data = df_da_us_top_skills, x = 'median', y = df_da_us_top_skills.index, hue = 'median', palette = 'light:b', ax = ax[1])
ax[1].legend().remove()
ax[1].set_title('Top 10 most in-demand skills for data analysts in US')
ax[1].set_xlabel('')
ax[1].set_ylabel('')
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))
ax[1].set_xlim(ax[0].get_xlim()) #getting ax[0] limits for 2nd graph
plt.tight_layout()
```
#### Results
![Highest paid and most in demand skills for data analyst roles in US](Python/Project_1/Images/Top_highest_paid_skills.png)
*two separate bar graphs reflecting highest paid skills and most in-demand skills for data analyst roles in US*

#### Insights

- Specialized Technical Skills Command Higher Salaries: The highest paid skills, such as dplyr, bitbucket, gitlab, and solidity, are more specialized tools and technologies, suggesting that niche or advanced technical expertise is rewarded with higher salaries.
- Emerging Technologies in High Demand: Skills such as Hugging Face and Solidity highlight the growing importance of machine learning frameworks and blockchain technology in the data analysis landscape, indicating trends toward these areas as lucrative skill sets.
- Diverse Skill Sets Lead to Higher Compensation: The range of skills shown (from version control tools like gitlab and bitbucket to cloud and database solutions like Couchbase and Cassandra) suggests that data analysts who expand their expertise beyond basic analysis tools can achieve higher pay.
- Core Programming and Visualization Tools are Essential: Python, Tableau, and R top the list of in-demand skills, demonstrating that proficiency in programming languages and data visualization tools is crucial for data analysts.
- SQL Knowledge Remains a Staple: SQL Server and SQL rank highly, confirming that strong database management and querying capabilities are fundamental for most data analyst roles.
- Widespread Business Tools are Important: Power BI, PowerPoint, Excel, and even Word feature as highly sought-after skills, indicating that data analysts are expected not only to analyze data but also to present and communicate insights effectively using familiar business software.
- Traditional Software Still Relevant: The inclusion of Excel and Word highlights that even as advanced technical skills become more important, traditional software still plays a significant role in day-to-day operations for data analysts.

## 4. What is the most optimal skill to learn for data analysts?

#### Visualize data

```python
#plotting the result
from adjustText import adjust_text
#top_5pc_skills.plot(kind = 'scatter', x = 'skill_percent', y = 'median_salary')
sns.scatterplot(data=df_plot, x = 'skill_percent', y = 'median_salary', hue = 'technology')
sns.despine()
sns.set_theme(style='ticks')


#prepare text for adjust_text
texts = []

for index, text in enumerate(top_5pc_skills.index):
    texts.append(plt.text(top_5pc_skills['skill_percent'].iloc[index], top_5pc_skills['median_salary'].iloc[index], text))


adjust_text(texts, arrowprops = dict(arrowstyle = '->', color = 'gray')) #using adjust_text module to prevent from text overlapping on the graph

ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')) #formatting to getting $xxK numbers format
from matplotlib.ticker import PercentFormatter #importing percent formatter from matplotlib submodule to get % on axis
ax.xaxis.set_major_formatter(PercentFormatter(decimals = 0))

plt.title('The most optimal skills for data analyst roles in US')
plt.xlabel('Percent of DA jobs')
plt.ylabel('Median yearly salary')
plt.tight_layout()
plt.show()
```
#### Results
![Most optimal skills for data analysts in US](Python/Project_1/Images/Most_optimal_skills_for_DA.png)
*A scatter plot presenting the most optimal skills to learn to get data analyst job position in US*


#### Insights

- Python and SQL are highly valuable: Python and SQL stand out as some of the most optimal skills for data analyst roles, with Python offering the highest median yearly salary (around $98K) and appearing in a substantial percentage of job postings (~25%). SQL, on the other hand, has the highest presence in job postings (~55%) with a median salary of ~$90K.

- Excel's wide usage but lower salary: While Excel is included in approximately 45% of data analyst job postings, its median yearly salary is on the lower end (~$85K), indicating that although it's a common requirement, it may not be associated with the highest-paying roles.

- Specialized database skills like Oracle and SQL Server pay well: Skills related to Oracle and SQL Server command some of the highest median salaries ($96K and $94K, respectively), even though they appear in fewer job postings (~15% each), suggesting that specialized database expertise can yield higher pay.

-Power BI and Tableau are lucrative analyst tools: Tableau and Power BI are associated with median salaries around $90K. However, Power BI is mentioned more frequently in job postings (~20%) compared to Tableau (~15%), suggesting Power BI might be more in demand despite similar earning potential.