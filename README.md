# Capstone-EarningsCallTranscripts
![Small](https://user-images.githubusercontent.com/67437966/103885261-a3216e80-50d7-11eb-8fcb-4d47f6b9d5a7.png)

This repo covers the Capstone Project I completed during General Assembly's Data Science Immersive. 


## Topic:
Can Data Science predict movement of share prices based on how the managers talk on the quarterly earnings call? Finance analysts and investors in general spend a lot of time looking at the financial performance of companies, but can we make better predictions without spending any time on financial results, just focusing on the word choices of the management during quarterly earnings calls?

I aim to predict share price movement from the day of the earnings call to the closing day of the subsequent period.

## Summary of conclusion:
The best model predicted share price movement with 63% accuracy, which was 11 percentage points above baseline. A couple of different models produced predictions around 58-60% accuracy.
For comparison, a fund with a 50-55% hit ratio is considered to be a [good](https://thehedgefundjournal.com/identifying-manager-skill/) – [very good](https://www.linkedin.com/pulse/high-investment-hit-rate-too-good-true-ali-chabaane) fund, and [60%](https://morphicasset.com/what-batting-averages-can-tell-you-about-funds-management/) is believed to be needed for Warren Buffet. 

## Code

I collected my codes into one notebook where you can navigate through the different stages with the help of the Table of Contents. 
Or you can check it through [nbviewer:](https://nbviewer.jupyter.org/github/tolgyirita/Capstone-EarningsCallTranscripts/blob/main/Rita_Capstone_notebook.ipynb)


## Presentation

The project ended with a 20 minutes presentation. You can find the slides in this repo [here](https://github.com/tolgyirita/Capstone-EarningsCallTranscripts/blob/main/Rita_Capstone_presentation_Dec2020.pdf)

## Data gathering 

The first part of the project was to gather data. I used:
-	Request, BeautifulSoup to scrape earnings call transcripts from the [fool.com](https://www.fool.com/earnings-call-transcripts/) website and parse the scrapings into relevant columns. (21k+ transcripts collected)
-	Alpha Vantage API to collect corresponding share prices (alphavantage.co)
I matched up the transcripts with the share prices, so I could define the direction of the price movement between the call’s date closing price and the subsequent period end’s closing price (next closes working day, if weekend). I labelled the texts to “UP” and “DOWN”.


## Data cleaning

I cleaned the inconsistencies of transcripts when it was necessary, removed duplicated items, and dropped lines where I could not find corresponding share prices.


## EDA
At this phase, I explored if there are any recognisable patterns or differences of word choices between the two categories.
I made investigations into words used in the texts and also explored the use of negative words, based on [sentiment word collection](https://sraf.nd.edu/textual-analysis/resources/#LM%20Sentiment%20Word%20Lists).


## Data pre-processing and modelling

### Pre-processing
After splitting the transcripts to stratified train and test sets, I probed the following pre-processing combinations of the texts:
-	Using Count Vectorizer vs TF-IDF Vectorizer
-	Using all words across the train set corpus vs using the negative word list from the sentiment collection
-	Limiting the features based on frequency (ignoring words occurring in more than 80% of texts, or less than in 1k texts) vs no limitation.

### Models
I ran the following models on the outcomes of the above pre-processing combinations:
-	Logistic Regression
-	Random Forest Classifier
-	KNN
-	Naïve Bayes
-	SVM

### Model tuning and selection
I chose to evaluate my models based on 5-fold cross-validation using roc_auc score.
I wanted to give equal importance to both classes, therefore I preferred roc_auc scoring over accuracy as it automatically adjusts to the baseline.

I used Randomized Search and Grid Search to tune the hyperparameter to have the best outcomes.

I also experimented with adjusting the prediction threshold to increase the final accuracy of my model.

### Feature engineering
Since the initial results highlighted date features from the text, I decided to build a model where the predictors contain the year, month, and day of the week feature and also included a sentiment score of the text (weighing positive and negative words with 1 and -2 respectively).
I also checked models where I excluded the year as a predictor.


## Conclusion

The best performing model was a logistic regression on engineered features, followed by random forest and logistic regression models where words were limited by frequency.

The best model predicted share price movement with 63% accuracy, which was 11 percentage points above baseline. A couple of different models (Logistic Regression, Random Forest Classifier built on word features only) produced predictions around 58-60% accuracy.

For comparison, a fund with a 50-55% hit ratio is considered to be a good – very good fund, and 60% is believed to be needed for Warren Buffet.

I found that 
-	negatively charged words alone are not as strong predictors as unrestricted word features.
-	Models based on Count vectorizer pre-processing word slightly better than the same model on TF-IDF vectorizer
-	Models worked better if words were limited by the frequency
-	Random forest worked slightly better than logistic regression
-	Most important features contain a reference to the time aspect.

For a quick visual sum up, please see my [slides](https://github.com/tolgyirita/Capstone-EarningsCallTranscripts/blob/main/Rita_Capstone_presentation_Dec2020.pdf)

## Future plans with the project
- Evaluate the model performance based on a simulated portfolio 
- Re-label my data and categorise the shares to OUTPERFOM - UNDERPERFORM compared to a global index’s movement 
- Find a more consistent timeframe that works (52 days)
- Explore ngrams and use stemming
- Split up into more categories: 
  ~ significantly outperforms
  ~ outperforms
  ~ underperform
  ~ significantly underperforms

