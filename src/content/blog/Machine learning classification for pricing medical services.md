--- 
title: 'Machine learning classification for pricing medical services'
description: '.'
pubDate: 'Feb 20 2025'
heroImage: '/'
--- 



The data I selected comes 
from:https://meps.ahrq.gov/mepsweb/data_stats/Pub_ProdResults_Details.jsp?pt=Statist
ical%20Brief&opt=2&id=1308 
This study analyzed the impact of medical services on pricing. 
I first clarified my research goal to provide clearer pricing logic for healthcare service 
providers. So I chose decision tree and neural network regression

###

Because healthcare providers have a significant differentiation in service types and 
operations, I hope to use these contents as inputs to maximize the inclusiveness of the 
model. I used a standardizer and saved the model parameters 
In order to automate the entire process using scaler in the subsequent pricing analysis. 

![blog placeholder](/Neural1.png)

###

cluster
![blog placeholder](/Neural2.png)
![blog placeholder](/Neural3.png)

### Pricing
![blog placeholder](/Neural4.png)


#### Markdown: 
The data from CDC is enormous, containing hundreds of features. There are countless 
research paths, and based on the machine learning models in the classroom, we can 
adopt different model combinations according to different purposes, such as 
1. Cost driven factor analysis (interpretability model) 
*What the model does: Identify which input features (such as complications, medication plans, 
etc.) contribute the most to predicting costs. 
*Principle: On existing regression or classification models, apply algorithms such as SHAP/LIME 
to quantify the positive and negative impact of each feature on the "model output" (cost). 
2. High cost patient identification (classification model) 
*What does the model do: Classify patients into "high cost" vs "non high cost" categories to 
facilitate pre screening of potential heavy spending populations. 
*Principle: Using logistic regression, decision trees, deep classifiers, etc., treat cost labels (such 
as>a certain threshold) as categories and learn the mapping of "features → categories". 
3. Analysis of Cost Sub item Structure (Multiple Output Regression or Branch Model) 
*What does the model do: simultaneously predict the proportion or absolute value of multiple 
sub item costs such as medication fees, examination fees, and surgical fees. 
*Principle: Use a multi output regression network or branch out multiple "sub networks" at the 
last layer to predict different cost types. 
4. Spending time series pattern mining (sequence model) 
*What does the model do: Based on the cost time series of the patient's past multiple visits, 
predict the cost fluctuations for the next or future cycle. 
*Principle: Use time series models such as RNN/LSTM/Transformer to capture the "sequence" 
and "autocorrelation" for dynamic prediction. 
5. Cost based Clustering 
*What does the model do: Automatically classify patients into several categories based on 
"cost+characteristics" (such as low, medium, and high cost groups) for differentiated 
management. 
*Principle: Use unsupervised learning methods such as K-means, hierarchical clustering, or 
spectral clustering to cluster samples into the same cluster based on distance or similarity. 
6. Pricing and Optimization of Insurance Products (Simulation and Optimization Models) 
*What does the model do: Simulate the cost sharing results between patients and insurance 
companies under different deductibles, reimbursement ratios, and other parameters, and find 
the optimal combination. 
*Principle: First, estimate the cost distribution using a regression model, and then run Monte 
Carlo simulation or solve linear/nonlinear optimization problems based on this.


<a href="javascript:history.back()" class="back-button">← Back</a>
