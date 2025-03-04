### Final project for AIPI590, Fall 2022: Deep RL product recommenders for E-commerce platforms

## Project Summary

This project implements methods described by the paper [*Supervised Advantage Actor-Critic for Recommender Systems*](https://arxiv.org/pdf/2111.03474.pdf) to address two key challenges in session-based recommender systems (RS) that aim to maximise cumulative profits: impractical usage of reinforcement learning (RL) algorithms and the lack of negative reward signals. For the former, we combine RL and (self-)supervised sequential learning approaches to use RL as a regularizer of supervised learning models, while the latter involves using a negative sampling strategy for training the RL component and combining it with supervised sequential learning. In the paper, this method is referred to as 'Supervised Negative Q-learning (SNQN)'. We follow the paper to calculate the advantage of a positive action over the average case in a Supervised Advantage Actor-Critic (SA2C) farmework, which can be further utilized as a normalized weight for learning the supervised sequential part.

Results are compared with [Microsoft's Smart Adaptive Reccomendations Algorithm (SAR)](https://github.com/microsoft/Product-Recommendations/blob/master/doc/sar.md). Based on a collaborative filtering approach, SAR is a scalable adaptive algorithm for personalized recommendations based on user transactions history and items description. It produces easily explainable / interpretable recommendations and handles "cold item" and "semi-cold user" scenarios. SAR can provide know kinds of recommendations: User recommendations, which recommend items to individual users based on their transaction history, and Frequently-Occurring-Together recommendations, which recommend items similar to a given set of items.

Our work proceeds in three main steps:
1. Implement code of *Supervised Advantage Actor-Critic for Recommender Systems* on data from Diginetica and RetailRocket.
2. Use offline evaluation metrics to benchmark performance of Deep RL recommendor
3. See how results compare to a Non-Deep RL recommendor, in our case, Microsoft's SAR.


## Dataset Overview

For both models, we conduct experiments using SNQN and SA2C for two real world datasets: [Diginetica](https://competitions.codalab.org/competitions/11161) and [RetailRocket](https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset). 


 <img width=390 align="center" src="img\diginetica.jpeg">
 
- Released as a part of the 2016 CIKM cup, this dataset includes user sessions extracted from an e-commerce search engine logs, with anonymized user ids, hashed queries, hashed query terms, hashed product descriptions and meta-data, log-scaled prices, clicks, and purchases. It includes 1,235,380 views and
18,025 purchases across 232,816 users and 184,047 items. 
 
 <img width=390 align="center" src="img\RetailRocket.png">
 
- Retail Rocket is a company that generates personalized product recommendations for shopping websites and provides customer segmentation based on user interests and other variables. Released as a part of a Kaggle competition, this dataset was collected from a real-world ecommerce website and consists of behaviour data, item properties and category trees. The behaviour data, i.e. events like clicks, add to carts, transactions, represent interactions that were collected over a period of 4.5 months. A visitor can make three types of events, namely “view”, “addtocart” or “transaction”. In total there are 2,756,101 events including 2,664,312 views, 69,332 add to carts and 22,457 transactions produced by 1,407,580 unique visitors. However, all values are hashed to address confidentiality concerns. 

<!-- https://towardsdatascience.com/ranking-evaluation-metrics-for-recommender-systems-263d0a66ef54
 -->
 
## Running the code

We used Google Colab to create jupyter notebooks for each model type (DRL and Non DRL) and dataset (Diginetica and Retail Rocket). To run our Deep RL model for Diginetica and RetailRocket, navigate to `/Models/Diginetica_SA2C.ipynb` and `/Models/RetailRocket_SA2C.ipynb`, where the respective source code is linked to `/src/Diginetica` and `/src/RetailRocket` respectively. All pre-processing steps are managed in the source code itself. Results are also computed within the individual Jupyter notebooks. Then, run `/Models/Microsoft_Reccomender.ipynb` independently for our non DRL recommender. 

## Evaluation Metrics
1.  HR (Hit Ratio): The fraction of users for which the correct answer is included in the recommendation list of length L. The larger L is, the higher hit ratio becomes, because there is a higher chance that the correct answer is included in the recommendation list. 


<!-- ![image](https://miro.medium.com/max/1400/1*p2oVTjdyCRgJvfSSC9TDpw.webp)  -->


2.  NDCG (Normalized Discounted Cumulative Gain): Measures the quality of the recommendation list based on the top-k ranking of items in the list where higher ranked items are scored higher. Gain for an item is essentially the same as the relevance score, which can be numerical ratings like search results in Google which can be rated in scale from 1 to 5, or binary in case of implicit data where we only know if a user has consumed certain item or not. 

<!-- <img width=250 align="center" src="https://miro.medium.com/max/584/1*GEvXfCqT6hq_KNT_WMnRFA.webp">


One obvious drawback of CG is that it does not take into account of ordering. By swapping the relative order of any two items, the CG would be unaffected. This is problematic when ranking order is important. For example, on Google Search results, you would obviously not like placing the most relevant web page at the bottom.

To penalize highly relevant items being placed at the bottom, we introduce the DCG

 <img width=250 align="center" src="https://miro.medium.com/max/640/1*sb2sXH1RHQFgZgl4l9pCSw.webp">

By diving the gain by its rank, we sort of push the algorithm to place highly relevant items to the top to achieve the best DCG score.

There is still a drawback of DCG score. It is that DCG score adds up with the length of recommendation list. Therefore, we cannot consistently compare the DCG score for system recommending top 5 and top 10 items, because the latter will have higher score not because its recommendation quality but pure length.

We tackle this issue by introducing IDCG (ideal DCG). IDCG is the DCG score for the most ideal ranking, which is ranking the items top down according their relevance up to position k.

 <img width=550 align="center" src="https://miro.medium.com/max/828/1*cDC8roXZrP-iUeR1vlmGBQ.webp">

And NDCG is simply to normalize the DCG score by IDCG such that its value is always between 0 and 1 regardless of the length.  -->


## Results

Diginetica:

| **Models**  | **HR@3** | **NG@3** | **HR@5** | **NG@5** | **HR@8** | **NG@8** |
| :---------: | :------: | :------: | :-------: | :-------: | :-------: | :-------: |
| SASRec-SA2C |  **0.1839**  |  **0.1477**  |  **0.2175**   |  **0.1616**   |  **0.2526**   |  **0.1735**   |
| Microsoft-SAR* |  **0.9351**  |  **1.057**  |  **0.9790**   |  **1.038**   |  **0.9941**   |  **1.030**   |

Retail Rocket:

| **Models**  | **HR@3** | **NG@3** | **HR@5** | **NG@5** | **HR@8** | **NG@8** |
| :---------: | :------: | :------: | :-------: | :-------: | :-------: | :-------: |
| SASRec-SA2C |  **0.1809**  |  **0.1536**  |  **0.2144**   |  **0.1673**   |  **0.2430**   |  **0.1770**   |
| Microsoft-SAR |  **0.1573**  |  **0.1836**  |  **0.1716**   |  **0.1861**   |  **0.1820**   |  **0.1874**   |


* *A quick note about running Microsoft SAR on the Diginetica dataset: The Diginetica dataset has a total of 9600 users/sessions where users make purchases where they buy about 1.3 items each and a total of 9400 items being purchased. Based on this ratio, it is apparent that each user bought a different item resulting in very little interaction between users. This lack of interaction between users and items causes a non-reinforcement learning method to overfit the training data and result in very large HR and NGDC. If we use a separate testing dataset, we suspect the HR and NGDC will drop to near 0.*

## Code Structure
```
├── README.md
├── Models
│   ├── Diginetica_SA2C.ipynb
│   ├── Microsoft_Reccomender.ipynb
│   └── RetailRocket_SA2C.ipynb
├── img
│   ├── RetailRocket.png
│   └── diginetica.jpeg
└── src
    ├── Diginetica
    │   ├── SA2C.py
    │   ├── SASRecModules.py
    │   ├── pop.py
    │   ├── replay_buffer.py
    │   ├── sample_data.py
    │   ├── split_data.py
    │   ├── test.py
    │   └── utility.py
    ├── RetailRocket
    │   ├── SA2C.py
    │   ├── SASRecModules.py
    │   ├── pop.py
    |   ├── pop_dict.txt
    |   ├── preprocess_kaggle.py
    │   ├── replay_buffer.py
    │   ├── split_data.py
    │   ├── test.py
    │   └── utility.py
    └── preprocessing_diginetica
        ├── prep_data.ipynb
        └── dataset-train
            └── README.md
```

## Contributors

| Name | Reference |
|---- | ----|
|Abhijith Tammanagari | [GitHub Profile](https://github.com/23abhijith)|
|Sarwari Das | [GitHub Profile](https://github.com/HarTigran)|
|Tigran Harutyunyan |[GitHub Profile](https://github.com/sarwaridas)|

