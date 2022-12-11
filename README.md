#  Deep RL product recommenders for E-commerce platforms

### Final project for AIPI590, Fall 2022 at Duke University to train and compare performance of session based recommendors.

## Contributors

| Name | Reference |
|---- | ----|
|Abhijith Tammanagari | [GitHub Profile](https://github.com/23abhijith)|
|Sarwari Das | [GitHub Profile](https://github.com/HarTigran)|
|Tigran Harutyunyan |[GitHub Profile](https://github.com/sarwaridas)|


https://towardsdatascience.com/ranking-evaluation-metrics-for-recommender-systems-263d0a66ef54

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
