cep-developer-assessment
========================

This assessment evaluates a candidate's software development skills relevant to CEP's work. Besides efficiency, CEP also looks for clarity in code submissions.

The assessment has three exercises that test a candidate's ability to manipulate CSV data and write JSON outputs using existing Python packages.

It is recommended that the candidate has familiarity with the following software packages: `numpy`, `scipy`, and `pandas`. These packages will simplify the work required in the assessment.

If you have any questions regarding the assessment, please feel free to reach out to [Mike Nguyen](https://github.com/miken) at miken@effectivephilanthropy.org.

For more information about CEP and our work, please visit effectivephilanthropy.org

# Instructions
For each exercise folder, there are three subfolders within:

* `input`: This folder contains all input files to be read by your program
* `output`: This folder contains "model" output files that your program ought to produce. For example, in exercise 1, there are two files in the `output` folder: `mean.csv` and `stats.csv`. Your program is expected to create these two files in the exact format and layout.
* `submissions`: This is where you can save your program in the format `your_name.py`

To submit your codes, first, fork this repo, add your own program files in the `/submissions` folder for each exercise, then make a pull request letting us know that you're submitting your files.

# Exercise 1
In spring 2014, CEP helped 9 foundation clients administer a survey to their grantees. At the end of the survey, CEP analysts cleaned and sent the data to you in a CSV format. The file is called `xl.csv` and saved in the `exercise1/input` folder. This file contains all survey responses, with each column representing a survey question and each row a set of responses from a grantee.

In this file, the column `fdntext` provides the client code for a given survey respondent, i.e., helps identify from which foundation the respondent receives the grant from. The 9 foundation clients have the following 9 codes:

```
Arlington 14S
Boylston 14S
Clarendon 14S
Dartmouth 14S
Kenmore 14S
Marlborough 14S
Peabody 14S
Roxbury 14S
Tremont 14S
```

You are curious to learn how the clients are rated by grantees on the 9 questions in the survey:
```
fldimp
undrfld
advknow
pubpol
comimp
undrwr
undrsoc
orgimp
undrorg
```

Respondents answer these survey questions on a scale of 1 to 7, with a higher number indicating a more positive rating. If respondents indicate that the question is not applicable to them, their responses will be coded as 77. If respondents indicate that they are not sure about the question's answers, their responses will be coded as 88. As such, in each of the 9 question columns in `xl.csv`, there are 10 unique values: `1`, `2`, `3`, `4`, `5`, `6`, `7`, `77`, `88`, and a blank value.

Since you're not interested in the `77` and `88` responses, you choose to recode these values as blank.

After recoding , you calculate the mean rating at the client level for each of the 9 questions so you can compare the client ratings side by side. For example, the following table format shows how the 9 clients are rated on the `comimp` question:

Client           | comimp
-----------------|-------------------
Arlington 14S    | 6.5151515151515156
Boylston 14S     | 5.4745762711864403
Clarendon 14S    | 5.7875647668393784
Dartmouth 14S    | 6.2045454545454541
Kenmore 14S      | 5.7307692307692308
Marlborough 14S  | 4.7705627705627709
Peabody 14S      | 5.8947368421052628
Roxbury 14S      | 6.2884615384615383
Tremont 14S      | 5.7445652173913047

## Task 1
Read file `xl.csv`, look at the 9 question columns above, and recode any `77` and `88` values as blank. After recoding, calculate the mean rating for each funder for each of the 9 questions. Save your output as `mean.csv`. You may refer to `exercise1/output/mean.csv` for a preview of how the output file should look like.

*Hint*: If you're familiar with `pandas`, the following line should help:
```python
# data is the recoded DataFrame with the 9 question columns
# that you're interested in
mean = data.groupby('fdntext').mean()
```

## Task 2
You're also interested in knowing more about the descriptive statistics of the client ratings; you want to know the highest, lowest, median, and quartile ratings for each of the 9 questions. Generate a CSV file `stats.csv` that will include these descriptive statistics. Refer to `exercise1/output/stats.csv` for a preview of how the output file should look like.

*Hint*: Using `pandas`, the following line should help:
```python
# mean is the DataFrame generated from Task 1
stats = mean.describe()
```
