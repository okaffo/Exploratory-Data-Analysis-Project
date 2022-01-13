# Exploratory-Data-Analysis-Project
Exploratory Data Analysis Lab
Estimated time needed: 30 minutes

In this module you get to work with the cleaned dataset from the previous module.

In this assignment you will perform the task of exploratory data analysis. You will find out the distribution of data, presence of outliers and also determine the correlation between different columns in the dataset.

Objectives
In this lab you will perform the following:

Identify the distribution of data in the dataset.

Identify outliers in the dataset.

Remove outliers from the dataset.

Identify correlation between features in the dataset.

Hands on Lab
Import the pandas module.

import pandas as pd
import numpy as np
Load the dataset into a dataframe.

df = pd.read_csv("https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DA0321EN-SkillsNetwork/LargeData/m2_survey_data.csv")
df.head()
Respondent	MainBranch	Hobbyist	OpenSourcer	OpenSource	Employment	Country	Student	EdLevel	UndergradMajor	...	WelcomeChange	SONewContent	Age	Gender	Trans	Sexuality	Ethnicity	Dependents	SurveyLength	SurveyEase
0	4	I am a developer by profession	No	Never	The quality of OSS and closed source software ...	Employed full-time	United States	No	Bachelor’s degree (BA, BS, B.Eng., etc.)	Computer science, computer engineering, or sof...	...	Just as welcome now as I felt last year	Tech articles written by other developers;Indu...	22.0	Man	No	Straight / Heterosexual	White or of European descent	No	Appropriate in length	Easy
1	9	I am a developer by profession	Yes	Once a month or more often	The quality of OSS and closed source software ...	Employed full-time	New Zealand	No	Some college/university study without earning ...	Computer science, computer engineering, or sof...	...	Just as welcome now as I felt last year	NaN	23.0	Man	No	Bisexual	White or of European descent	No	Appropriate in length	Neither easy nor difficult
2	13	I am a developer by profession	Yes	Less than once a month but more than once per ...	OSS is, on average, of HIGHER quality than pro...	Employed full-time	United States	No	Master’s degree (MA, MS, M.Eng., MBA, etc.)	Computer science, computer engineering, or sof...	...	Somewhat more welcome now than last year	Tech articles written by other developers;Cour...	28.0	Man	No	Straight / Heterosexual	White or of European descent	Yes	Appropriate in length	Easy
3	16	I am a developer by profession	Yes	Never	The quality of OSS and closed source software ...	Employed full-time	United Kingdom	No	Master’s degree (MA, MS, M.Eng., MBA, etc.)	NaN	...	Just as welcome now as I felt last year	Tech articles written by other developers;Indu...	26.0	Man	No	Straight / Heterosexual	White or of European descent	No	Appropriate in length	Neither easy nor difficult
4	17	I am a developer by profession	Yes	Less than once a month but more than once per ...	The quality of OSS and closed source software ...	Employed full-time	Australia	No	Bachelor’s degree (BA, BS, B.Eng., etc.)	Computer science, computer engineering, or sof...	...	Just as welcome now as I felt last year	Tech articles written by other developers;Indu...	29.0	Man	No	Straight / Heterosexual	Hispanic or Latino/Latina;Multiracial	No	Appropriate in length	Easy
5 rows × 85 columns

Distribution
Determine how the data is distributed
The column ConvertedComp contains Salary converted to annual USD salaries using the exchange rate on 2019-02-01.

This assumes 12 working months and 50 working weeks.

Plot the distribution curve for the column ConvertedComp.

df.dropna(subset=["ConvertedComp"],axis=0,inplace=True)
# your code goes here
import matplotlib as mpl
import matplotlib.pyplot as plt
import seaborn as sns
sns.distplot(df['ConvertedComp'],kde=True)
plt.show()
/opt/conda/envs/Python-3.8-main/lib/python3.8/site-packages/seaborn/distributions.py:2557: FutureWarning: `distplot` is a deprecated function and will be removed in a future version. Please adapt your code to use either `displot` (a figure-level function with similar flexibility) or `histplot` (an axes-level function for histograms).
  warnings.warn(msg, FutureWarning)

Plot the histogram for the column ConvertedComp.

# your code goes here
plt.hist(df['ConvertedComp'])
plt.title('Histogram of ConvertedComp')
plt.xlabel('ConvertedComp')
plt.ylabel('Number of Responders')
plt.show()

What is the median of the column ConvertedComp?

