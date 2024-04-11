---
title: Attack Vectors
teaching: 15
exercises: 15
---

::::::::::::::::::::::::::::::::::::::: objectives

- Learn about the types of attacks people can use to identify participants in data.
- Learn which types of techniques correspond to which type of attack.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What are the 7 types of attacks to identify participants from data?
- Is there a technique that protects against all attacks?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Definitions

Before going over attack vectors, we must first learn and be reminded of some definitions pertaining to anonymized data sets. The following definitions are taken from Su *et al.* [^1]:

### Identifiers
Attributes that can directly determine personal identities, such as name and identity card

### Quasi-Identifiers
Attributes that can indirectly determine personal identity by associating multiple attributes, such as age and zip code. A quasi-identifier attribute value is a quasi-identifier data.

### Sensitive Attributes
Attributes that are relevant to personal privacy information, such as disease names and medical costs. A sensitive attribute value is a sensitive data.

### Multi-dimensional Data
Data that can be presented in a data table in which each record(row) corresponds to one person and each column to a specific attribute.

### Equivalence Class
A set of anonymized records with the same values for all the quasi-identifier attributes, i.e., all the records in each equivalence class are indistinguishable in terms of their quasi-identifier attributes.

## Attack Vectors

Diaz and Garcia [^2] give a good overview of the common attacks on databases. The following table outlines the 7 types of attacks. 

| Attack | Description |
| ------ | ----------- |
| Linkage | Consists of combining at least two anonymized databases in order to reveal the identity of some individuals present in both. |
| Re-identification | This kind of attacks occurs when the anonymization process is reversed. |
| Homogeneity | Can occur when all the values for a sensitive attribute in an equivalence class are identical. |
| Background Knowledge | In this case, the adversary has some foreknowledge about the target of the attack (e.g. knows some auxiliary information about an individual in the database). |
| Skewness | Can be carried out when there is an infrequent value for a sensitive attribute in the whole database which is extremely frequent in an equivalence class. |
| Similarity | May occur when the values of a sensitive attribute in an equivalence class are semantically similar (although different). |
| Inference | Consists of using data mining techniques in order to extract information from the data. |

## Anonymization Technique Susceptibility

It's important to understand which types of anonymization techniques are susceptible to which type of attack. Again, Diaz and Garcia [^2] provide a nice table that corresponds each technique to an appropriate attack vector.

| Technique                    | Linkage | Re-identification | Homogeneity | Background | Skewness | Similarity | Influence |
| ---------------------------- | :-----: | :---------------: | :---------: | :--------: | :------: | :--------: | :-------: |
| k-anonymity | $\checkmark$ | $\checkmark$ |  |  |  |  |  |
| ($\alpha$,k)-anonymity | $\checkmark$ | $\checkmark$ | $\checkmark$ |  |  |  |  |
| $l$-diversity | | | $\checkmark$ | $\checkmark$ |  |  |  |
| Entropy $l$-diversity |  |  | $\checkmark$ | $\checkmark$ |  |  |  |
| Recursive ($c,l$)-diversity |  |  | $\checkmark$ | $\checkmark$ |  |  | |
| t-closeness |  |  |  |  | $\checkmark$ | $\checkmark$ |  |
| Basic $\beta$-likeness |  |  |  |  | $\checkmark$  |  |  |
| Enhanced $\beta$-likeness |  |  |  |  | $\checkmark$ |  |  |
| $\delta$-disclosure privacy |  |  |  |  | $\checkmark$ |  | $\checkmark$ |

Here we will go into more detail about each technique. Each technique has their own section later in this training for more detail and implementation.

### k-anonymity
A database verifies k-anonymity if each equivalence class of the database has at least k rows. In
other words, for each row of the database, there are at least k-1 indistinguishable rows with respect to the quasi-identifiers. Note that $k \geq 1$ is always verified and the probability of identifying an individual in the database using the quasi-identifiers will be at most 1/k.

### ($\alpha$,k)-anonymity
Given only one sensitive attribute $S$, it is checked if the database is k-anonymous and the frequency of each possible value of $S$ is lower or equal than $\alpha$ in each equivalence class.

### $l$-diversity
In the case of a single sensitive attribute $S$, it is satisfied if for each equivalence class, there are at least $l$ distinct values for $S$. Note that $l \geq 1$ is always verified.

### Entropy $l$-diversity
A database with a single sensitive attribute S verifies this condition if $H(EC) > \log(l)$, for
every equivalence class EC of the database. Note that $H(EC)$ is the entropy of the equivalence class $EC$, defined as:

$$ H(EC) = -\sum_{s \in D} p(EC, s)\log(p(EC,s)),$$

with D the domain of $S$, and $p(EC, s)$ the fraction of records in $EC$ that have $s$ sensitive attribute.

### Recursive $(c,l)$-diversity
The main potential of this technique is that if a value of the sensitive attribute $S$ is
removed from an equivalence class which verifies $(c, l)$-diversity, then $(c, l-1)$-diversity is preserved. For the implementation of this technique, has been used as reference (in order to get the formal definition of the concept). Specifically, suppose there are n different values for a sensitive attribute $S$ in an equivalence class $EC$. Be $r_i \left(i \in \{1, …, n\}\right)$ the number of times that the i-th most frequent value of $S$ appears in $EC$. Then, EC verifies recursive $(c, l)$-diversity for $S$ if $r_1 < c \left(r_1 + r_{l+1} + . . . + r_n\right)$.

