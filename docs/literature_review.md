# Literature review and public dataset

**PREDICT-Play: Predictive Relational Evaluation of Demand for In-licensing Content Titles**  

---

## Publication 1

| Field | Detail |
|---|---|
| Title | Knowledge Graph Convolutional Networks for Recommender Systems |
| Harvard reference | Wang, H., Zhao, M., Xie, X., Li, W. and Guo, M. (2019). Knowledge graph convolutional networks for recommender systems. The World Wide Web Conference on - WWW ’19. [online] doi:10.1145/3308558.3313417.3313417. |
| Link | [Publisher DOI](https://doi.org/10.1145/3308558.3313417); [open paper](https://arxiv.org/abs/1904.12575). |
| Modelling | The modelling was done with KGCN (Knowledge Graph Convolutional Networks) with sum, concatenation and neighbour aggregators. The baselines include SVD, LibFM, LibFM+TransE, PER, CKE and RippleNet. Within the model they make use of cross-entropy, negative sampling, L2 regularisation and Adam. The data split is as follows: 60/20/20 train/validation/test split, three repetitions. Evaluation Metrics used: AUC, F1 and Recall@K. |
| Preprocessing techniques | They convert ratings to implicit feedback (Using a MovieLens positive threshold of 4. They sample unobserved negatives and match items to Microsoft Satori entities and retain high-confidence triples (>0.9), excluding unmatched/ambiguous items. |
| Datasets used | MovieLens 20M, Book-Crossing and Last.FM, enriched with Satori knowledge graphs. The processed movie subset contains 16,954 items. |
| Key takeaways | 1. They leveraged typed knowledge graph relations to dynamically weigh item metadata according to personalised user preferences.<br>2. Evaluate a shallow graph rather than assuming greater depth helps with modelling .<br>3. When evaluating using offline implicit feedback metrics like CTR and Top-K we should remain aware that offline accuracy differs from live production.  |

---

## Publication 2


| Field | Detail |
|---|---|
| Title | Predicting movie success with machine learning techniques: ways to improve accuracy |
| Harvard reference | Lee, K., Park, J., Kim, I. and Choi, Y. (2016). Predicting movie success with machine learning techniques: Ways to improve accuracy. Information Systems Frontiers, [online] 20(3), pp.577–588. doi:10.1007/s10796-016-9689-z.|
| Link | [Publisher article](https://doi.org/10.1007/s10796-016-9689-z). Journal issue: 2018; first published online: 2016. |
| Modelling | Seven candidate algorithms were used namely: adaptive tree boosting, gradient tree boosting, linear discriminant analysis, logistic regression, a multilayer perceptron, random forests and a support vector classifier. The Cinema Ensemble Model (CEM) combines gradient boosting, discriminant analysis, logistic regression and random forests using plurality voting. If there is a tie gradient boosting resolves it. They conducted ten repeated random 80/20 train-test splits. Performance is then measured using average percent hit rates categorised into two groups: exact-class (Bingo) and within one class away from the outcome. (1-Away). |
| Preprocessing techniques | They focussed on top 400 movies by viewership and decided to remove records with missing values, they ended up with 375 complete records. They bucketed the target variable into one of 6 buckets. Encode categorical variables as binary indicators, including 16 multi-hot genre indicators. The 21 features cover six groups: genre, sequel, opening-day plays, pre-release movie buzz, transmedia storytelling and star buzz. |
| Datasets used | Movies released in the Korean market from 25 October 2012 to 31 December 2014. The initial sample comprises the top 400 by attendance. Main sources: Korean Film Council and Naver; IMDb contributes information on foreign films' transmedia origins. |
| Key takeaways | 1. This paper highlights that tree-based gradient boosting models handles this type of data well. This supports testing XGBoost and not assuming that complex architecture performs best. <br>2. Test meaningful feature construction: CEM's exact-class accuracy rises from 53.7% without transmedia information to 58.5% with it, a 4.8 percentage-point increase. This emphasises adding contextual features to the data. <br>3. Due to the restricted nature of the study (only top 400 theatrical releases) the outcomes of this study cannot be used as benchmark for other studies or projects. 

---

## Publication 3


| Field | Detail |
|---|---|
| Title | Predicting consumer behavior with Web search |
| Harvard reference | Goel, S., Hofman, J.M., Lahaie, S., Pennock, D.M. and Watts, D.J. (2010). Predicting consumer behavior with web search. Proceedings of the National Academy of Sciences, [online] 107(41), pp.17486–17490. doi:10.1073/pnas.1005962107. |
| Link | [Publisher article](https://doi.org/10.1073/pnas.1005962107). |
| Modelling | Log-linear models predict movie opening-weekend game first-month revenues specifically to handle highly skewed popularity distributions. Linear/ autoregressive models are used for song ranks. The team analysed influenza search trends to establish how search queries compare to traditional autoregressive models and also compared this to a combined approach. For cross-validation and estimation: movies/games use leave-one-out estimation and song models use preceding observations to predict the following week. Reported evaluation principally uses correlation between predicted and realised outcomes, not RMSE or R-squared lift. |
| Preprocessing techniques | Associate movie searches with IMDb identifiers appearing in search results, selecting the highest-ranked match when necessary. Games use identifiers from gaming websites and music queries are matched to normalised song titles within Yahoo! Music. The team then aggregates the relevant search activity before the forecast event. A Log-transform is applied to skew revenue and search-volume variables for movies/games.|
| Datasets used | Yahoo! search activity linked to 119 US-released films, 106 video games and 307 Billboard songs. The study combines IMDb movie data, Hollywood Stock Exchange predictions, VGChartz game data and Billboard charts, mainly covering 2008-2009 entertainment outcomes. CDC/Google Flu Trends data support the additional monitoring analysis. |
| Key takeaways | 1. Compare search volumes to the baseline. When examining search volumes, which achieved a correlation of 0.85 with box-office sales, it is noted that simply using the baseline, which has a correlation of 0.94, was sufficient. <br>2. Search data yields the best results where in areas where baseline information is weak or unavailable. <br>3. Only information known before prediction should be included in modelling. |

---

## Public dataset

---

| Field | Detail |
|---|---|
| Name | MovieLens 20M Dataset |
| Harvard reference | Harper, F.M. and Konstan, J.A. (2015). The MovieLens datasets. ACM Transactions on Interactive Intelligent Systems, [online] 5(4), pp.1–19. doi:10.1145/2827872. |
| Link | [Dataset](https://grouplens.org/datasets/movielens/20m/); [official README](https://files.grouplens.org/datasets/movielens/ml-20m-README.html). |
| Number of instances | 20,000,263 ratings and 465,564 tag applications across 27,278 movies and 138,493 users. |
| Number of feature columns | Six source files contain 19 column entries, including repeated keys: movies.csv (3), ratings.csv (4), links.csv (3), tags.csv (4), genome-scores.csv (3), genome-tags.csv (2). These are not 19 independent predictors. |
| Number of classes | No native binary watch/completion label. We will simulate this |
| Target range | Not available we will simulate this. |
| Origin | Observed MovieLens preference activity collected by GroupLens, University of Minnesota, from January 1995 to March 2015.|
