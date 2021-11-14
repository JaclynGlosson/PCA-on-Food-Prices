# Principle Component Analysis on Food Prices

The objective if this project was to

### Readme Table of Contents
* [Exploratory Analysis](...)
* [Defining the Problem in terms of PCs](.)
* [Values of the PCs](.)
* [Variance Estimate Checks](.)
* [Visual Plots and Interpretation](.)
* [Use a Regression](.)
* [Standardize Variables and Repeat Analysis](.)
* [Use a Regression](.)

## Exploratory Analysis
This data provides food prices (in cents) per pound in different US cities in May of 1978. It was collected by the U.S. Department of Labour Bureau of Labor Statistics. There are 24 different cities with 5 different food categories, and there are no missing data. On average, Butter is the most expensive food (mean 144.2), and Bread is the least (Mean 38.44). 

<b><br> Correlations <br></b>
Bread and Hamburger prices are highly correlated (.649), and Tomato and Hamburger prices are the second most correlated (.556). Apples and Tomato are the least correlated (.133).The high correlation between Bread and Hamburger could be because they are often paired together at meals, and therefore often sold together. Their supply and demand fluctuations may be related because of this, and therefore their prices would be. Note that there are no negative correlations in this data set. In general, above average food prices are associated with each other, likely due to relatively similar costs (compared with non perishable, nonfood items) in production, transportation, and distribution. Correlations are important for PC, as the “greater the correlation among the variables the greater the data reduction we can achieve” (Subhash Sharma, Pg 76). Note that most variables have moderate correlations, therefore I should be able to achieve a moderate amount of data reduction. 

<b><br> Covariance and Variance <br></b>
The covariance matrix reveals that Hamburgers have the largest price variation (135.33) in prices. Butter has the second largest variation (85.14). Bread and Apples have similar variation (70; 69.87), and Tomato has the least variation (54.74). Added together, the sum of all variance in the data is 415.08. <b> In  terms of the percent of variation each variable accounts for in the data, Hamburger accounts for the largest percent (32.6%) of the total variance in the data, while Bread (16.8%) and Apples (16.8%) have nearly identical percentages, and Tomato has the lowest (13%).</b> These variances are particularly important to examine when doing PCA, because I expect those with the highest variation percent to be eigenvalues of high magnitude on the first or second PC. Covariance is the unnormalized measure of the joint variability of two variables, and is closely related to correlation. As expected as demonstrated from our correlation matrix, Bread and Hamburgers have the largest covariance (63.17), Tomato and Hamburger have the second (47.83), and Tomato and Apples have the least (8.25). This mirrors the patterns found in the correlation matrix, since correlation is computed with covariance. In terms of the cities themselves, the city of Anchorage has the highest total food prices, followed by Honolulu and Philadelphia. Buffalo has the lowest total food prices, followed by San Diego and Los Angeles.

![Covariance and correlations](https://github.com/JaclynGlosson/PCA-on-Food-Prices/blob/512c301d601c381c8a4d79a0de008a7234e10567/images/image47.png)

## Defining the Problem in terms of PCs
Principle Component Analysis is used to “explain the variance-covariance structure of a set of variables." It is considered a multidimensional data reduction technique. The number of PCs will be equal to the number of variables, in this case, 5. PCs are uncorrelated to each other, and their variances are as large as possible. PCA can be especially helpful when there is multicollinearity in the data. <b>There is multicollinearity in the current data</b>, as transportation, gas prices, subsidies, and seasons can all simultaneously affect food prices. PCs explain variation in descending order, such that the first PC “explains the maximum amount of variance possible with a linear combination”.  PCs are calculated using Eigenvalues and Eigenvectors, which are computed using the covariance matrix. 

* Eigenvalues are calculated using the covariance matrix in the formula | ∑ - λI | = 0, where: ∑ is the covariance matrix. 
* Eigenvectors are calculated using both the covariance matrix and a single Eigenvalue, such that ∑x = λ(k)x, where x =eigenvector. 
* The principle component formulas are as follows, where Y1 is the first principle component, and a11….a15, the eigenvectors, are as large as possible, subject to √(a112 + a122  +...+ a152 ) =1
* Y1=a11\*Bread+a12\*Hamburger+a13\*Butter+a14\*Apples + a15\*Tomato
* Y2=a21\*Bread+a22\*Hamburger+a23\*Butter+a24\*Apples + a25\*Tomato
* Y3=a31\*Bread+a32\*Hamburger+a33\*Butter+a34\*Apples + a35\*Tomato
* Y4=a41\*Bread+a42\*Hamburger+a43\*Butter+a44\*Apples + a45\*Tomato
* Y5=a51\*Bread+a52\*Hamburger+a53\*Butter+a54\*Apples + a55\*Tomato

The variance of Hamburgers was the largest in the dataset. As such, I expect that, being the first PC and having the most variation, the absolute value of a12 will be a large number, in order to magnify the variable with the largest variance. Using R, I calculate the eigenvalues and eigenvectors such that:

![eigenvectors and values](https://github.com/JaclynGlosson/PCA-on-Food-Prices/blob/512c301d601c381c8a4d79a0de008a7234e10567/images/image20.png)

Note that the columns correspond to the eigenvalues (PC1) and the rows correspond to the eigenvectors for that eigenvalue. Eigenvalues demonstrate the share of importance of the PC relative to the other PCs. The sum of all Eigenvalues is equal to 415.08, the sum of all variances. The first eigenvalue is almost 3 times as big as the second, so <b>the first PC explains three times as much of the variance of the data as the second PC.</b> More specifically, the first eigenvalue explains 52.2% of the data variation (calculated from 216.79/415.08), the second eigenvalue explains 19% of the data variation, and the third explains 14.9% of the data variation. Together, these first three PCs account for 86.1% of the data variation. As this meets my (flexible) 80% threshold for variation explanation, and as I want to explain with as few numbers as possible, I can drop the fourth and fifth components. If this data had a larger amount of observations, I would certainly use all three PCs. However, because this dataset is relatively small and I am trying to explain it in as few PCs as possible, for the analysis in this paper I will focus on the first two PCs, which consist of 61.2% of the data variance.  

<b>PC 1</b>
For PC1, E12 corresponds with Hamburgers, which had the largest variation. As expected, the absolute value of this eigenvector (-.071) is very large, since we want to magnify the variable with the largest variance on the first PC, which will account for the largest percent of variation. Note that Bread did not have the second highest variance- Butter had the second largest variance. However, Bread and Hamburger had the highest correlation (.649), and therefore the highest covariance. Therefore it makes sense they load together on PC1. Hamburger’s second highest correlation was with Tomato, so it makes sense that Tomatoes have the third highest magnitude in PC1. 

<b>PC 2</b>
For PC2, we see that Butter’s eigenvalue has the greatest magnitude, since Butter had the second most variation in the dataset. Though Butter’s highest correlations are with Tomato and Bread, the variation of these variables were already captured in PC1. PC2’s second highest magnitude is with Hamburgers, which makes sense because it accounts for most of the variation in the data. Note that Bread and Hamburgers are positive, unlike the other eigenvalues in PC2.

![](https://github.com/JaclynGlosson/PCA-on-Food-Prices/blob/6dc29ae280e876011f4977d80335b55f5162c18b/images/image17.png)







