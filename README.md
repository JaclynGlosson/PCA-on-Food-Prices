# Principle Component Analysis on Food Prices

The objective if this project was to

### Readme Table of Contents
* [Exploratory Analysis](https://github.com/JaclynGlosson/PCA-on-Food-Prices#exploratory-analysis)
* [Defining the Problem in terms of PCs](https://github.com/JaclynGlosson/PCA-on-Food-Prices#defining-the-problem-in-terms-of-pcs)
* [Values of the PCs](https://github.com/JaclynGlosson/PCA-on-Food-Prices#values-of-the-pcs)
* [Variance Estimate Checks](https://github.com/JaclynGlosson/PCA-on-Food-Prices#variance-estimate-checks)
* [Visual Plots and Interpretation](https://github.com/JaclynGlosson/PCA-on-Food-Prices#visual-plots-and-interpretation)
* [Use a Regression](https://github.com/JaclynGlosson/PCA-on-Food-Prices#use-a-regression)
* [Standardize Variables and Repeat Analysis](https://github.com/JaclynGlosson/PCA-on-Food-Prices#standardize-variables-and-repeat-analysis)
* [Use a Regression](https://github.com/JaclynGlosson/PCA-on-Food-Prices#use-a-regression-1)

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

I examine values close to the absolute value of 0.5, as shown above. For PC1, a high price of Hamburgers and Bread is associated with a low value of PC1. All values are negative, such that any food value that has a high price will have a low value for PC1. This is akin to a city food price index. For PC2, a high price of Butter is associated with a low value of PC2. For PC3, a high price of Apples is associated with a high value of PC3. Using the formula Y1=a11\*Bread+a12\*Hamburger+a13\*Butter+a14\*Apples + a15\*Tomato, I plug in the eigenvectors for the variable coefficients. The outputs (Y1, Y2,...Y5) for each PC are shown below.

![](https://github.com/JaclynGlosson/PCA-on-Food-Prices/blob/9a089dfa48068c095f42d3394662bc8b105249b0/images/image58.png)

## Values of the PCs
By multiplying the food prices in each city by the eigenvectors in the equation Y1=a11\*Bread+a12\*Hamburger+a13\*Butter+a14\*Apples + a15\*Tomato; I calculate the PC for the city in question. In PC1, a high price of Hamburgers and Bread is associated with a low value of PC1. Anchorage had the lowest PC1, meaning it has the highest price of Hamburgers and Bread. Honolulu has the second lowest PC1 value, meaning it has the second highest prices for Hamburgers and Bread. These values correspond exactly to the original data, as both Honolulu and Anchorage have the highest Bread and Hamburger prices in the data. These high prices could be due to the high price of transportation to import hamburgers and bread to these cities. San Diego has the highest PC1 value, meaning it has the lowest prices for Hamburgers and Bread. We can see from the original data that San Diego had the lowest Hamburger price out of all the data, but there were several other cities with lower Bread prices. However when Bread and Hamburger prices are added up for each city, San Diego has the lowest combined value, therefore this is consistent with the data. It should also be noted that when all food prices are added together for each city, San Diego has the second lowest cost (393.7 in total), just after Buffalo (380.7 in total). As the difference in these two is mainly due to the cost of butter in San Diego, including PC2 in my analysis (price of butter) will account for this. In summary, the PC values match what I expect given the data.

For PC2, a high price of Butter is associated with a low value of PC2. Kansas City has the lowest value of PC2, meaning it has the highest price of Butter. This is consistent with the original data, where Kansas City did have the highest butter prices. Buffalo has the highest value of PC1, meaning it has the lowest price of Butter. This is almost consistent with the original data, as Buffalo had the second lowest price of butter after Milwaukee. 

Even though I will not be fully using PC3 in this analysis, it is somewhat helpful to interpret it. For PC3, a high price of Apples is associated with a high value of PC3. Kansas City has the lowest value of PC3, meaning it has the lowest prices of apples. This is close to the original data, however both Buffalo and Detroit had lower apple prices. Milwaukee has the highest value of PC3, meaning it has the highest price of Apples. This is different from the original data, as Anchorage, Chicago, Dallas, Honolulu, and San Fancisco all had higher prices of Apples than Milwaukee.

There are a few discrepancies in the PCs and original data, with most of them being in PC3, which makes sense as it accounts for less variation of the data than PC1 and PC2. 

## Variance Estimate Checks

![](https://github.com/JaclynGlosson/PCA-on-Food-Prices/blob/187786f8041c233408395b36356198949aa5cff5/images/image42.png)

<b>Property C: Check variance estimates of PCs vs Original X and other properties. </b>The last two PCs (0.08 and 0.05) are weaker than the percent variance explained in both the Apples and Tomato (0.16 and 0.12) variables, therefore they should be excluded.

<b>Property D: eik is the importance of the ith variable Xk to the ith PC. Use cutoff (-0.5 or 0.5) </b>
As 0.5 is an arbitrary cut off, I have examined some eigenvectors that are close to 0.5. Because PC1 explains a larger percent of the variance, I would include the Bread eigenvector in my interpretations, even though it is below the 0.5 threshold.

<b>Property E: eik is proportional to correlation (Xi, Xk), p (Xi,Xk)</b>
PC1: PC1 eigenvectors are indeed proportional to this correlation. The strongest correlation (-0.9) is associated with the most extreme eigenvector (-0.71) in the Hamburger row. The second strongest correlation (-0.79) is associated with the second most extreme eigenvector (-0.45), which is Bread. The weakest correlation (-0.38) is associated with the weakest eigenvectors (-0.22) which is Apples.

![](https://github.com/JaclynGlosson/PCA-on-Food-Prices/blob/187786f8041c233408395b36356198949aa5cff5/images/image35.png)

<b>Property G: Are they all uncorrelated?</b> Yes!

![](https://github.com/JaclynGlosson/PCA-on-Food-Prices/blob/187786f8041c233408395b36356198949aa5cff5/images/image40.png)

## Visual Plots and Interpretation
Control Chart: PC1 against PC2: PC1 is High Priced Hamburger Sandwiches/ High priced food (High priced food is associated with a low X axis value, low priced food is associated with a high X axis value). PC2 is High Priced Butter (High priced butter is associated with a low Y axis value, low priced butter is associated with a high Y axis value) I can see clearly that both Anchorage and Honolulu have high priced hamburger sandwiches, as well as butter. Buffalo has low priced hamburger sandwiches and low priced butter. San Diego and Los Angeles have low priced hamburger sandwiches, but high priced butter.
As discussed previously, the below scree plot demonstrates that Eigenvalues decrease in percent of variance explained (Red) while the variable Hamburgers accounts for the most percent variance in the variables (blue). The percent variance explained (red)is in a consistently sloping line, as expected. Note a large percent variance difference between PC1 and PC2, but a much smaller one between PC2 and PC3, justifying the use of just PC1 and PC2.

![](https://github.com/JaclynGlosson/PCA-on-Food-Prices/blob/187786f8041c233408395b36356198949aa5cff5/images/image36.png)
![](https://github.com/JaclynGlosson/PCA-on-Food-Prices/blob/187786f8041c233408395b36356198949aa5cff5/images/image54.png)

## Use a Regression
The real advantage of PC lies in its dimension reduction ability. When I run a regression between two of the original IVs and a random DV variable, I see that I get a lower R squared (0.765) than if I run a regression between two PCs and the same random DV variable (0.917). Thus, even though I am using less data, I am able to explain a similar amount of variance than if I used all data. By just using the first two (and therefore the ones that most explain the variance) PCs, I can determine that 91.7% of the variance in the data is explained by the regression line.

![](https://github.com/JaclynGlosson/PCA-on-Food-Prices/blob/d4cad23f9055c664e835ab467a7dfd865599ae03/images/image45.png)

# Standardize Variables and Repeat Analysis
There is no reason to believe that one food is more important than another. Because Hamburgers have a much higher variance than other variables, it will receive an “artificially higher weight” in unstandardized data. Given that I am not emphasizing any one food over another, standardizing the data is better for an overall analysis in this data set, especially if I am trying to construct a price index. Standardizing the variables puts the variables in terms of how many standard deviations the data is away from the mean, which is zero, with each standard deviation and variance being equal to one. Since all variances are equal to one, eigenvalues will change in an unstandardized analysis compared to a standardized analysis. Though the correlation matrix is the same as raw data, the covariance matrix has changed, which of course will impact our PCs since Eigenvalues and Eigenvectors are computed using the covariance matrix. The covariance matrix of standardized data is equal to the correlation matrix. Because each variable has a variance of one, each variable will contain 20% of the total variance in the data.

![](https://github.com/JaclynGlosson/PCA-on-Food-Prices/blob/d4cad23f9055c664e835ab467a7dfd865599ae03/images/image34.png)

For this, I use the eigenvalue-greater-than-one rule, which states that “for standardized data the amount of variance extracted by each component should, at minimum, be equal to the variance of at least one variable” (Subhash Sharma, Pg 76). As the variance of one variable is 1 in standardized data, I retain those eigenvalues which are larger than 1, therefore I retain the first and the second eigenvalue, the first and the second PCs. The first eigenvalue explained 52.2% of the data variation, the second eigenvalue explains 19% of the data variation, and the third explains 14.9% of the data variation. In the standardized data, the first eigenvalue explains 48.7% of the data variation, the second eigenvalue explains 18.5% of the data variation, and the third explains 16.7% of the data variation. As expected, the percent of variation explained by each PC is roughly comparable to the unstandardized data. For standardized data, I would use the first two PCs, which account for 67.2% of the variance in the data. A large discrepancy between the two is seen within the eigenvectors. Whereas in unstandardized PC1, the price of hamburgers is clearly influential at 0.7, there is no single clear influencer in standardized PC1. Both the price of hamburgers and bread have similar values (0.52 and 0.51 respectively), which is not seen in the initial analysis. However, the general interpretation is still similar to the initial analysis, as a high price of Hamburgers and Bread is associated with a low value of PC1. All PC1 values are still negative, such that any food value that has a high price will have a low value for PC1. In the unstandardized PC2, Butter had the largest absolute value at -0.75 (recall that butter had the second largest variance), and in the standardized PC2, Apples have the largest absolute variance (-0.87) and Butter’s variance is negligible. For PC2, high prices of Apples are associated with a low value of PC2. In the initial analysis PC3, a high price of Apples was associated with a high value of PC3. For standardized PC3, a high price of Butter is associated with a low value of PC3. Given these eigenvalue differences, the resulting PCs for cities will likewise differ from the unstandardized PCs, since the eigenvalues are used to calculate the PCs. 

![](https://github.com/JaclynGlosson/PCA-on-Food-Prices/blob/d4cad23f9055c664e835ab467a7dfd865599ae03/images/image36.png)

Plot interpretation: I observe that Anchorage and Honolulu have a high price of hamburgers/bread and a high price of apples. I can see buffalo has a low price of hamburgers/bread and a low price of apples, again this matches the data. Los Angeles has a high price of hamburger/bread and a low price of apples, again consistent with the data.

## Use a Regression
Running a regression between original standardized variables and a random Y variable, and running a regression between all PCs and a random Y variable, I am able to get the same R squared (0.9396) meaning that 94% of the variance in the data can be explained by the regression line. When I run a regression between two of the original IVs and a random DV variable, I see that I get a lower R squared (0.641) than if I run a regression between two PCs and the same random DV variable (0.912). Thus, even though I am using less data, I am able to explain a similar amount of variance than if I used all data. By just using the first two (and therefore the ones that most explain the variance) PCs, I can determine that 91% of the variance in the data is explained by the regression line. Therefore using standardized PCAs will enable use to construct a price index that describes most of the variance in the data.








