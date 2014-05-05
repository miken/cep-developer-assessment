cep-developer-assessment
========================

This assessment evaluates a candidate's software development skills relevant to CEP's work. Besides efficiency, CEP also looks for clarity in code submissions.

The assessment has three exercises that test a candidate's ability to manipulate CSV data and write JSON outputs using existing Python packages.

We recommend that the candidate has familiarity with statistics and the following Python packages: `numpy`, `scipy`, and `pandas`.

* [Instructions](#instructions)
* [Exercise 1](#exercise-1)
  * [Task 1](#task-1)
  * [Task 2](#task-2)
* [Exercise 2](#exercise-2)
* [Exercise 3](#exercise-3)

If you have any questions regarding the assessment, please feel free to reach out to [Mike Nguyen](https://github.com/miken) at miken@effectivephilanthropy.org.

For more information about CEP and our work, please visit www.effectivephilanthropy.org

# Instructions
For each exercise folder, there are three subfolders within:

* `input`: This folder contains all input files to be read by your program
* `output`: This folder contains "model" output files that your program ought to produce. For example, in exercise 1, there are two files in the `output` folder: `mean.csv` and `stats.csv`. Your program is expected to create these two files in the exact format and layout.
* `submissions`: This is where you can save your program in the format `your_name.py`

To submit your codes, first, fork this repo, add your own program files in the `/submissions` folder for each exercise, then email Mike Nguyen (miken@effectivephilanthropy.org) with a link to your GitHub forked repo.

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

**Hint**: If you're familiar with `pandas`, the following line should help:
```python
# data is the recoded DataFrame with the 9 question columns
# that you're interested in
mean = data.groupby('fdntext').mean()
```

## Task 2
You're also interested in knowing more about the descriptive statistics of the client ratings; you want to know the highest, lowest, median, and quartile ratings for each of the 9 questions. Generate a CSV file `stats.csv` that will include these descriptive statistics. Refer to `exercise1/output/stats.csv` for a preview of how the output file should look like.

**Hint**: Using `pandas`, the following line should help:
```python
# mean is the DataFrame generated from Task 1
stats = mean.describe()
```

# Exercise 2
Next, given the set of 9 ratings per question, you're interested in finding out the [percentile](http://en.wikipedia.org/wiki/Percentile) of each client rating. For example, the following table shows the client ratings and their percentiles for the `comimp` question.

Client           | comimp             | comimp_pct  |     
-----------------|--------------------|-------------|
Arlington 14S    | 6.5151515151515156 | 94.44444444 |
Boylston 14S     | 5.4745762711864403 | 16.66666667 |
Clarendon 14S    | 5.7875647668393784 | 50          |
Dartmouth 14S    | 6.2045454545454541 | 72.22222222 |
Kenmore 14S      | 5.7307692307692308 | 27.77777778 |
Marlborough 14S  | 4.7705627705627709 | 5.555555556 |
Peabody 14S      | 5.8947368421052628 | 61.11111111 |
Roxbury 14S      | 6.2884615384615383 | 83.33333333 |
Tremont 14S      | 5.7445652173913047 | 38.88888889 |

Given the `mean.csv` file that you have generated in Exercise 1, now stored in `exercise2/input/mean.csv`, generate a CSV file `pct.csv` that stores the percentile ratings for each client for the 9 survey questions. Refer to `exercise2/output/pct.csv` for a preview of how the output file should look like.

**Hint**: `scipy` has a function called [percentileofscore](http://docs.scipy.org/doc/scipy-0.7.x/reference/generated/scipy.stats.percentileofscore.html) that should make percentile calculation easy:
```python
from scipy.stats import percentileofscore

# If you have the list of 9 client ratings stored in rating_list
# you can do the following loop:
percentile_list = []
for r in rating_list:
    percentile = percentileofscore(rating_list, r, kind='mean')
    percentile_list.append(percentile)
    
# Now you have 9 percentile ranks for a given question
# stored in percentile_list
```

# Exercise 3
At CEP, we deliver results to clients using an online reporting platform. This platform reads client data from a JSON file and renders these data points as charts and objects. 

You're now interested in creating a JSON file to render a report for the client Tremont 14S. A JSON file for Tremont 14S, with only one percentile chart in report for the question `sample_question` may look as follows:
```
{
    "version":"1.0",
    "reports":
    [
        {
            "name":"Tremont 14S Report",
            "title":"Tremont 14S Report",
            "cohorts": [],
            "segmentations": [],
            "elements":{
                "sample_question":{
                    "type":"percentileChart",
                    "absolutes":[5, 6, 6, 6, 7],
                    "current": {
                        "name": "2014",
                        "value": 6.09,
                        "percentage": 42
                    },
                    "cohorts": [],
                    "past_results":[],
                    "segmentations": []
                }
            }
        }
    ]
}

```
Here's an explanation for all the keys in the JSON object above:
* `version`: accepts a string value. You may input "1.0" here.
* `reports`: accepts a JSON array of report objects. Each report object contains the following keys:
  * `name`: accepts a string value for the name of the report. You may use "Tremont 14S Report"
  * `title`: accepts a string value for the name of the report. You may use "Tremont 14S Report"
  * `cohorts`: accepts a JSON array of strings. For simplicity's sake, you may input an empty array here
  * `segmentations`: accepts a JSON array of strings. For simplicity's sake, you may input an empty array here
  * `elements`: accepts a JSON object. This object contains all relevant data points for the reports. Each key represents the question column name and its corresponding value is a chart object. The chart object contains the following keys:
    * `type`: accepts a string value. You may use "percentileChart"
    * `absolutes`: accepts an array of five numeric values. These values represent the 0th, 25th, 50th, 75th, and 100th percentile values for the given question
    * `current`: accepts a JSON object. This object has three keys:
      * `name`: accepts a string value. You may use "2014"
      * `value`: accepts a numeric value. This is the mean rating for Tremont 14S for the question `sample_question`.
      * `percentage`: accepts a numeric value. This is the percentile rating for Tremont 14S for the question `sample_question`.
    * `cohorts`: accepts a JSON array of strings. For simplicity's sake, you may input an empty array here
    * `past_results`: accepts a JSON array of strings. For simplicity's sake, you may input an empty array here
    * `segmentations`: accepts a JSON array of strings. For simplicity's sake, you may input an empty array here

For this exercise, you will read the three files produced in exercises 1 and 2: `mean.csv`, `stats.csv`, and `pct.csv`. These files are saved in the `exercise3/input/` folder as well.

From these 3 files, you can extract the following data points for Tremont 14S:
* `mean.csv`: Mean ratings for Tremont 14S on the 9 questions.
* `pct.csv`: Percentile ratings for Tremont 14S on the 9 questions.
* `stats.csv`: The quartile values for the set of 9 client ratings on the 9 questions.

Using these data points, create 9 percentile chart objects under the `elements` key of the JSON object above. Refer to `exercise3/output/output.json` for a preview of how the output file should look like.

**Hint**: The hint for this exercise is buried deep somewhere within this repository. If you're familiar with GitHub, you might be able to find it.

Good luck! :pray:
