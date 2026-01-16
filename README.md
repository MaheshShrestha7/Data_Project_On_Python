# Data_Project_On_Python
## 1_EDA_Intro.ipynb
Contains Explainatory Data Analysis intro.

## 2_Skill_Demand.ipynb
 # The Analysis
 ## What are the most demanded skills for the TOP 5 most popular data roles?

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