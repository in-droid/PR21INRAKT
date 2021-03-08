**Problem**

For this project we decided to take a kaggle challenge "Craft canned beers data set"
(https://www.kaggle.com/nickhould/craft-cans). With the given data we plan to the answer to the following
questions :

-   What types of beers are most popular in different regions.

-   Find if there are correlations between attributes like alcohol content by volume, bitterness, style of beer.

-   Find how much the beers are similar to one another.

After analysing the data the goal is to make a recommender system that
after the user inputs his favourite beers, outputs a set of beers that
the user also might like.

**Data**

The data is split in two files.

-   beers.csv (List of 2K+ craft canned beers from the US.)

    -   row number

    -   ibu - International bittering units, which describe how bitter a drink is.

    -   abv - The alcoholic content by volume with 0 being no alcohol and 1 being pure alcohol.

    -   id - unique id

    -   name - name of the beer

    -   style - beer style(lager, IPA, ale, etc.)

    -   brewery id - Unique identifier for brewery that produces this beer; can use to join with brewery info.

    -   ounces - Size of beer in ounces.

<!-- -->

-   breweries.csv ( List of 500+ breweries in the United States.)

    -   brewery\_id

    -   name - Name of the brewery.

    -   city - City that the brewery is located in.

    -   state - state that the brewery is located in.
