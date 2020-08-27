# Univariate and Bivariate data
Bivariate data is data that involves two different variables whose values can change. Bivariate data deals with relationships between these two variables. The purpose of bivariate data is to analyze and explain this relationship.

Some data has only one variable. For example, if I were to record the ages of all students in a school and graph my data, then there would only be one variable, the age of the students. This type of data is known as univariate data and it does not deal with relationships, but rather it is used to describe something. In this example univariate data is used to express the ages of the students in a school.

# Measure of relationship
The measure of relationship are those statistical measures that we use in context of univariate population i.e., the population consisting of measurement of only one variable. But if we have the data on two variables, we are said to have a bivariate population and if the data happen to be on more than two variables, the population is knowns as multivariate population. If for every measurement of a variable X, we have corresponding value of a second variable Y, the resulting pairs of values are called bivariate population. In addition, we may also have a corresponding value of the third variable Z, or the fourth variable W and so on, the resulting pairs of values are called a multivariate population. In case of bivariate or multivariate populations, we often wish to know the relation of the two and/or more variables in the data to one another. We may like to know, for example, whether the number hours of student devotes for studies is somehow related to their family income, to age, to sex or to similar other factors. There are several methods of determining the relationship between variables, but no method tell us for certain that a correlation is indicative of casual relationship. Thus we have to answer two types of question in bivariate or multivariate population:
1. Does there exist association or correlation between the two (or more) variables? If yes, of what degree?
2. Is there any cause and effect relationship between the two variables in case of the bivariate population or between one variable on one side and two or more variables on the other side in case of multivariate population? If yes, of what degress and in which direction?

The first question is answered by the use of correlation technique and the second question by the technique of regression. There are several methods of applying the two techniques, but the important ones are as under: 
- In case of bivariate population: Correlation can be studied through cross tabulation, Charles Spearman's coefficent of correlation, Karl Pearson's coffecinent of correlation; whereas cause and effect relationship can be studied through simple regression equations.
- In case of multivariate population: Correlation can studied through cofficient of multiple correlation, coefficient of partial correlation; where cause and effect relationship can be studied through multiple regression equations. 

## Correlation
Correlation is a bivariate analysis that measure the strength of association between two variables and the direction of the relationship. In terms of the strength of relationship, the value of the correlation coefficient varies between +1 and -1. A values +- 1 indicates a prefect degree of association between two variables. As the correlation coefficent values goes towards 0, the relation between two variables will be weaker. The direction of relationship is indicated by the sign of the coefficent; a + sign indicates a postive releationship and a - sign indicates a negative relationship. Usually, in statistics we measure fours types of correleations: Pearson correlation, Kendall rank correlation, Spearman correlation and the point-biserial correlation.
### Pearson r Correlation
Pearson r correlation is the most widely used correlation statistics to measure the degree of the relationship between linearly releated variables. For example, in stock market, if we want to measure how two stock are related to each other. Pearson r correlation is use to measure the degree of relationship between the two. The point-biserial correlation is conducted with the Pearson corrleation formula except that one of the variable is dichotomous. The following formula is used to calculate the Pearson r correlation: 

r<sub>xy</sub> = (n&#8721;x<sub>i</sub>y<sub>i</sub>y - &#8721;x<sub>i</sub> &#8721;y<sub>i</sub>) / sqrt(n&#8721;x<sub>i</sub><sup>2</sup> - (&#8721;x<sub>i</sub>)<sup>2</sup>) sqrt(n&#8721;y<sub>i</sub><sup>2</sup> - (&#8721;y<sub>i</sub>)<sup>2</sup>)


r<sub>xy</sub>= Pearson r correlation coefficent between x and y

n = number of observations

x<sub>i</sub> = values of x (for ith observation)

y<sub>i</sub> = values of y (for ith observation)


## Covariance
In mathematics and statistics, covariance is a measure of the relationship between two random variables. The metric evaluates how much - to what extend - the variables change together. In other words, it is essentially a measure of the variance between two variables. However, the metrics does not assese the dependency between variables.

Unlike the correlation coefficent is measured in units. The units are computed by multiplying the units of the two variables. The variance can take any postive and negative values. The values are interpreted as follows:
- **Postive covariance**: Indicates that two variable tend to move in the same direction.
- **Negative covariance**: Reveals that two variables tend to move in the inverse directions.

The covariance formula is similar to the formula for correlation and deals with the calculation of data points from the average value in a dataset. For example, the covriance between two random variables X and Y can be calculated using the formula (for population):

Cov(x,y) = (&#8721;(x<sub>i</sub> - X) (y<sub>i</sub> - Y))/n

x<sub>i</sub> = the values of the x-variable

y<sub>i</sub> = the values of the y-variable

X = mean of the x-variable

Y = mean of the y-variable

n = the number of data points

### Covriance vs Correlation
Covariance and correlation both primarily assess the relationship between variables. The closet analogy to the relationship between them is the releationship between the variance and standard deviation.
**Covariance** measures the total variation of two random variables from their expected values. Using covariance we can only gauage the direction of the relationship(whether the variables tend to move in tandem or show an inverse relationship). However, it does not indicate the strength of the relationship, nor the dependency between the variables.
On the other hand, **correlation** measures the strength of the relationship between variables. Correlation is the scaled measure of covariance. It is dimensionless. In other word, the correlation coffecient is always a pure value and not measured in any units.
The relationship between the two concepts can be expressed using the formula below:

p(x,y) = Cov(x,y)/sd<sub>x</sub>sd<sub>y</sub>

p(x,y) = the correlation between the variable x and y

Cov(x,y) = the covariance between the variable x and y

sd<sub>x</sub> = the standard deviation of the x-variable

sd<sub>y</sub> = the standard deviation of the y-variable
