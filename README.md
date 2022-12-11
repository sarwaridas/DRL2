### Final project for AIPI590, Fall 2022: Deep RL product recommenders for E-commerce platforms

### Project Summary

This project implements methods described by the paper [*Supervised Advantage Actor-Critic for Recommender Systems* [1]](https://arxiv.org/pdf/2111.03474.pdf) to address two key challenges in session-based recommender systems (RS) that aim to maximise cumulative profits: impractical usage of reinforcement learning (RL) algorithms and the lack of negative reward signals. For the former, we combine RL and (self-)supervised sequential learning approaches to use RL as a regularizer of supervised learning models, while the latter involves using a negative sampling strategy for training the RL component and combining it with supervised sequential learning. In the paper, this method is referred to as 'Supervised Negative Q-learning (SNQN)'. We follow the paper to calculate the advantage of a positive action over the average case in a Supervised Advantage Actor-Critic (SA2C) farmework, which can be further utilized as a normalized weight for learning the supervised sequential part.

We conduct experiments using SNQN and SA2C for two real world datasets: [Diginetica](https://competitions.codalab.org/competitions/11161) and [RetailRocket](https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset). 


 <img width=390 align="center" src="img\diginetica.jpeg">
 
Released as a part of the 2016 CIKM cup, this dataset includes user sessions extracted from an e-commerce search engine logs, with anonymized user ids, hashed queries, hashed query terms, hashed product descriptions and meta-data, log-scaled prices, clicks, and purchases. It includes 1,235,380 views and
18,025 purchases across 232,816 users and 184,047 items. 
 
 <img width=390 align="center" src="img\RetailRocket.png">

Released as a part of a Kaggle competition, this dataset was collected from a real-world ecommerce website and consists of behaviour data, item properties and category trees. The behaviour data, i.e. events like clicks, add to carts, transactions, represent interactions that were collected over a period of 4.5 months. A visitor can make three types of events, namely “view”, “addtocart” or “transaction”. In total there are 2,756,101 events including 2,664,312 views, 69,332 add to carts and 22,457 transactions produced by 1,407,580 unique visitors. 

Our work proceeds in three main steps:
- Implement code of *Supervised Advantage Actor-Critic for Recommender Systems* on data from Diginetica and RetailRocket.
- Use offline evaluation metrics to benchmark performance of Deep RL recommendor
- See how results compare to a Non-Deep RL recommendor, in our case, collaborative filtering.

<!-- https://towardsdatascience.com/ranking-evaluation-metrics-for-recommender-systems-263d0a66ef54
 -->
 
 
### HR (Hit Ratio)
In recommender settings, the hit ratio is simply the fraction of users for which the correct answer is included in the recommendation list of length L.

![image](https://miro.medium.com/max/1400/1*p2oVTjdyCRgJvfSSC9TDpw.webp)

As one can see, the larger L is, the higher hit ratio becomes, because there is a higher chance that the correct answer is included in the recommendation list. Therefore, it is important to choose a reasonable value for L.

### NDCG (Normalized Discounted Cumulative Gain)
NDCG stands for normalized discounted cumulative gain. We will build up this concept backwards answering the following questions:

What is gain?
What is cumulative gain?
How to discount?
How to normalize?
Gain for an item is essentially the same as the relevance score, which can be numerical ratings like search results in Google which can be rated in scale from 1 to 5, or binary in case of implicit data where we only know if a user has consumed certain item or not.

Naturally Cumulative Gain is defined as the sum of gains up to a position k in the recommendation list

![image](https://miro.medium.com/max/584/1*GEvXfCqT6hq_KNT_WMnRFA.webp)


One obvious drawback of CG is that it does not take into account of ordering. By swapping the relative order of any two items, the CG would be unaffected. This is problematic when ranking order is important. For example, on Google Search results, you would obviously not like placing the most relevant web page at the bottom.

To penalize highly relevant items being placed at the bottom, we introduce the DCG

![image](https://miro.medium.com/max/640/1*sb2sXH1RHQFgZgl4l9pCSw.webp)

By diving the gain by its rank, we sort of push the algorithm to place highly relevant items to the top to achieve the best DCG score.

There is still a drawback of DCG score. It is that DCG score adds up with the length of recommendation list. Therefore, we cannot consistently compare the DCG score for system recommending top 5 and top 10 items, because the latter will have higher score not because its recommendation quality but pure length.

We tackle this issue by introducing IDCG (ideal DCG). IDCG is the DCG score for the most ideal ranking, which is ranking the items top down according their relevance up to position k.

![image](https://miro.medium.com/max/828/1*cDC8roXZrP-iUeR1vlmGBQ.webp)

And NDCG is simply to normalize the DCG score by IDCG such that its value is always between 0 and 1 regardless of the length.




| **Models**  | **HR@3** | **NG@3** | **HR@5** | **NG@5** | **HR@8** | **NG@8** |
| :---------: | :------: | :------: | :-------: | :-------: | :-------: | :-------: |
| SASRec-SA2C |  **0.1839**  |  **0.1477**  |  **0.2175**   |  **0.1616**   |  **0.2526**   |  **0.1735**   |


## Contributors

| Name | Reference |
|---- | ----|
|Abhijith Tammanagari | [GitHub Profile](https://github.com/23abhijith)|
|Sarwari Das | [GitHub Profile](https://github.com/HarTigran)|
|Tigran Harutyunyan |[GitHub Profile](https://github.com/sarwaridas)|

