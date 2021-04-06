Beta distributions

We display the average bitterness and alcohol levels of each beer using the beta distribution.
For the bitterness we get a distribution which tells us that the majority of the beers are not that bitter.
And the average alcohol level of all beers is 0.06.


Clusters

For the clustering we devide the dataset into regions and states, because the whole dataset is too large to display with a graph.
We constroct the matrix using "name" of beers as X values and fill up the Y values of that X value with:
-Average "ibu", we add "-1" if it doesn't have an "ibu" value;
-Average "abv" (alcohol level), and again add "-1" if it doesn't contain an "abv" value;
-"1" for the "style" it is, and add 0's for all other styles that it is not;
-"1" for the "brewery_id" it is, and again fill with 0's for which "brewery_id" it is not.

We use this matrix to construct a dendrogram for the given dataset.
