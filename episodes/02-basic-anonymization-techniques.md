---
title: Basic Anonymization Techniques
teaching: 5
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Understand what basic anonymisation techniques are available
- Know the difference between anonymisation and pseudonymisation
- What is the trade-off between anonymisation and data utility

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How would you implement these techniques in Python?
- What is anonymisation and pseudonymisation?
- What is data utility?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Techniques for data anonymisation or pseudonymisation

These techniques will be demonstrated using Python and not ARX software. It is important to be aware of these techniques as a base for more advanced techniques. 

- **Data masking**: Real values such as words or characters are swapped for fake information.
- **Randomization**: Removing identifiers from a data set and replacing them with random data
- **Aggregations**: Combining data from multiple individuals to create a group or interval.
- **Perturbation**: Modifying data by adding random noise to the dataset.
- **Synthetic Data**: Replacing data with artificial data that closely mirrors the actual data.

## Anonymisation versus pseudonymisation

There is an important distinction between **Anonymisation** and **Pseudonymisation**.

- **Anonymisation**: The process of removing all personal identifiers from a data set and ensuring that an individual can not be identified with the data that remains.
- **Pseudonymisation**: Changing personal identifiers in such a way that a direct association cannot be made unless additional information in the data set is used.

The handling of data under these two techniques is made specifically in the EU's General Data Protection Regulation (GDPR). According to the GDPR Recital 26 [^1], concerning pseudonymization:

> Personal data which have undergone pseudonymisation, which could be attributed to a natural person by the use of additional information should be considered to be information on an identifiable natural person.

whereas data that is anonymised [^1]:

> The principles of data protection should therefore not apply to anonymous information, namely information which does not relate to an identified or identifiable natural person or to personal data rendered anonymous in such a manner that the data subject is not or no longer identifiable.

So **pseudonymised** data is still subject to GDPR data protection laws, whereas **anonymised** data is not. To understand the difference in these two techniques, we'll take the first row of the data set presented in [Terminology](01-terminology.md) lesson.

```output

    Surname     First Name    Gender     Age     \
0   Matheson    Alison        Female     39

    Party Choice   Postcode    Email                    \
0   NZ First       6418        thomas76@yahoo.com

    Job Title             Company         Salary
0   Quantity surveyor     Barnes LLC      40000
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Which data do we need to Anonymise, Pseudonymise

In the above data set, which is the personal identifiable information?

:::::::::::::::  solution

## Solution

Matheson, Alison, thomas76\@yahoo.com.

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::

We use the data masking technique, explored in the future [Data Masking](03-masking.md) lesson. We get the following pseudonymous data, shown here with the original data set for clarity.

```output

                 Original Data       Pseudononymous Data
                  
    Surname        Matheson                p1NhhSM
    First Name     Alison                  BDIDs7d
    Gender         Female                  Female
    Age            39                      39
    Party Choice   NZ First                NZ First
    Postcode       6418                    6418
    Email          thomas76@yahoo.com      GaT7n7Y@yahoo.com
    Job Title      Quantity surveyor       Quantity surveyor
    Company        Barnes LLC              Barnes LLC   
    Salary         40000                   40000
                             
```

We see that the **Surname**, **First Name**, and **Email** cannot identify the person, but there is still data in that can be used to identify the person, remembering the previous discussion on **quasi-identifiers**. 

:::::::::::::::::::::::::::::::::::::::  challenge

## Which data do we need to Anonymise, Pseudonymise

In the above Pseudonymous data set, which pieces of data, or quasi-identifiers, could be used to identify the person? 

This is assuming that the entity re-identifying the person has access to other data sets, for instance, voter registration.

:::::::::::::::  solution

## Solution

- Age: 39, Postcode: 6418, and Gender: Female.
- Age: 39, Party Choice: NZ First, and Postcode: 6418
- Gender: Female, Company: Barnes LLC, Salary: 40000
- etc.

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::

To anonymise the above data set, we need to completely erase the identifying data and use techniques learned later, such as, [Aggregation](06-aggregation.md) and [Pertubation](07-pertubation.md). The output of this is gives:

```output

                 Original Data          Anonymised Data
                  
    Surname        Matheson                
    First Name     Alison                  
    Gender         Female                  Female
    Age            39                      25-44
    Party Choice   NZ First                Right Leaning
    Postcode       6418                    6418
    Email          thomas76@yahoo.com      
    Job Title      Quantity surveyor       Quantity surveyor
    Company        Barnes LLC              Barnes LLC   
    Salary         40000                   31-50K
                             
```

As we've seen so far. We can make a dataset more anonymous, but we a lot of the utility of the data. In this case, utility means the type of analysis we can do on the data after anonymisation. This is known as the privacy-data utility trade off. 

:::::::::::::::::::::::::::::::::::::::: keypoints

- There is a big difference between anonymisation and pseudonomysation.
- With legacy anonymisation techniques, there is a trade-off between anonymisation and data utility.

::::::::::::::::::::::::::::::::::::::::::::::::::


[^1]: https://www.privacy-regulation.eu/en/recital-26-GDPR.htm
