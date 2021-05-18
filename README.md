## Problem

For this project we decided to take a kaggle challenge "*Craft canned beers data set*" (https://www.kaggle.com/nickhould/craft-cans). With the given data we plan to the answer to the following questions :

What types of beers are most popular in different regions.

Find if there are correlations between attributes like alcohol content by volume, bitterness, style of beer.

Find how much the beers are similar to one another.

After analysing the data the goal is to make a recommender system that after the user inputs his favourite beers, outputs a set of beers that the user also might like.

Data

The data is split in two files.

- beers.csv (List of 2K+ craft canned beers from the US.)

  - row number

  - ibu - International bittering units, which describe how bitter a drink is.

  - abv - The alcoholic content by volume with 0 being no alcohol and 1 being pure alcohol.

  - id - unique id

  - name - name of the beer

  - style - beer style(lager, IPA, ale, etc.)

  - brewery id - Unique identifier for brewery that produces this beer; can use to join with brewery info.

  - ounces - Size of beer in ounces.

- breweries.csv ( List of 500+ breweries in the United States.)

  - brewery_id

  - name - Name of the brewery.

  - city - City that the brewery is located in.

  - state - state that the brewery is located in.



## Data Cleaning

Before we analyze and use the data we have to account for the missing values.
Since there are few missing alcohol level(**abv**) and **style** values, we'll impute them using simpler methods, and then perform a visual analysis of the data. Afterwards we will impute the missing **ibu** using a more complex approach since a very large portion of the data is missing.
For the missing **abv** values, we will use a **hot deck** imputation method, where we replace the missing value with the median alcohol level of beers of the same **style**. This method seems most intuitive since it is common for beers of the same style to have similar alcohol levels.

## Visualizations

Before doing the models, we need to gather information about the distributions, outliers, and mine knowledge from the data. We do this using different visualisations that will help us discover more details about the data.

![alt text](https://github.com/in-droid/PR21INRAKT/blob/master/images/styleDist.png?raw=true)
<br/><br/>
From the barchart above we can see that the most 'popular' style is America IPA, with American Pale Ale on the second place and American Amber/ Red Ale on the third place. There are many styles that only take a small fracture of the whole dataset that are not shown here.

![alt text](https://github.com/in-droid/PR21INRAKT/blob/master/images/scatterPlot.png?raw=true)
<br/><br/>
Next we plot the bitterness and the alcoholic content of each beer, so we can see if there is any kind of correlation between these two variables. The scatter plot above suggests a positive linear correlation.
The **pearson coefficient** is 0.68 which confirms the statement above.

![alt text](https://github.com/in-droid/PR21INRAKT/blob/master/images/catPlot.png?raw=true)
<br/><br/>
This visualization specifies the *bitterness* and *alcoholic content* for the top 10 most 'popular' styles off beers.

![alt text](https://github.com/in-droid/PR21INRAKT/blob/master/images/betaDist.png?raw=true)
<br/><br/>
The Beta destribution above is bell shaped. The graph for 'Bitterness' is skewed right (positively skewed) and shows us that the majority of beers are not that bitter. The graph for 'Alcohol Levels' is normally distributed with a mean value of 0.06.

It would be useful if we have the information separated by state, so we can recognize if there is any dependency between the state of production and the beer *bitterness*, *alcohol content*, *style*, etc.
![alt text](https://github.com/in-droid/PR21INRAKT/blob/master/images/stateMap.png?raw=true)
<br/><br/>
The interactive map above shows the statistics for each state separately. What is interesting is the fact that we do not have bitterness data from the breweries in *South Dakota*.

## Imputation of data - Bitterness

Later in the project we will use clustering to make a recommendation system, in order to score the clusters we will use silhouette score which does not work with missing values. To solve this problem we will use imputation.

Many beers throughout the states and all in *South Dakota* do not have information about *bitterness* provided. The case of the missing values was not provided in the description of the data, so we assume that the values are most likely *missing at random*. In this case we try to model the *bitterness units* (*ibu*) with multiple regression models, choose the one that best fits the data, and use it to predict the **NaN** values.

The models are evaluated using R^2 score and **RMSE** (root mean squared error). Because sometimes the splitting of the data can be biased(it may contain different distributions in the test and train set), so we repeat the 'experiment' four times and from that we calculate the mean of the metric used for evaluation.
From all the models *Random forest regressor* works the best with R^2= 0.775 and ***RMSE*** = 12.749 (average values of four repeated experiments.)

## Regional grouping

To better gain some insight into any possibly interesting correlations or differences in data, we decided to group the beers into geographical regions based on their respective brewery's state. These regions include the North East, the South East, the Mid West, the South West and the West. 
We will try to distinguish if there is any speciffic difference in taste between regions of the United States, and see which styles are most popular everywhere.

![alt text](https://github.com/in-droid/PR21INRAKT/blob/master/images/regionAlc.png?raw=true)
<br/><br/>
We compared some attributes between these regional subsets. 
There does not seem to be much of a difference in mean alcohol level between states, but the Mid West seems to have the highest and the North East has the lowest.

![alt text](https://github.com/in-droid/PR21INRAKT/blob/master/images/regionStyle.png?raw=true)
<br/><br/>
We looked at the most popular styles in each region.
'American IPA' dominates in popularity in each region. The 4 runners up in each region are farily close in popularity to each other, with the exception of 'American Pale Ale' that is fairly more poular in the South East and the Mid West.

## Clustering

We use hierarchical clustering to identify and group similar beers together, so that we may later use these clusters for our recommendation function. 
We chose to use a clustering approach for the recommendation function because our data does not contain any individual user information, so there is no reliable way to use matrix factorization.

![alt text](https://github.com/in-droid/PR21INRAKT/blob/master/images/clustering.png?raw=true)
<br/><br/>
Since the whole dataset is too big, for a better visualiation we only show clusters of states and regions.
The above graph shows the clusters in the state '(MN) Minnesota', having a decent silhouette score of 0.219. 

## Recommendation function

For our recommendation function we will first create 2 clusterings for the whole beers dataset. First one using the optimal calculated t metric for cutting, and the second with the optimal metric decreased by 0.2 points. The second is mainly used in cases where the first's derived groups are too big and we need to branch out more to get a better recommendation. 

Most clusters are of reasonable length, but we have some clusters with more than 400, 200 and 100 elements, that are a bit too big to get recommendations from. This is why we employ the second clustering method for those.

## GUI Implementation

Finally we created a graphical user interface where you can input names of your favorite beers and recieve a list of recommended beers that you might like.
For the gui we used pyqt5 and created a few widgets and a function that get activated by clicking a button, after which results for recommendation are displayed.

![alt text](https://github.com/in-droid/PR21INRAKT/blob/master/images/gui.png?raw=true)
