
# Examining Racial Discrimination in the US Job Market

### Background
Racial discrimination continues to be pervasive in cultures throughout the world. Researchers examined the level of racial discrimination in the United States labor market by randomly assigning identical résumés to black-sounding or white-sounding names and observing the impact on requests for interviews from employers.

### Data
In the dataset provided, each row represents a resume. The 'race' column has two values, 'b' and 'w', indicating black-sounding and white-sounding. The column 'call' has two values, 1 and 0, indicating whether the resume received a call from employers or not.

Note that the 'b' and 'w' values in race are assigned randomly to the resumes when presented to the employer.

<div class="span5 alert alert-info">
### Exercises
You will perform a statistical analysis to establish whether race has a significant impact on the rate of callbacks for resumes.

Answer the following questions **in this notebook below and submit to your Github account**. 

   1. What test is appropriate for this problem? Does CLT apply?
   2. What are the null and alternate hypotheses?
   3. Compute margin of error, confidence interval, and p-value.
   4. Write a story describing the statistical significance in the context or the original problem.
   5. Does your analysis mean that race/name is the most important factor in callback success? Why or why not? If not, how would you amend your analysis?

You can include written notes in notebook cells using Markdown: 
   - In the control panel at the top, choose Cell > Cell Type > Markdown
   - Markdown syntax: http://nestacms.com/docs/creating-content/markdown-cheat-sheet


#### Resources
+ Experiment information and data source: http://www.povertyactionlab.org/evaluation/discrimination-job-market-united-states
+ Scipy statistical methods: http://docs.scipy.org/doc/scipy/reference/stats.html 
+ Markdown syntax: http://nestacms.com/docs/creating-content/markdown-cheat-sheet
</div>
****


```python
import pandas as pd
import numpy as np
from scipy import stats
import sys
%matplotlib inline
```


```python
# Reading in data

data = pd.io.stata.read_stata("C:\\Users\\sudhanshu\\Desktop\\Springboard\\Inferential Statistics\\Excercise2\\data\\us_job_market_discrimination.dta")

```


```python
# Ques1: What sort of test is appropriate?

#Answer: Two tail Z test for proportions
#underlying assumption is bionomial distribution
```


```python
# Ques2: What are Null and Alternate Hypothesis?

# Ho: No difference between interview call for a White sounding name v.s a Black sounding name
# H1: Significant difference between interview call for a White sounding name v.s a Black sounding name

```


```python
# Checking for  significant diffrence between proportions:

#Subsetting data
black_sounding=data[data["race"]=="b"]
white_sounding=data[data["race"]=="w"]

```


```python
#Counting sample and population size

black_Total=black_sounding["race"].count()
white_Total=white_sounding["race"].count()
total_Call_Count=data["race"].count()

```


```python
#Calculating interview call per population

black_Call=black_sounding["call"].sum()
white_Call=white_sounding["call"].sum()

```


```python
#Calculating Sample and Population proportions

black_Call_Prop= black_Call/black_Total
white_Call_Prop= white_Call/white_Total
total_Call_Prop=data["call"].sum()/data["call"].count()

```


```python
#Calculating Z Score: at critical value +-1.96 (95%)

Stand_Deviation_Population=np.sqrt((2*total_Call_Prop*(1-total_Call_Prop))/total_Call_Count) #as Sample size is same for both populations

ZScore=(white_Call_Prop - black_Call_Prop)/Stand_Deviation_Population

print("Z Score is",ZScore)

```

    Z Score is 5.81017218579
    


```python
#Confidence Interval 95%
print("From:", (white_Call_Prop - black_Call_Prop) + 1.96*Stand_Deviation_Population)
print("To:", (white_Call_Prop - black_Call_Prop) - 1.96*Stand_Deviation_Population)
```

    From: 0.042838798034
    To: 0.0212269103849
    


```python
#As Z Score is outside the critical value range of +-1.96 we can reject the NULL Hypothesis 
#and say that there is a significant difference between the interview call w.r.t race. White race sample has a 
#greater chance of an interview call w.r.t an equivalent Black race sample
# Hence race is an important factor if we need to predict interview call
```
