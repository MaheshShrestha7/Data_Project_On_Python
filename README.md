# Data Engineer Australian Market Analysis - Project On Python
# Overview 
his project provides a comprehensive analysis of the data engineering job market in Australia. Driven by a need to navigate the evolving tech landscape, this study identifies high-demand skills and maps them against salary trends to uncover optimal career opportunities.

Leveraging data from [Luke Barousse’s Python Course](https://www.youtube.com/watch?v=wUSDVGivd-8), this repository contains Python-based explorations into job titles, compensation, and geographic trends. By analyzing the intersection of technical requirements and economic returns, this project serves as a strategic guide for data professionals aiming to maximize their market value.

# The Questions 
Below are the questions I wanted to answer in my project : 
1. What are the skills most in-demand for the TOP 3 popular data roles?
2. How are in-demand skills trending for Data engineers?
3. How well do jobs and skills pay for Data engineers?
4. What are the optimal skills for data engineers to learn? (High demand and High paying)

# Tools used 
 For my deep dive into the data engineers job market, I harnessed the power of several key tools : 
  - Python
    - Pandas Library : Used for analysing the data.
    - Matplotlib Library : used for visualing data.
    - Seaborn Library : used for creating more advanced visuals.
  - Jupyter Notebooks : used for running python scripts , which allows to include notes/analysis.
  - Visual Studio Code : go-to IDE for executing python script.
  - Git & GitHub : Used for version control and sharing of python code/analysis. 


# Data Preparation and Cleanup 
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Cleanup data
I start by importing the required Python libraries and loading the dataset, followed by the initial data cleaning to ensure data is clean and ready for analysis. 




# The Analysis

 ## 1_EDA_Intro.ipynb
Contains Explainatory Data Analysis intro.

# 2_Skill_Demand.ipynb   

# What are the most demanded skills for the TOP 5 most popular data roles?

 To find the most demanded skills for top 5 most popular data roles. I filtered out these postitions by which ones were the most popular, and then got the top 5 skills for these top 5 roles. This query highlights the most popular job titles and their top skills , showing which skills I should pay attention to depending on the role I'm targeting.

 View my notebook with detailed steps here : 
 [2_Skill_Demand.ipynb](3_Project/2_Skill_Demand.ipynb)

 ### Visualise Data
 ```python
fig, ax =plt.subplots(len(job_titles),1, figsize=(10,15)) # Creating subplots for each job title        
sns.set_style('ticks')  # Setting seaborn style for better aesthetics

for i, job_title in enumerate( job_titles):
    df_plot = df_skills_percent[df_skills_percent['job_title_short']==job_title].head(5) # Getting top 5 skills for each job title
    #df_plot.plot(kind='barh',x='job_skills',y='skill_percent',ax=ax[i],title=job_title) # Plotting horizontal bar chart
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')  # Using seaborn for better aesthetics

    ax[i].set_title(job_title)  # Setting title for each subplot
    #ax[i].invert_yaxis()  # Inverting y-axis to have the highest count on top
    ax[i].set_ylabel('') # Removing y-axis label
    ax[i].legend().set_visible(False) # Hiding legend
    ax[i].set_xlim(0,68) # Setting x-axis limit for better visualization

    # Adding data labels to bars as Skill Percentage
    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f"{v:.1f}%", color='black', va='center')  # Adding data labels to bars

    # Hiding x-axis ticks for all except the last subplot
    if i != len(job_titles) - 1:
        ax[i].set_xticks([]) # Hiding x-axis ticks

fig.suptitle('Likelihood of Skills requested in Australia Job Posting', fontsize=16) # Setting overall title
fig.tight_layout(h_pad=0.5) # Fix the overlapping issue
plt.show() 

```

### Results

![Visualisation](3_Project/images/skill_demand_plot.png)


### Insights
- SQL is the most versatile skill, highly demanded across all 5 top roles , most prominently for Data engineer (62.5%)
- Also, SQL is most requested skill for Senior data enginner
- Data engineer requires more specialised technical skills (AWS,azure,Spark) compared to data analyst.




# 3_Skill_Trend.ipynb
# How are in-demand skills trending for Data engineers?

 ### Visualise Data
 ```python
from matplotlib.ticker import PercentFormatter
ax = plt.gca()  # Getting current axis
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))  # Formatting y-axis to percentage

# Adding skill names at the end of each line for clarity
for i in range(3):
    plt.text(11,df_plot.iloc[-1,i],df_plot.columns[i]) #11 is the index for December, df_plot.iloc[-1,i] gets the last row value for each skill ,df_plot.columns[i] gets the skill name
plt.show()
 ```

![Trending Top Skills for Data Engineers in Australia](3_Project/images/skill_trend_plot.png)
*Bar graph visualising the trending top skills for data engineers in Australia in 2023*


### Insights : 
- Airflow remains the most in-demand skills throughout the year.
- alteryx is the second most in-demand skills and seem to have dipped down to its lowest (of around 1%) in the month of May and August.
-Angular is always the lowest throughout the year.




# 4_Salary_analysis.ipynb
# How well do jobs pay?

 ### Visualise Data
 ```python
sns.boxplot(data=df_AUS_top6, x='salary_year_avg', y='job_title_short',order=job_order)
sns.set_theme(style='ticks')

# this is all the same
plt.title('Salary Distributions in Australia')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 600000) 
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
 ```
 ![Salary Distributions of Data Jobs in Australia](3_Project/images/salary_analysis_plot.png)
 *Box plot visualising the salary distributions for the TOP 6 data job titles.*

 ### Insights :
 - Software engineer has the most consistent salary ranging from ~100K to ~220K
 - Senior Data Engineer has boxed salary between ~100K to ~150K



# What are the in-demand and highest paying skills for data engineers in Australia ?

### Visualise Data 
```python
fig, ax = plt.subplots(2, 1)  

sns.set_theme(style='ticks')

# Top 10 Highest Paid Skills for Data Engineers
sns.barplot(data=df_DE_top_pay, x='median', y=df_DE_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')
ax[0].legend().remove()
# original code:
# df_DE_top_pay[::-1].plot(kind='barh', y='median', ax=ax[0], legend=False) 
ax[0].set_title('Top 10 Highest Paid Skills for Data Engineers')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))


# Top 10 Most In-Demand Skills for Data Engineers
sns.barplot(data=df_DE_skills, x='median', y=df_DE_skills.index, hue='median', ax=ax[1], palette='light:b')
ax[1].legend().remove()
# original code:
# df_DE_skills[::-1].plot(kind='barh', y='median', ax=ax[1], legend=False)
ax[1].set_title('Top 10 Most In-Demand Skills for Data Engineers')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_xlim(ax[0].get_xlim())  # Set the same x-axis limits as the first plot
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

plt.tight_layout()
plt.show()
```
 ![The highest Paid and most in-demand skills for Data engineers in Australia](3_Project/images/salary_analysis_2_plot.png)
 *Two seperate bar graphs visualising the highest paid skills and most in-demand skills for data engineers in Australia*

 ### Insights :
 - SSIS is the highest paid skill for data engineers in Australia with salary hitting ~160K
 - go is the most in-demand skills
 - bitbucket, c, git, atlassian and node.js are on the same category relating to highest paying skills for data engineers of around ~150K



# 5_Optimal_Skills.ipynb
# What are the most optimal skill to learn for Data Engineers?

 #### Visualise Data
 ```python
from adjustText import adjust_text
from matplotlib.pyplot as plt
sns.scatterplot(
    data=df_plot,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)

plt.show()
 ```

 #### Results : 
 ![Most Optimal Skills for Data Engineers in Australia](3_Project/images/optimal_skills_plot.png)
*A scatter plot visualising the most optimal skills (high paying and high demand) for data engineers in Australia.*

#### Insights : 
- The scatter plot show `go` is highly paid skill under programming technology however has less percentage of data engineer jobs openings.

- `python` and `sql` skills are high demanded skills for data engineer but are on the lower side of median salary.

- Under `cloud` technology , `aws` is highly regarded.
- `sql server` from `database` technology is on the lower side of job openings.


# What I Learned 
Throughtout this project, I deepned my understanding of the data engineers job market and enhanced my technical skills in Python, especially in data manipulation and visualisation. Here are the few specific things to mention : 
- Advanced Python Proficiency: Leveraged the Pandas ecosystem for complex data manipulation and used Matplotlib and Seaborn to create sophisticated, data-driven visualizations.

- Data Integrity & Preprocessing: Gained hands-on experience in rigorous data cleaning, reinforcing the principle that the quality of analytical insights is directly tied to the integrity of the underlying data.

- Strategic Market Analysis: Developed a framework for aligning technical skill sets with market realities. By analyzing the intersection of demand, compensation, and availability, I’ve refined my approach to strategic career development in the tech sector.


# Insights 
This project provided several general insights into the job market for data engineers : 
- Compensation Mapping: Identified a direct correlation between specialized technical competencies and premium salary brackets.

- Market Dynamics: Observed a high rate of volatility in tool-specific demand, emphasizing the need for an adaptable skill set in a shifting landscape.

- Strategic Career Pathing: Pinpointed "high-value" skill clusters—those that are both high-demand and high-compensation—to provide a data-backed roadmap for maximizing professional ROI.

# Challanges 
This project was not without its challanges, but it provided good learning opportunities : 
- Data Integrity: Addressing missing and inconsistent entries required a robust cleaning pipeline to ensure the accuracy and reliability of the final analysis.

- Data Visualization: Translating complex datasets into intuitive visual representations was a significant challenge, requiring iterative design to balance detail with clarity.

- Scope Management: Maintaining a balance between deep-dive technical analysis and a high-level overview was critical to ensuring the project remained accessible without losing depth.

# Conclusion
This project offers a comprehensive look into the data engineering job market, identifying the core skills and emerging trends shaping the industry. The insights gathered provide a strategic roadmap for professionals looking to pivot into or advance within data engineering roles. As the landscape evolves, this analysis underscores the necessity of continuous learning and proactive adaptation. This repository serves as a foundation for future market research and data-driven career planning.



