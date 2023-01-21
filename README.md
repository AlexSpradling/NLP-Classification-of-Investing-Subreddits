# NLP Classification Of Text Within Investing-Focused Subreddits
---
### A Comparison of r/WallStreetBets and r/Investing
---
## Problem Statement 

Our client, Ademala Research, a hedge-fund of relaxed moral composure, wishes to move markets by using spam bots to post spurious investment news and information on personal finance subreddits. In order for the bots to be effective, they must be trained to recognize and use similar language to a real-life subreddit poster. Ademala has commissioned our firm to conduct a preliminary study of the language, phrases, and idioms used in two popular subreddits, r/WallStreetBets and r/Investing, and report back our findings. As such, we have undertaken to first build a Natural Language Processing classification model that is capable of distinguishing whether a post originated in r/WallStreetBets or r/Investing based on the text used in the post. In this way, Ademala will be able to use our findings to build robust and nefarious spam-bots. 

### Background

r/WallStreetBets and r/Investing are popular personal finance subreddits on the webpage reddit.com, with r/WallStreetBets being the more popular of the two, with 13.3M members vs. 2.1M members. Users will frequently post investment queries, receive advice or share their financial gains and losses. While r/Investing is not so dissimilar from any other forum that centers on personal finance, r/WallStreetBets is more akin to a gambling den, and rose to international prominence in 2020 when its members engineered a short-squeeze on stock issued by the video game retailor GameStop. 

r/WallStreetBets posters proudly proclaim themselves to be "degenerates" as they favor high-risk high-reward options-centered trading strategies on high-volatility penny stocks, often losing more money than they have in their brokerage accounts due to the leverage. r/WSB posts use a idiomatic and often colorful combination of slang, emoticons and profanity to express their goals and beliefs whereas r/investing posts are far more conventional. 

### Summary 

We began our modeling process by collecting over 10,000 posts from each subreddit, and cleaning and processing the text data in a fashion that would allow our classifers to render more accurate predictions. We employed a word lemmatization transformer to reduce derivationally related forms of words to a common base as well as TF-IDF and Count Vectorization methods to tokenize the text. We used care to *not* remove text items that were strongly r/WallStreetBets such as the usage of '🚀', '🙌', '🐻', '🐂', '💵', '🍗','💎' emojis, which are used to convey market or investment sentiment. 

With the cleaned and processed text from each subreddit, we conducted thorough exploratory data analysis and found that outside of a few distinguishing words, and idioms, r/WSB and r/investing were largely similar. The two subreddits share similar post word-length distribution, content, frequency of webpage sharing, stock ticker selection as well as many other similarities. Using SpaCy's NLP library, we were able to determine that the two subreddits are roughly 81% similar. However, the EDA process did reveal that the primary difference in investment sentiment between the two subreddits is one of short vs. long-term trading. r/WSB posters more often use language related to short-term and high-leverage investment strategies than r/investing posters. 

For our modeling process, we chose to use 5 different classifcation models and 2 different language preprocessors. We used a `TfidVectorizer` as well as a `CountVectorizer` to vectorize the lemmatized text, and used Logistic Regression, Random Forest, Extra Trees, KNN, and Naive Bayes classifers as evaluators. For hyperparameter optimization, we first used `RandomSearchCV` over the 5 evaluators using the `TfidVectorizer`, making 1,250, fits and then repeated the process again using the `CountVectorizer` making another 1,250 fits. At the end of this random search process we found that the `TfidVectorizer` and Logistic Regression evaluator had delivered the best results, with a classification accuracy score of .90 on training data and .87 on un-seen testing data. 

With this data, we then undertook a rigorous gridsearch of hyperparameters for our `TfidVectorizer` and Logistic Regression evaluator exploring the effects and interactions of different hyperparameters, making over 16,000 different fits. Our final tuned production model performed quite well, with an overall training accuracy score of .90 and an overall testing accuracy score of 
.878. On unseen testing-data, the final model correctly classified 89% of the r/WSB posts and 86% of the r/investing posts, suggesting the production model is well balanced, achieving an area under the cuve score of .94.

### Conclusions

1. Logistic Regression was the best performing model across the training and testing data splits amongst the five models we evaluated. 

2. The TF-IDF vectorizer's more nuanced approach to vectorization yielded better results than the Count Vectorizer.

3. We achieved better classification accuracy using title and selftext from subreddit posts rather than one or the other. 

4. Our classification model broke down when either subreddit used general investment language. A r/WallStreetBets posts devoid of profane language, references to options or short term investment strategies or any of the stock tickers frequently mentioned (such as TSLA or NKLA) became difficult to distinguish from a r/Investing post. Conversely, a r/Investing post that centered on options or tickers such as TSLA looked more like a r/WallStreetBets post to the classifier, and accuracy was lost. 

A high-performance spam-bot would best be programmed to use words that are strongly-r/WSB in a majority of its posts. These words might include options, or other leverage-centric investments terms. The bot should also use emojis such as a '🚀', '🙌', '🐻', '🐂', '💵', '🍗','💎' to reiterate its points or use idiomatic r/WallStreetBets terminology such as "Tendies", Stonks" or acronyms such as "DD" (due diligence). The usage of these phrases or terms lends a post the WallStreetBet-ness that general personal finance posts in r/Investing do not have. Put simply, a reddit personal finance post is just any other personal finance post *unless* it uses a word like *tendies* and then it immediately self-identifies itself as r/WallStreetBets. 



