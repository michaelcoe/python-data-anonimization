---
title: Data Masking
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

and we identified that the **Surname, First Name, and Email** were pieces of identifiable information. The first step to anonymization is to perform what is called data masking on these columns.

:::::::::::::::::::::::::::::::::::::::  challenge

## Identifying Indentifiable Information

The goal of data masking is the replace the columns data with randomly generated data, thus, masking what the original data was. Would it be sufficient to just randomize the names by mixing up with strings at random?

:::::::::::::::  solution

## Solution

While this can work in theory, with an output at follows:

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::

To accomplish this in Python, we can make use of the `random.choices()` method. This method will pick elements at random up to a value of k=N, where N is the number of elements desired. In practice, the following method will replace the columns **Surname** and **First Name** with a 7 element string of alphanumeric characters. 

```python
def mask_names(df):
    copy_df = df.copy(deep=True)
    for column in ['Surname', 'First Name']:
        copy_df[column] = [''.join(random.choices(string.ascii_letters + 
                           string.digits, k=7)) for _ in range(copy_df.shape[0])]
    
    return copy_df
```

Running the dataframe through this function yields the following output:

```output

   Surname    First Name  Gender    Age      \
0  p1NhhSM    BDIDs7d     Female    39
1  clRmpmY    nUafmQm     Female    19
2  4Porjry    vUH5pvR     Female    31
3  NeGrucW    lY1ZAvn     Female    51
4  Du58Gi0    Fpus1ub     Female    67 


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

In the above, the call to 

```python
''.join(random.choices(string.ascii_letters + 
                           string.digits, k=7))
```

creates a string of alphanumeric characters, and the `for loop` loops through every row of the dataframe. 

This is nice, but we've now lost the ability to differentiate between some of the rows. For example, we could have two 39 year hold females working at the same company with the same job description and we cannot tell them apart. To correct for this, we can easily add a Unique ID column with the following code snippet:

```python
  df['UniqueID'] = np.arange(1001,1001+df.shape[0], 1)
```

```output

   Surname    First Name  Gender    Age      \
0  p1NhhSM    BDIDs7d     Female    39
1  clRmpmY    nUafmQm     Female    19
2  4Porjry    vUH5pvR     Female    31
3  NeGrucW    lY1ZAvn     Female    51
4  Du58Gi0    Fpus1ub     Female    67 


   Party Choice  Postcode    Email                 \
0  NZ First      6418        thomas76@yahoo.com
1  National      8213        anthonydavies@pritchard-barnett.maori.nz
2  ACT           8869        hunter97@gmail.com
3  ACT           9785        laurensonpeter@xtra.co.nz
4  NZ First      8686        matthew87@hope-smith.ac.nz 


   Job Title                     Company                        Salary   UniqueID
0  Quantity surveyor             Barnes LLC                     40000    1001
1  Illustrator                   Cook, Pratt and Peters         175000   1002
2  Banker                        McDonald, Thompson and Kelly   195000   1003
3  Mining engineer               Freeman-Barnett                254000   1004
4  Industrial/product designer   Cresswell-Smith                225000   1005
```

Here we are adding a Unique ID starting at 1001 and increasing by 1 each row. This UniqueID can can be recorded in a private data file that then corresponds to each participants **Surname** and **First Name**. A keen observer would see that we still have a column of identifiable data in the Email address of each participant. Like the previous two columns, we can use data masking for the Email as well in the following method:

```python
def randomize_emails(df):
    copy_df = df.copy(deep=True)
    new_emails = []
    for entry in df['Email']:
        domain = entry.split("@")[1]
        new_emails.append(''.join(random.choices(string.ascii_letters + 
                          string.digits, k=7)) + '@' + domain)

    copy_df['Email'] = new_emails

    return copy_df
```

```output

   Surname    First Name  Gender    Age      \
0  p1NhhSM    BDIDs7d     Female    39
1  clRmpmY    nUafmQm     Female    19
2  4Porjry    vUH5pvR     Female    31
3  NeGrucW    lY1ZAvn     Female    51
4  Du58Gi0    Fpus1ub     Female    67 


   Party Choice  Postcode    Email                 \
0  NZ First      6418        GaT7n7Y@yahoo.com
1  National      8213        uevPWGu@pritchard-barnett.maori.nz
2  ACT           8869        lxTTdoM@gmail.com
3  ACT           9785        qqIiDkx@xtra.co.nz
4  NZ First      8686        2JO8exc@hope-smith.ac.nz 


   Job Title                     Company                        Salary   UniqueID
0  Quantity surveyor             Barnes LLC                     40000    1001
1  Illustrator                   Cook, Pratt and Peters         175000   1002
2  Banker                        McDonald, Thompson and Kelly   195000   1003
3  Mining engineer               Freeman-Barnett                254000   1004
4  Industrial/product designer   Cresswell-Smith                225000   1005
```

This method is slightly different than the one for names in that we have to split the Email into two parts using the `@` symbol as the split character. The first part of the email is the address and the second part is the domain. The code then generates a random string of 7 characters for the address as before and adds the domain back onto the string to make the complete e-mail address. In some instances, even having the domain might count as a quasi-identifier when given the rest of the information in the dataframe.

:::::::::::::::::::::::::::::::::::::::: keypoints

- Data Masking is a powerful tool to get get rid of identifiable data.

::::::::::::::::::::::::::::::::::::::::::::::::::


