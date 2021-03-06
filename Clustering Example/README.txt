

I went through multiple attempts and iterations for exploration using both Hierarchical Clustering and K-Means, but the code only shows the two attempts that I think are the most interesting to look at.

Process:
The first step was to load and plot all of the datasets in order to get a better visual understanding of the data to try to identify some common patterns in what could be deemed as roads. Just by looking at the data, some preliminary conclusions were that roads have a tighter distribution in the lower range of time taken between the antennas.

The next step was to plot each dataset's time spent between antennas as a boxplot to get an idea of the distribution of the points. Skewed data show a lopsided boxplot, where the median cuts the box into two unequal pieces. If the longer part of the box is to the right (or above) the median, the data is said to be skewed right. If the longer part is to the left (or below) the median, the data is skewed left. As expected, when comparing the boxplot to it's graphed counterpart, the graphs that can be visually identified as roads tend to be skewed left. 

My thought process for this assignment was that instead of throwing all the data into a clustering algorithm, I wanted to use descriptive statistics of the datasets to cluster each dataset on. So if the datasets with roads truly are left skewed, then the clustering could be based on that data description. As such, I chose variables that described the measures of location/spread for each dataset, resulting in a 50 row dataset with each antenna pair as a row with variables for mean and range between the median and the first quartile. Unfortunately, because the 50-row dataset is so small, having too many variables resulted in too many clusters, so I had to narrow the variables down.

Using Hierarchical Clustering resulted in many leaves, however when plotted into a heatmap showed that the many leaves could be rolled up into two main clusters. With some minor modifications like choosing a slice of the time frame instead of the whole day, I used K-Means to plot the results and assign the antenna pairs with the cluster results which gave me two clusters of 25 items each. The clusters were labelled as either Road or Not Road. Likely with further work and more refined clustering, sub-clusters could have been created (such as two roads, road and train, field, etc).
