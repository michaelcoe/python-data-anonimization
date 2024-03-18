---
title: Terminology
teaching: 15
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Be able to determine what identifiable information is.
- Be able to determine what quasi-identifiable information is.
- Be able to determine what sensitive attributes is.
- Be able to determine what insensitive information is.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How do I classify specific categories of data?

::::::::::::::::::::::::::::::::::::::::::::::::::

Before diving into [Arx-Data Anonymization Tool][Arx-software] and anonymizing data, we need to get familiar with what type of information we want to anonymize. In general, there are four categories of information we are concerned with:

- Identifiable information: information in a database that allow for the identification of an individual (e.g. name, ID number, email...).
- Quasi-identifiable information: information that seem not to be relevant information, but when combined make it possible to identify an individual (e.g. gender, age, city, etc.).
- Sensitive attributes: information that hold private data that must remain private and cannot be extracted.
- Insensitive information: information that cannot be used to indentify an individual.

Lets look at an example data set to illustrate these types of information. First we'll load and see what the data looks like by using pandas and python. Please note that all of the entries in the example data set were made with the python package [Faker][python-faker]. None of the data here represents a real person. The data was also tailored to New Zealand.

```python
import pandas as pd

voter_data = pd.read_csv('data/synthetic_voter_data.csv')
print(voter_data.head())
```

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

## Indentifiable Information

**Identifiable information** is information in that lets you identify a person with no other pieces of data. This type of data can be a name, Email, address, ID number, etc.

:::::::::::::::::::::::::::::::::::::::  challenge

## Identifying Indentifiable Information

In the data set above, which columns represent identifiable information. That is data that can be used to identify a person.

:::::::::::::::  solution

## Solution

- The Surname and First Name
- The Email address of the person can be traced back to them

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::

## Quasi-indentifiable Information

**Quasi-identifies** allow linking with other data sets. It is assumed that the data holder knows which attributes are quasi-identifiers. In practice, this can be difficult since this depends on all the other data available. This data seems to not show relevant information at first, but when combined it can be used to indentify a person. Important here is that this type of information includes multiple columns of data. Examples of these combined data is: gender, age, city, etc.

:::::::::::::::::::::::::::::::::::::::  challenge

## Identifying Quasi-Indentifiable Information

In the data set above, which columns combined can be considered quasi-identifiable information. Remember that this will be a combination of multiple columns.

:::::::::::::::  solution

## Solution

- Age, Gender, Postcode
- Age, Job Title, Company
- Age, Party Choice, Postcode
- Gender, Company, Salary

As the above shows, multiple combinations of columns can be considered quasi-indentifiable.

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::

## Sensitive Attributes

**Sensitive Attributes** are columns that hold private information that must remain private and cannot be extracted. These are attributes that shouldn't be linkable to an individual. The problem with these attributes is determining what is sensitive depends on the context.

:::::::::::::::::::::::::::::::::::::::  challenge

## Identifying Sensitive Attributes

In the data set above, which columns hold sensitive attributes.

:::::::::::::::  solution

## Solution

Sensitive attributes are dependent on the data holder. In this case, the Party Choice column is likely a sensitive attribute and should not be linked.

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::

## Insensitive Information

**Insensitive Information** is information that is neither sensitive, identifiable, or quasi-identifiable. These do not need to be anonymized.

:::::::::::::::::::::::::::::::::::::::  challenge

## Identifying Insensitive Information

In the data set above, which columns would be considered insentivie information.

:::::::::::::::  solution

## Solution

This is sort of a trick question. Insensitive information is a bit subjective and none of the information provided in the practice data set can be considered insensitive. In general, everything should be considered to be a quasi-identifier, but it will reduce the quality of the data.

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- **Identifiers** allow for direct identification and should be removed.
- **Quasi-Identifiers** allow identification when linked with other datasets.
- **Sensitive Attributes** are attributes that shouldn't be linkable to an individual.
- **Insensitive Data** is data that can be left alone as it's neither sensitive, identifiable, or quasi-identifiable.

::::::::::::::::::::::::::::::::::::::::::::::::::