# your code goes here
df['ConvertedComp'].median()
57745.0
How many responders identified themselves only as a Man?

# your code goes here
df['Gender'].value_counts()
Man                                                            9725
Woman                                                           679
Non-binary, genderqueer, or gender non-conforming                59
Man;Non-binary, genderqueer, or gender non-conforming            26
Woman;Non-binary, genderqueer, or gender non-conforming          14
Woman;Man                                                         7
Woman;Man;Non-binary, genderqueer, or gender non-conforming       2
Name: Gender, dtype: int64
Find out the median ConvertedComp of responders identified themselves only as a Woman?

# your code goes here
df.loc[df['Gender']=='Woman',['ConvertedComp']].median()
ConvertedComp    57708.0
dtype: float64
df.groupby('Gender')['ConvertedComp'].median()
Gender
Man                                                            57744.0
Man;Non-binary, genderqueer, or gender non-conforming          59520.0
Non-binary, genderqueer, or gender non-conforming              67142.0
Woman                                                          57708.0
Woman;Man                                                      21648.0
Woman;Man;Non-binary, genderqueer, or gender non-conforming    30244.0
Woman;Non-binary, genderqueer, or gender non-conforming        65535.5
Name: ConvertedComp, dtype: float64
Give the five number summary for the column Age?

#first drop NaN from column Age
df.dropna(subset=["Age"],axis=0,inplace=True)
Double click here for hint.

# your code goes here
#find 1st five number summary of the column Age
data=df["Age"]
print('Min:%.3f'% data.min())
print('Q1:%.3f'% data.quantile(.25))
print('Median:%.f'% data.quantile(.50))
print('Q3:%.3f'% data.quantile(.75))
print('Max:%.3f'% data.max())
Min:16.000
Q1:25.000
Median:29
Q3:35.000
Max:99.000
Plot a histogram of the column Age.

# your code goes here
plt.hist(df["Age"])
plt.title("Histogram of Age of Respondents")
plt.xlabel("Age")
plt.ylabel("Number of Respondents")
plt.show()

Outliers
Finding outliers
Find out if outliers exist in the column ConvertedComp using a box plot?

# your code goes here
df['ConvertedComp'].plot(kind='box',figsize=(20,8),vert=False)
plt.title('Box Plot of ConvertedComp')
plt.show()

#find out outliers in the column Age
df['Age'].plot(kind='box',figsize=(20,8),vert=False)
plt.title('Age')
plt.show()

Find out the Inter Quartile Range for the column ConvertedComp.

# your code goes here
Q1 = df['ConvertedComp'].quantile(0.25)
Q3 = df['ConvertedComp'].quantile(0.75)
IQR = Q3 - Q1
print(IQR)
73165.5
Find out the upper and lower bounds.

# your code goes here
upper_bound = Q3 + 1.5*IQR
lower_bound = Q1 - 1.5*IQR
print(upper_bound,lower_bound)
209748.25 -82913.75
Identify how many outliers are there in the ConvertedComp column.

# your code goes here
df[(df['ConvertedComp']>upper_bound)].ConvertedComp.count()
861
Create a new dataframe by removing the outliers from the ConvertedComp column.

# your code goes here
df2 = df[(df['ConvertedComp']>lower_bound)&(df['ConvertedComp']<upper_bound)]
df2
Respondent	MainBranch	Hobbyist	OpenSourcer	OpenSource	Employment	Country	Student	EdLevel	UndergradMajor	...	WelcomeChange	SONewContent	Age	Gender	Trans	Sexuality	Ethnicity	Dependents	SurveyLength	SurveyEase
0	4	I am a developer by profession	No	Never	The quality of OSS and closed source software ...	Employed full-time	United States	No	Bachelor’s degree (BA, BS, B.Eng., etc.)	Computer science, computer engineering, or sof...	...	Just as welcome now as I felt last year	Tech articles written by other developers;Indu...	22.0	Man	No	Straight / Heterosexual	White or of European descent	No	Appropriate in length	Easy
1	9	I am a developer by profession	Yes	Once a month or more often	The quality of OSS and closed source software ...	Employed full-time	New Zealand	No	Some college/university study without earning ...	Computer science, computer engineering, or sof...	...	Just as welcome now as I felt last year	NaN	23.0	Man	No	Bisexual	White or of European descent	No	Appropriate in length	Neither easy nor difficult
2	13	I am a developer by profession	Yes	Less than once a month but more than once per ...	OSS is, on average, of HIGHER quality than pro...	Employed full-time	United States	No	Master’s degree (MA, MS, M.Eng., MBA, etc.)	Computer science, computer engineering, or sof...	...	Somewhat more welcome now than last year	Tech articles written by other developers;Cour...	28.0	Man	No	Straight / Heterosexual	White or of European descent	Yes	Appropriate in length	Easy
4	17	I am a developer by profession	Yes	Less than once a month but more than once per ...	The quality of OSS and closed source software ...	Employed full-time	Australia	No	Bachelor’s degree (BA, BS, B.Eng., etc.)	Computer science, computer engineering, or sof...	...	Just as welcome now as I felt last year	Tech articles written by other developers;Indu...	29.0	Man	No	Straight / Heterosexual	Hispanic or Latino/Latina;Multiracial	No	Appropriate in length	Easy
5	19	I am a developer by profession	Yes	Never	The quality of OSS and closed source software ...	Employed full-time	Brazil	No	Some college/university study without earning ...	Computer science, computer engineering, or sof...	...	Just as welcome now as I felt last year	Tech articles written by other developers;Indu...	31.0	Man	No	Straight / Heterosexual	Hispanic or Latino/Latina	Yes	Too long	Easy
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
11392	25134	I am a developer by profession	Yes	Less than once a month but more than once per ...	OSS is, on average, of HIGHER quality than pro...	Employed full-time	Ecuador	No	Bachelor’s degree (BA, BS, B.Eng., etc.)	Computer science, computer engineering, or sof...	...	Somewhat less welcome now than last year	Tech articles written by other developers	32.0	Man	No	Straight / Heterosexual	Hispanic or Latino/Latina	No	Appropriate in length	Easy
11393	25136	I am a developer by profession	Yes	Never	OSS is, on average, of HIGHER quality than pro...	Employed full-time	United States	No	Master’s degree (MA, MS, M.Eng., MBA, etc.)	Computer science, computer engineering, or sof...	...	Just as welcome now as I felt last year	Tech articles written by other developers;Cour...	36.0	Man	No	Straight / Heterosexual	White or of European descent	No	Appropriate in length	Difficult
11394	25137	I am a developer by profession	Yes	Never	The quality of OSS and closed source software ...	Employed full-time	Poland	No	Master’s degree (MA, MS, M.Eng., MBA, etc.)	Computer science, computer engineering, or sof...	...	A lot more welcome now than last year	Tech articles written by other developers;Tech...	25.0	Man	No	Straight / Heterosexual	White or of European descent	No	Appropriate in length	Neither easy nor difficult
11395	25138	I am a developer by profession	Yes	Less than once per year	The quality of OSS and closed source software ...	Employed full-time	United States	No	Master’s degree (MA, MS, M.Eng., MBA, etc.)	Computer science, computer engineering, or sof...	...	A lot more welcome now than last year	Tech articles written by other developers;Indu...	34.0	Man	No	Straight / Heterosexual	White or of European descent	Yes	Too long	Easy
11396	25141	I am a developer by profession	Yes	Less than once a month but more than once per ...	OSS is, on average, of LOWER quality than prop...	Employed full-time	Switzerland	No	Secondary school (e.g. American high school, G...	NaN	...	Somewhat less welcome now than last year	NaN	25.0	Man	No	Straight / Heterosexual	White or of European descent	No	Appropriate in length	Easy
9493 rows × 85 columns

#create new dataframe
#create new dataframe
df['ConvertedComp']=df['ConvertedComp'].clip(lower_bound,upper_bound)
df['ConvertedComp'].describe()
count     10354.000000
mean      72214.282524
std       58630.015010
min           0.000000
25%       26834.500000
50%       57600.000000
75%      100000.000000
max      209748.250000
Name: ConvertedComp, dtype: float64
Correlation
Finding correlation
Find the correlation between Age and all other numerical columns.

# your code goes here
df.corr()['Age']
Respondent       0.002394
CompTotal        0.006949
ConvertedComp    0.314245
WorkWeekHrs      0.031592
CodeRevHrs      -0.015742
Age              1.000000
Name: Age, dtype: float64
Authors
Ramesh Sannareddy

Other Contributors
Rav Ahuja

Change Log
Date (YYYY-MM-DD)	Version	Changed By	Change Description
2020-10-17	0.1	Ramesh Sannareddy	Created initial version of the lab
Copyright © 2020 IBM Corporation. This notebook and its source code are released under the terms of the MIT License.

