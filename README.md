
# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 3: Web APIs and NLP - Warriors & Lakers Subreddits

### Problem Statement
We hypothesize that NBA team online advertisements are not targeted at the users as effectively as they could be targeted. This project aims to utilize natural language processing to classify user posts as coming from the Warriors or Lakers subreddits.  This classification model can then be applied to other user posts on the internet to determine the fan loyalty of the user to effectively advertise accordingly.

---
### Datasets Used

* 'r/warriors' subreddit scraping (submissions only)
* 'r/lakers' subreddit scraping (submissions only)

---
### Notebook Contents

* **Notebook 1** -  Data scraping, cleaning, and EDA on Warriors subreddit.
* **Notebook 2** -  Data scraping, cleaning, and EDA on Lakers subreddit.
* **Notebook 3** -  Merging warriors/lakers data frames, processing, and modeling on dataframe with countvectorizer and TF-IDF vectorizer transformers.  Models used were Naive Bayes and Random Trees.
* **Notebook 4** -  Further analysis on best performing model (Naive Bayes) with best performing hyperparameters.


### Other Folder Contents in Repository

* **best_param_dfs** - This folder includes a dataframe which is comprised of the best parameters when running the Random Forests model.  Because this model could take several minutes to an hour to run, the best parameters were saved to a separate dataframe for analysis before closing Jupyter notebooks.

* **cleaned_dfs** - This folder includes the cleaned lakers and warriors dataframes before the merging of the 2 dataframes in Notebook 3.  It also includes a copy of the merged dataframe before beginning the modeling process.

---


### Noteworthy mentions from process

* **Data Scraping** -  After review of a sample of submissions and comments, it was noted that the language was not too distinct in the submissions vs the comments.  It was also observed that comments often went on irrelevant tangents unrelated to the subreddit.  Therefore, only submissions were scraped. Reddit only allowed scraping of 100 rows at a time:  in order to combat this, the *time.sleep* package was used to pull 100 rows of data every couple seconds.  The ending total number of rows pulled was 5,000 which was deemed to be a sufficient amount of data.
In order to ensure that duplicate data was not pulled in the time.sleep function, a time stamp was added to the function in order to identify which rows of data had already been selected for function.

* **Feature Engineering** - A 'whole_post' feature was created for *both* subreddits, which combined values in 'title' and 'selftext' column. Before creating this feature, it was noted that there empty spaces in the in the 'selftext' column that were *not* treated as null values. In order to ensure there were no empty spaces, the text in the title and selftext columns were combined. Because the chronological order of words is not relevant for our modeling, combining these texts made sense for a more concise but accurate dataframe to model off of.  In short, only this engineered feature, 'whole_post', was used in modeling.

* **Stop Words** - Use of the scikitlearn English stop words, as well as my own customized list of stop words (including URL words such as 'com', 'http', etc) proved to be optimal in every instance a model was run.  My customized list of stop words (named 'STOP_WORDS') was excluded when fitting all models.

---

### Random Forests Model - Summary
The Random Forests model utilized the TF-IDF vectorizer transformer and bootstrapping. The description of the best parameters seen in this model were as follows:

* **N_estimators**:  the number of trees in our model – a sweet spot of 125 was found after multiple trial and errors around this value.

* **Max_features**: the number of features considered, which in this case, is words – Overall increasing the max depth proved to be beneficial but had diminishing returns at around 70.

* **Ccp_alpha**:  picks the most complex model to use – the optimal amount for this parameter was zero every single time the model was run.  

***Summary***: Overall, the testing score was very successful at 87.75%, especially successful when you compare this score to the baseline accuracy score of about 50%.  However, this model is way overfit, as the training score is a substantially higher at 99.73%.  Improvements could be removing features and depth amount, but the Random Forests model tends to usually suffer from overfitting.


### Naive Bayes Model - Summary
The Naive Bayes model utilized the TF-IDF vectorizer transformer.  Countvectorizer was also used but proved to be less successful. The description of the best parameters seen in this model were as follows:

* **Max_features**: max features proved to be more successful the more this parameter was increased, but had diminishing returns around 9,000.

* **Ngram_range**: the optimal lower boundary was 1 and optimal upper boundary was 2.  Adjusting this parameter had little effect on the model.

* **Stop_words**:  stop words always proved to be optimal over no stop words used, or more interestingly, more optimal over just English stop words used.  In the model, I appended other words to include in stop words list, such as “http”, “com”, and other filler words.

***Summary*** The testing score on this model was slightly less than the Random forest, but is a more ideal model because it suffers from far less overfitting (as seen in the training score of 92.8% instead of 99%).

An additional noteworthy finding was the most commonly occurring words in the misclassified posts: Within these posts, I found the most commonly occurring words. Interestingly, I found that a lot of the opposing team key words (such as the actual words ‘warriors’ and ‘lakers’) were seen. This occurrence actually makes sense, as people in the warriors subreddit, for example, very frequently write posts about Lebron James (who is arguably considered the "face of the NBA").  Our model predicts that a mention of Lebron would be “Lakers”, but in reality, this post is written by a Warriors fan talking about Lebron.



### Conclusions & Limitations
We obtained successful models with very high accruacy from subreddit posts.  We can apply the model to other user posts on internet, such as Twitter, Facebook, blogs, etc. On each website we determine if the user is a Warriors or Lakers fan and advertise accordingly.

Note we do have some limitations:
Some reddit users might be Lakers fans, but are still active in Warriors subreddits or frequently discuss the Warriors in their media platforms. Our also model currently only works on Warriors and Lakers – a model needs to be trained for other teams.
Our best model, Naïve Bayes, is still fairly overfit.  Tuning hyperparameters could improve the model.



---
