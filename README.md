# Project dashboard for Astro 416
The goal for this dashboard is to implement two different classification methods to identify lyman-alpha emitters, low redshift galaxies, and active galactic nuclei from the HETDEX catalogue.

## Input
This program queries the HETDEX database and converts the ECSV files into DataFrames. Two files come in the query: 
- The Source Observation Table: hetdex_sc1_vX.dat/.fits/.ecsv
- The Detection Information Table: hetdex_sc1_detinfo_vX.fits/.dat/.ecsv

The source observation table contains one row per source while the detection information table contains one row per continuum or detection, which gives more information about bright sources. This dashboard uses the detection information table both for more data and for the fact that it contains more information like SNR, line flux, line width, and other parameters that are used in the classification algorithm.

## Filtering
To remove outliers, the code uses Median Absolute Deviation (MAD). A low MAD threshold results in the use of a percentile based method which removes the upper and lower 1% of data - the middle 98% of data is kept in this case. A higher MAD threshold removes any data that is not within 10x the threshold. This removes only the most extreme of outliers, and more data is typically kept with this method.

## Classification Method 1
The first, more simple, model is the Logistic Regression model. With three objects, and simple binary system is not enough to suffice so the dashboard implements a slightly more creative approach. By iterating through thrice, with each iteration assigning one object a "1," and the other two "0," we get three likelihoods for each row - basically saying, what is the probability that this row is not the other two objects. It then compares the probabilities for each of the three cases for every row and assigns the most likely of lae, agn, or lzg to each.

Given the known source_types, the accuracy is easily verified. To visualize the effectiveness, the user can see interactive plots of correctly and improperly identified points both as a scatter plot and a heatmap. 

## Neural Network