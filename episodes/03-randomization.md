---
title: Randomization
teaching: 10
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objectives

- Explain how to randomize identifiable data.
- Implement randomization in python.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Can you identify the person after randomization is applied?
- What do you need to do to the data to randomize it?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Replacing columns with random data.

In lesson [Terminology](01-terminology.md), we started with the following dataset:

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

and we identified that the **Surname, First Name, and Email** were pieces of identifiable information. A fairly rudimentary form of anonymization is to perform what is called randomization on these columns. To accomplish this in Python, we can make use of the `random.sample()` method. This method will randomly sample a string of characters a set amount of times. In practice, the following method will replace the columns **Surname** and **First Name** with a 7 element string of alphanumeric characters. 

```python
def randomize_names(df):
    copy_df = df.copy(deep=True)
    for column in ['Surname', 'First Name']:
        column_text = copy_df[column].tolist()
        copy_df[column] = [''.join(random.sample(column_text[i], 
                          len(column_text[i]))) for i in range(len(column_text))]
    
    return copy_df
```

Running the dataframe through this function yields the following output:

```output

   Surname     First Name   Gender    Age      \
0  thoeanMs    nlosiA       Female    39
1  PrhencsoM   alitCin      Female    19
2  Robb        oVtiarci     Female    31
3  tsaMwthe    bebDei       Female    51
4  Wecalla     usSan        Female    67 


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

From the above output, we can see that the **Surname** and **First Name** is a mixed set of characters. The problem with this type of anonymisation is that the names can be readily found even through they are randomly sampled. This is particularly true since the string still has capital letters. We can force all the characters to be lowercase by using the python string method `lower()`, but the name can still be easily found.

:::::::::::::::::::::::::::::::::::::::  challenge

## Identifying Indentifiable Information

Can you determine the Surname and First Name of the 1st row of the above randomized data set?

:::::::::::::::  solution

## Solution

Caitlin McPherson

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::

In the next section, [Data Masking](04-masking.md), we will go over a more powerful method of de-identifying identifiable data known as Data Masking.

:::::::::::::::::::::::::::::::::::::::: keypoints

- Randomization is a quick and easy tool to mix up identifiable data, but is not very effective at anonymisation.

::::::::::::::::::::::::::::::::::::::::::::::::::
