##Problem

For this project we decided to take a kaggle challenge "Craft canned beers data set" (https://www.kaggle.com/nickhould/craft-cans). With the given data we plan to the answer to the following questions :

What types of beers are most popular in different regions.

Find if there are correlations between attributes like alcohol content by volume, bitterness, style of beer.

Find how much the beers are similar to one another.

After analysing the data the goal is to make a recommender system that after the user inputs his favourite beers, outputs a set of beers that the user also might like.

Data

The data is split in two files.

beers.csv (List of 2K+ craft canned beers from the US.)

row number

ibu - International bittering units, which describe how bitter a drink is.

abv - The alcoholic content by volume with 0 being no alcohol and 1 being pure alcohol.

id - unique id

name - name of the beer

style - beer style(lager, IPA, ale, etc.)

brewery id - Unique identifier for brewery that produces this beer; can use to join with brewery info.

ounces - Size of beer in ounces.

breweries.csv ( List of 500+ breweries in the United States.)

brewery_id

name - Name of the brewery.

city - City that the brewery is located in.

state - state that the brewery is located in.



##Data Cleaning

Before we can use our data we have to clean and correct some missing values. 
A small number of beers were missing their 'alcohol level' attribute. 
We decided to replace each value with the median alcohol level of the same style beer.

Very few beers(5 to be exact) were missing their style attribute. Since the number is so small, we decided to replace it with the most common style.

A very large portion of the dataset is missing the 'bitterness level' attribute. 
We decided it would be best instead of replacing the missing values, to exclude that portion of the dataset when dealing with analyses involving bitterness.

##Statistical analysis

While checking the distribution of different beer styles we found that the most popular styles are 'American IPA' accounting for roughly 18% of all beers, 'American Pale Ale (APA)' accounting for 10% of all beers and 'American Amber/Red Ale' making up 5% of all beers.


We performed a Pearon Correlation test and found that there is a strong positive correlation between bitterness and alcohol level.

We looked at the top ten beer styles and found that 'American Double/Imperial IPA' has a significanlty higher alcohol and bitterness level compared to the rest. 'American IPA' also has a notable bitterness level.

#Grouping states into regions

Since the sample size can vary exponentialy from state to state, we decided it would be better to group state data into geographical regions: North East, South East, South West, Mid West and West.

We compared some attributes between these regional subsets. 
There does not seem to be much of a difference in mean alcohol level between states, ut the Mid West seems to have the highest and the North East has the lowest.

We looked at the most popular styles in each region.
'American IPA' dominates in popularity in each region. The 4 runners up in each region are farily close in popularity to each other, with the exception of 'American Pale Ale' that is fairly more poular in the South East and the Mid West.

##Beta distributions

We display the average bitterness and alcohol levels of each beer using the beta distribution.
For the bitterness we get a distribution which tells us that the majority of the beers are not that bitter.
And the average alcohol level of all beers is 0.06.


##Clusters

For the clustering we devide the dataset into regions and states, because the whole dataset is too large to display with a graph.
We constroct the matrix using "name" of beers as X values and fill up the Y values of that X value with:
-Average "ibu", we add "-1" if it doesn't have an "ibu" value;
-Average "abv" (alcohol level), and again add "-1" if it doesn't contain an "abv" value;
-"1" for the "style" it is, and add 0's for all other styles that it is not;
-"1" for the "brewery_id" it is, and again fill with 0's for which "brewery_id" it is not.

We use this matrix to construct a dendrogram for the given dataset.


##Future work

We plan to look into more correlations and better optimize our clustering methods in order to develop a good recommendation system that can suggest beers to a user based on their current preferences.

