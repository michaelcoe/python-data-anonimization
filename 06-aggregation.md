---
title: Aggregation
teaching: 10
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objectives

- Explain how to mask identifiable data.
- Implement data masking in python.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Can you identify the person after data masking is applied?
- What do you need to do to the data when using data masking?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Aggregating Numeric Data.

Aggregation is the practice of combining the data from multiple individuals to create group-level views of the data. So instead of storing each value individually, we convert the data in ranges or groups.

As before, we will start with the dataset given in lesson [Terminology](01-terminology.md):

```output

    Surname     First Name    Gender     Age     \
0   Matheson    Alison        Female     39
1   McPherson   Caitlin       Female     19
2   Robb        Victoria      Female     31
3   Matthews    Debbie        Female     51
4   Wallace     Susan         Female     67


    Party Choice   Postcode    Email                    \
0   NZ First       6418        thomas76@yahoo.com	   
1   National       8213        anthonydavies@pritchard-barnett.maori.nz
2   ACT            8869        hunter97@gmail.com	  
3   ACT            9785        laurensonpeter@xtra.co.nz 
4   NZ First       8686        matthew87@hope-smith.ac.nz	


    Job Title                       Company                       Salary
0   Quantity surveyor               Barnes LLC                    40000
1   Illustrator                     Cook, Pratt and Peters        175000
2   Banker                          McDonald, Thompson and Kelly  195000
3   Mining engineer                 Freeman-Barnett               254000
4   Industrial/product designer     Cresswell-Smith               225000
```

The above dataset has two quasi-identifiable columns that we can use aggregation on.

:::::::::::::::::::::::::::::::::::::::  challenge

## Which Data to Aggregate

In the above dataset, which columns can be readily aggregated?

:::::::::::::::  solution

## Solution

- Age, Salary

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::

Data aggregation in python is fairly straight forward when using pandas. We can use the pandas method `cut` to accomplish this. The relevant snippet of python code is as follows:

```python
def aggregate_age(df):
    copy_df = df.copy(deep=True)
    bins = [18, 25, 44, 60, 75, 90, 100]
    labels = ['18-24', '25-44', '45-60', '60-75', '75-90', '90+']
    copy_df['Age'] = pd.cut(copy_df['Age'], bins=bins, labels=labels)

    return copy_df
```

Running the code specifies cutoffs for the data in the **bins** variable and will replace the data therein with the corresponding **labels**. The following is the output of the above data snippet:

```output

   Surname     First Name   Gender    Age      \
0  Matheson    Alison       Female    22-44
1  McPherson   Caitlin      Female    18-24
2  Robb        Victoria     Female    25-44
3  Matthews    Debbie       Female    45-60
4  Wallace     Susan        Female    60-75


   Party Choice  Postcode    Email                 \
0  NZ First      6418        thomas76@yahoo.com
1  National      8213        anthonydavies@pritchard-barnett.maori.nz
2  ACT           8869        hunter97@gmail.com
3  ACT           9785        laurensonpeter@xtra.co.nz
4  NZ First      8686        matthew87@hope-smith.ac.nz 


   Job Title                     Company                        Salary
0  Quantity surveyor             Barnes LLC                     40000
1  Illustrator                   Cook, Pratt and Peters         175000
2  Banker                        McDonald, Thompson and Kelly   195000
3  Mining engineer               Freeman-Barnett                254000
4  Industrial/product designer   Cresswell-Smith                225000
```

We can see that the **Age** column has been made such that each individual fits into a larger group. This makes it difficult to determine what the overall age of the individual. Similar to **Age**, we can put the **Salary** into groupings as well.

:::::::::::::::::::::::::::::::::::::::  challenge

## Appropriate Bins

What are some appropriate groupings to put the **Salary** column in?

:::::::::::::::  solution

## Solution

- This is really up to user choice, but in the following case we will use:

['0-30K', '31-50K', '51-80K', '81-100K', '101-250K', '250K+']

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::

```python
def aggregate_salary(df):
    copy_df = df.copy(deep=True)
    bins = [0, 30000, 50000, 80000, 100000, 250000, 1000000]
    labels = ['0-30K', '31-50K', '51-80K', '81-100K', '101-250K', '250K+']
    copy_df['Salary'] = pd.cut(copy_df['Salary'], bins=bins, labels=labels)

    return copy_df
```

```output

   Surname    First Name  Gender    Age      \
0  Matheson    Alison       Female    22-44
1  McPherson   Caitlin      Female    18-24
2  Robb        Victoria     Female    25-44
3  Matthews    Debbie       Female    45-60
4  Wallace     Susan        Female    60-75


   Party Choice  Postcode    Email                 \
0  NZ First      6418        thomas76@yahoo.com
1  National      8213        anthonydavies@pritchard-barnett.maori.nz
2  ACT           8869        hunter97@gmail.com
3  ACT           9785        laurensonpeter@xtra.co.nz
4  NZ First      8686        matthew87@hope-smith.ac.nz 


   Job Title                     Company                        Salary
0  Quantity surveyor             Barnes LLC                     31-50K
1  Illustrator                   Cook, Pratt and Peters         101-250K
2  Banker                        McDonald, Thompson and Kelly   101-250K
3  Mining engineer               Freeman-Barnett                250K+
4  Industrial/product designer   Cresswell-Smith                101-250K
```
As the examples show, aggregation is a fairly easy method to anonymise quasi-identifiable data. Important here is that you can further aggregate the **Job Title** column by generalizing the job title to a larger group. For example, you can make anything with ***engineer*** in it to a group that's named ***engineering***.

:::::::::::::::::::::::::::::::::::::::: keypoints

- Aggregating is a powerful way to anonymise data that can be grouped or combined.
- Aggregation is easily done within the pandas python package.

::::::::::::::::::::::::::::::::::::::::::::::::::
