STUDENT DROPOUT ANALYSIS
Data Visualization & Preprocessing using Python (CSE0315)
Submitted by: Urugonda Varshitha (BETN1DS25015) 
· Submitted to: Dr. Nidhi Dandotiya

1. Introduction and Problem Statement
Getting a student enrolled is only the first step. Keeping that student in education until the course is finished is a separate problem, and it is the one this project looks at. It falls under SDG 4, Quality Education.
Dropout rarely has a single cause. Marks, attendance, stress levels, family income and study habits all play a part, and they affect different groups of students in different ways. If the data is not broken down, dropout looks like one general issue and the groups that need help get missed.
This project studies a dataset of 10,000 students to find out which students drop out and which factors are linked to it. The analysis uses standard statistical methods along with a few automated tools, and the results are shown in an interactive Power BI dashboard that an educator could use to spot students at risk.

3. Objectives
• Clean and preprocess the dataset, and record how missing values and outliers were handled.
• Find the factors most closely linked to dropout: attendance, GPA, stress, gender, department and year of study.
• Compare manual analysis with automated tools, and check where the automated output needed correcting.
• Build an interactive Power BI dashboard that shows which groups of students are most at risk.

4. Dataset
The analysis uses the file student_dropout_dataset_v3.csv, which has 10,000 rows and 19 columns. Each row is one student. The target column is Dropout, where 0 means the student stayed and 1 means the student dropped out. There were no duplicate rows.
Type
Columns
Numeric
Age, Family_Income, Study_Hours_per_Day, Attendance_Rate, Assignment_Delay_Days, Travel_Time_Minutes, Stress_Index, GPA, Semester_GPA, CGPA
Categorical
Gender, Internet_Access, Part_Time_Job, Scholarship, Year_of_Study, Department, Parental_Education
Target
Dropout
Four columns had missing values.
Column
Missing values
Share
Family_Income
500
5.00%
Study_Hours_per_Day
500
5.00%
Stress_Index
500
5.00%
Parental_Education
511
5.11%
   
5. Data Preprocessing
I worked on a copy of the data so the original file stayed untouched.
Renaming. The Semester column held values like Year 1 to Year 4, so I renamed it Year_of_Study.
Missing values. The three numeric columns (Family_Income, Study_Hours_per_Day, Stress_Index) were filled with their median, since the median is not pulled by extreme values. Parental_Education is categorical, so its 511 gaps were filled with the most common value, Bachelor.
Outliers. I used the IQR rule on each numeric column. Family_Income was the clearest case: the valid range came out as roughly 0 to 70,904, and 683 values lay above it. I capped those values at the upper limit instead of dropping rows, which kept all 10,000 students. This lowered the mean income from about 38,400 to about 36,300, so the change is worth remembering when reading income results. Other columns had a smaller number of flagged values, for example 33 in Age and 138 in Study_Hours_per_Day, and I left them as they were because they are realistic values.
Isolation Forest. As a second outlier check I ran Isolation Forest on ten numeric columns and compared it with the IQR rule.
Method
Rows flagged
Isolation Forest
500
IQR (any column)
414
Flagged by both
89
Only Isolation Forest
411
Only IQR
325
The overlap is small. IQR looks at one column at a time, while Isolation Forest looks at unusual combinations across columns, so they find different rows. Isolation Forest was also set to flag 5% of rows, which is why it returns exactly 500. That number comes from the setting, not from the data.
Encoding. Gender, Internet_Access, Part_Time_Job and Scholarship were mapped to 0 and 1. Parental_Education (High School to PhD) and Year_of_Study (1 to 4) were given ordered numbers because their order matters. Department has no natural order, so it was one-hot encoded into five columns.
Scaling. The ten numeric columns were standardised with StandardScaler so each has a mean of 0 and a standard deviation of 1. Two files were saved: a cleaned version and a cleaned, encoded and scaled version.

6. Tools and Techniques
Tool
Used for
Google Colab, Python
Running the whole analysis
Pandas, NumPy
Cleaning, grouping and summarising the data
Matplotlib
Charts in the notebook
Scikit-learn
Isolation Forest outlier detection and StandardScaler
Statsmodels
One-way ANOVA
Sweetviz
Automated exploratory report, generated right after loading the data
Power BI
The interactive dashboard, including the Key Influencers visual
Sweetviz was chosen because it runs locally and needs no account or API key. Its report was treated as a first look only, and the figures in it were checked against my own Pandas calculations. Isolation Forest was the machine-learning method used for outliers, and it was compared with the manual IQR rule instead of replacing it.

