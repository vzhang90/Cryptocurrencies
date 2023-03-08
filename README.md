# Cryptocurrencies
This report will include what cryptocurrencies are on the trading market and how they could be grouped to create a classification system for a potential new investment by analyzing this [crypto_data.csv file](https://github.com/vzhang90/Cryptocurrencies/blob/main/crypto_data.csv), retrieved from [CryptoCompare](https://min-api.cryptocompare.com/data/all/coinlist).

This data is not ideal, so processing is required to better fit the machine learning models. Since there is no known output we are looking for, unsupervised learning is most suited in this scenario to group cryptocurrencies based on a clustering algorithm. 


<sub><b><em>Cryptocurrency Dataset Sampled:</b></em> [crypto_data.csv](https://github.com/vzhang90/Cryptocurrencies/blob/main/crypto_data.csv)</sub>  
<br>


## Overview of Analysis
> <sub><b><em>Unsupervised Machine Learning code:</b></em> [crypto_clustering.ipynb](https://github.com/vzhang90/Cryptocurrencies/blob/main/crypto_clustering.ipynb)</sub>

---
### **Preprocessing Data for PCA**     
Using Pandas, the dataset was initially preprocessed in order to be able to perform Principal Component Analysis (PCA) after:
1. read in the [crypto_data.csv](https://github.com/vzhang90/Cryptocurrencies/blob/main/crypto_data.csv) to the Pandas DataFrame named `crypto_df`     
2. keep all the cryptocurrencies that are being traded    
3. drop the `IsTrading` column    
4. remove rows that have at least one null value     
5. filter the `crypto_df` DataFrame so it only has rows where coins have been mined   
6. create a new DataFrame that holds only the cryptocurrency names, and use the `crypto_df` DataFrame index as the index for this new DataFrame    
7. remove the `CoinName` column from the `crypto_df` DataFrame since it's not going to be used on the clustering algorithm
8. use the `get_dummies()` method to create variables for the two text features, `Algorithm` and `ProofType`, and store the resulting data in a new DataFrame named `X`     
9. use the StandardScaler `fit_transform()` function to standardize the features from the `X` DataFrame

> Using the `StandardScaler() sklearn` library to standardize the features is required before moving onto next steps 
---
### **Reducing Data Dimensions using PCA**

Applying the Principal Component Analysis (PCA) algorithm to reduce the dimensions of the X DataFrame to three principal components and place these dimensions in a new DataFrame:
- create a new DataFrame named `pcs_df` that includes the following columns, `PC 1`, `PC 2`, and `PC 3`, and uses the index of the `crypto_df` DataFrame as the index
---
### **Clustering Cryptocurrencies using K-means**

1. use the `pcs_df` DataFrame, create an elbow curve using `hvPlot` to find the best value for K
2. use the `pcs_df` DataFrame to run the K-means algorithm to make predictions of the K clusters for the cryptocurrencies’ data
3. create a new DataFrame named `clustered_df` by concatenating the `crypto_df` and `pcs_df` DataFrames on the same columns. The index should be the same as the crypto_df DataFrame    
4. add the `CoinName` column that holds the names of the cryptocurrencies, created in Step 6 when preprocessing the data using PCS, to the `clustered_df`   
5. add another new column to the `clustered_df` named `Class` that holds the predictions, i.e., `model.labels_`, from Step 2
---
### **Visualizing Cryptocurrencies Results**
Using scatter plots with Plotly Express and `hvplot`, visualize the distinct groups that correspond to the three principal components created when reducing data dimensions with PCA earlier. Then, create a table with all the currently tradable cryptocurrencies using the `hvplot.table()` function.
1. create a 3D scatter plot using the Plotly Express `scatter_3d()` function to plot the three clusters from the clustered_df DataFrame    
2. add the `CoinName` and `Algorithm` columns to the `hover_name` and `hover_data` parameters, respectively, so each data point shows the CoinName and Algorithm on hover    
3. create a table with tradable cryptocurrencies using the `hvplot.table()` function     
4. print the total number of tradable cryptocurrencies in the `clustered_df` DataFrame    
5. use the `MinMaxScaler().fit_transform` method to scale the `TotalCoinSupply` and `TotalCoinsMined` columns between the given range of zero and one     
6. create a new DataFrame using the `clustered_df` DataFrame index that contains the scaled data created in Step 4    
7. add the `CoinName` column from the `clustered_df` DataFrame to the new DataFrame     
8. add the `Class` column from the `clustered_df` DataFrame to the new DataFrame    
10. create an `hvplot` scatter plot with x="TotalCoinsMined", y="TotalCoinSupply", and by="Class", and have it show the `CoinName` when hovering over the the data point
---
## Results

## Summary