### t-closeness
The goal is again similar to that of the two previous techniques. A database with one sensitive
attribute $S$ verifies t-closeness if all the equivalence classes verify it. An equivalence class verifies t-closeness if the distribution of the values of $S$ are at a distance no closer than t from the distribution of the sensitive attribute in the whole database. In order to measure the distance between the distributions, the Earth Mover’s distance (EMD) between the two distributions using the ordered distance is applied for numerical sensitive attributes. For categorical attributes, the equal distance is used.

### Basic $\beta$-likeness and enhanced $\beta$-likeness
In particular, be $P=\{p_1, …, p_n\}$ the distribution of a sensitive attribute $S$ in the whole database and $Q=\{q_1, …, q_n\}$ that of an equivalence class $EC$. Be $\max\left(D\left(P, Q\right)\right) = \max \{D\left(p_i , q_i\right): p_i \in P, q_i \in Q \wedge p_i < q_i\}$, then basic $\beta$-likeness is verified if $\max\left(D\left(P, Q\right)\right) \leq \beta$ and enhanced $\beta$-likeness is verified if $D\left(p_i , q_i\right) \leq \min\{\beta, − \log(p_i)\} \forall q_i \in Q$. In both cases $\beta > 0$. Note that enhanced $\beta$-likeness provides more robust privacy than basic $\beta$-likeness

### $\delta$-disclosure privacy
Considering a database with only one sensitive attribute $S$, be $p\left(EC, s\right)$ the fraction of
records with s as sensitive attribute in the equivalence class EC, $p\left(DB, s\right)$ that for the whole database (DB). Then, $\delta$-disclosure privacy is verified if:

$$\left| \log\left(\dfrac{p\left(EC, s\right)}{p\left(DB, s\right)}\right) \right| < \delta,$$

for every $s \in D$ (with D the domain of S) and every equivalence class $EC$.

## Examples

Let's look at a few examples to illustrate how an attack can be carried out.

### Linkage

Lets assume that you have a data set about certain medical conditions

```output

    Gender     Age    Postcode      Diagnosis
0   Female     39     6418          Meningitis
```

You can't really pick out who this might be just from this data, but suppose you also have access to public voter information with the following entry:

```output

    Surname     First Name    Gender     Age     \
0   Matheson    Alison        Female     39

    Party Choice   Postcode
0   NZ First       6418
```

It is easy to see how the two data sets can be linked.

### Skewness

Lets suppose we have a set of data that tests for a specific virus in the following form:

```output

    Surname     First Name    Postcode   Age    Virus Present    
0   Matheson    Alison        6418       39     Pos              
1   McPherson   Caitlin       8213       19     Pos              
2   Robb        Victoria      8869       31     Pos              
3   Matthews    Debbie        9785       51     Neg
4   Wallace     Susan         8686       67     Neg
```

And we anonymize the data and group by the postcode column to the following table:

```output

    Surname     First Name    Postcode    Age       Virus Present    
0   *           *             6***        28-40     Pos              
1   *           *             8***        18-27     Pos              
2   *           *             8***        28-40     Pos              
3   *           *             8***        > 40      Neg
4   *           *             9***        > 40      Neg
```

Now lets say that a person only knows that Victoria Robb lives in a Postcode that starts with an 8. Because the sensitive attribute of "Virus Present" is ***skewed*** towards Positive, there is a high likelihood that a person from that postcode is positive for the virus. 

### Similarity

In keeping with medical records, suppose we have a data set with the following data:

```output

    Postcode    Age    Salary     Disease
0   6418        39     40000      gastric ulcer
1   8213        19     175000     gastritis
2   8869        31     195000     stomach cancer
3   9785        51     254000     bronchitis
4   8686        67     225000     stomach ulcer
```
Now suppose we anonymize the data as follows and keep with the sensitive attributes **Salary** and **Disease**.

```output

    Postcode    Age     Salary     Disease
0   6***        28-40   40000      flu
1   8***        18-27   175000     gastritis
2   8***        28-40   195000     stomach cancer
3   8***        > 40    225000     stomach ulcer
4   9***        > 40    254000     bronchitis
```

In this example, the Postcode and Age are sufficiently anonymized. Suppose someone knows that Victoria Robb lives in a zip code that starts with 8. We can then know two pieces of sensitive information about Victoria because the Equivalence Classes are similar.

:::::::::::::::::::::::::::::::::::::::  challenge

## Identifying Sensitive Information by Similarity

Given the anonymized dataset above, what are the two sensitive attributes about Victoria Robb that can be identified through similarity.

:::::::::::::::  solution

## Solution

We know that Victoria earns a high salary and that she has problems with her stomach.

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::


:::::::::::::::::::::::::::::::::::::::: keypoints

- No one technique is able to combat all vectors of attack.

::::::::::::::::::::::::::::::::::::::::::::::::::

[^1]: Su, B., Huang, J., Miao, K., Wang, Z., Zhang, X., & Chen, Y. (2023). K-Anonymity Privacy Protection Algorithm for Multi-Dimensional Data against Skewness and Similarity Attacks. Sensors (Basel, Switzerland), 23(3), 1554. https://doi.org/10.3390/s23031554

[^2]: Sáinz-Pardo Díaz, J., López García, Á. A Python library to check the level of anonymity of a dataset. Sci Data 9, 785 (2022). https://doi.org/10.1038/s41597-022-01894-2