8. Statistical and Exploratory Analysis
Dropout by group. Gender and department make almost no difference to dropout.
Group
Dropout rate
Female
23.8%
Male
23.2%
Arts
23.9%
Business
23.3%
CS
23.4%
Engineering
24.0%
Science
23.0%
Dropped out vs stayed. The two groups differ most on GPA and stress.
Measure
Stayed
Dropped out
GPA
2.58
1.43
Stress_Index
5.27
6.28
Attendance_Rate
82.48
79.31
Study_Hours_per_Day
4.07
3.82
Assignment_Delay_Days
1.74
2.00
Attendance level. I grouped Attendance_Rate into three bands: Low (up to 75), Medium (75 to 85) and High (above 85). Of the 10,000 students, 2,043 fall in Low, 4,516 in Medium and 3,441 in High.
High-risk group. Students with Low attendance and a GPA below 1.5 numbered 666, and 63.2% of them dropped out, far above the overall rate of about 23%.
Correlation. On the original data, Stress_Index has a correlation of 0.26 with Dropout. Study_Hours_per_Day has a weaker negative correlation of about -0.09. Age and Family_Income are close to zero.
ANOVA. A one-way ANOVA tested whether average GPA differs across the three attendance levels. The result was significant (F = 195.8, p < 0.001). Average GPA rises from 2.00 for Low attendance to 2.26 for Medium and 2.56 for High.
9. Visualisation and Dashboard
Charts in Python. The charts were made in Matplotlib: a pie chart of the share of students who dropped out, bar charts of dropout rate by department and by gender, a line chart of dropout rate from Year 1 to Year 4, and histograms of GPA, attendance rate and stress index. The two bar charts use the same y-axis range (0 to 30%), which shows how small the gaps between groups are.
Power BI dashboard. The dashboard is built on the cleaned dataset and has one page with these parts:
• KPI cards for the headline numbers: total students, dropout rate, average attendance and average GPA.
• A column chart of dropout rate by attendance level.
• A line chart of dropout rate by year of study, split by department.
• A column chart of average GPA for students who stayed and students who dropped out.
• A scatter plot of GPA against stress index with a trend line.
• A Key Influencers visual with Dropped out as the outcome, which ranks the columns that push dropout up or down. This is the AI-assisted element of the dashboard.
Dashboard screenshot to be inserted here.

11. Key Findings
• About 23% of the 10,000 students dropped out.
• Gender and department are weak factors. Dropout stays between 23.0% and 24.0% across all departments and is 23.8% for female and 23.2% for male students.
• GPA shows the biggest gap. Students who dropped out averaged 1.43, against 2.58 for those who stayed.
• Stress is higher among students who dropped out (6.28 against 5.27) and has a correlation of 0.26 with dropout.
• Attendance and GPA move together. Average GPA rises from 2.00 to 2.56 between Low and High attendance, and the difference is statistically significant.
• The riskiest group found was students with Low attendance and a GPA below 1.5. Of these 666 students, 63.2% dropped out.

13. Evaluation of the Tools, Limitations and Conclusion
Where the automated tools helped. Sweetviz gave a quick overview of every column straight after loading, and Isolation Forest found unusual rows that the IQR rule did not catch, since it looks at several columns together.
Where they needed care. Isolation Forest flags a fixed share of rows set by its contamination setting, so its count of 500 says little by itself, and only 89 of those rows were also flagged by IQR. The two methods should be read as complementary, not as one being right and the other wrong.
Limitations.
• GPA, Semester_GPA and CGPA are closely related, and GPA may partly reflect a student's progress toward dropping out, so its strong link to dropout should be read with that in mind.
• Median filling and capping Family_Income change the original distributions slightly. About a quarter of Family_Income values sit at the minimum of 25,000, which limits what income can show.
• The data describes individual students and has no state, region or calendar year, so the project cannot compare regions or forecast trends over time.
• The ANOVA compares GPA across attendance levels. It does not test dropout directly, and it shows association, not cause.
Conclusion. In this dataset, GPA, attendance and stress are the clearest signs of dropout, while gender and department make very little difference. Students with low attendance and low GPA form a clearly separate high-risk group, with a dropout rate of 63.2% against about 23% overall. An educator could use the dashboard to find such students early and offer support before they leave. Automated tools saved time on the first look at the data, but their output was most useful when checked against manual analysis. 
