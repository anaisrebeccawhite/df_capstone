# Digital Futures Capstone Project: Predicting Tour de France Rider Placement Using a Random Forest Model

## INTRODUCTION
### The Tour de France is most prestigious road cycling race of the year, and has been a staple in my family’s calendar every year. In this project I aimed to create a data-driven system of prediction using different metrics from professional cycling. 
### I was aware from the start that the predictions may not be particularly accurate, since there are so many sources of unpredictability (take, for example, the 2021 incident where a spectator caused a crash by <a href="https://www.bbc.co.uk/news/world-europe-59582145" target="_blank">holding out a sign</a> ). Road cycling — as with any sport — has a huge amount of variables, and not all of them translate to available or analysable data. However, I had two objectives: 
### Firstly, I wanted my precitions to be better than both choosing randomly and a reasonable baseline: Through exploring the data, I noticed that there was a significant corellation between the rank of the riders this year and last year, so I decided to use this as a baseline — I would be happy if my model gave me a better prediction than this.  
### Secondly, I wanted to see which aspects of cycling allowed me to make the best predictions and give the best insights into performance.


## TARGET
### There are five key competitions to choose from within the Tour de France: General Classification, Best Young Rider, Points Classification (sprinting), Points Classification (mountains), and Team Classification (see <a href="https://www.cyclingweekly.com/news/racing/tour-de-france/tour-de-france-the-jerseys-59552" target="_blank">here</a> for further informaton). Though I would end up using data concerning most of these categories, what I aimed to predict was the general classification (GC), for which the winner receives the coveted yellow jersey.
### For this I used the cyclist's rank (position they finished in the race) and the time (given in increments after the winner, whose time is 0) - that are highly correlated with each other. I decided on rank rather than time, since this is less of a range than time (typically 1–150, whereas time is usually anywhere between a second and 3 hours). To make the task of predicting ranks more manageable, I split them into groups: podium, 4–25, 25–50, 50–75, 75–100, and below 100. This way I had 6 categories to predict, rather than a series from 0 to around 150.

## DATA SOURCES
### The data I used is primarely scraped from <a href="https://www.procyclingstats.com/" target="_blank">Pro Cycling Stats</a> using BeautifulSoup, and I enriched this with a dataset from <a href="https://www.kaggle.com/jaminliu/a-brief-tour-of-tour-de-france-in-numbers/data" target="_blank">Kaggle</a>. 

## MODEL CHOICE
### I decided to use a Random Forest (RF) model for my predictions, since:
### I had some categories with far more samples that others (i.e. imbalanced classes), and RFs can work with such classes;
### RFs can achieve a high accuracy with a dataset with many variables and allow me to assess their importance; and
### It was a type of model I was less comfortable with, and I wanted to reinforce my skills!

## MODEL IMPLEMENTATION 
### To begin modelling, separated out my target column (the categorised ranks) and I split my data 0.8 to 0.2 for a set to train the model on, and to test it. Since I was working with some categories that had very few samples (podium was 112) and imbalances classes, I stratified the split such that my test and train sets would have an equal proportion of each class. This avoids, for example, all of the podium samples going into the train set.
### The parameters I used for my model are as follows:
### n_estimators=150 - the number of decision trees in the forest
### max_depth = 8, - the maximum number of 'levels' or 'splits' for each tree
### min_samples_leaf=10, - the minimum number of samples that would end up on a 'leaf' of the tree once a splitting decision to be made
### random_state=24 - randomising the outcome; I set a seed value so that the answer is pseudo-random: that is, it's a random outcome, but it will be the same every time the code is run. This allows the results to be replicated.
### These are used in my first notebook; for the second I used the hyperparamenters suggested by GridSearchCV, which were 500, 9, and 8 respectively. 

## MODEL RESULTS 
### The overall accuracy has improved from the baseline of 41% to 48%: it's not huge, but it's definitely better.
### To analyse the results, I highlight precision, since this highlights False positives: those that are put in the category incorrectly.

### podium: 0.52 > 0.18 in this case, there are many more false positives in this category than our baseline. However, if we look at the recall score, it has gone up from 0.5 to 0.67, indicating fewer false negatives. That is, most of the samples that were in past podium placers were put in the correct category, but a fair few that were not were also put in the category too. This is not particularly surprising considering the sample size: if we look at the support column, we see there are only 6 samples. This is a problem that also occurs in the 75–100 category with 11 samples (low precision, high recall).
### 4–25: 0.50 > 0.64 the precision has increased in this case, as well as the recall and F1 score. This is an indication that the model has both fewer false positives and false negatives. It's also important to note that this has the highest sample size of 168, 28 times greater than the podium category.
### 25–50: 0.35 > 0.46 again, all of the metrics are higher for this category, which indicates the model will give us better predictions for this category than simple looking at last year's results.
### 50–75: 0.29 > 0.39 all metrics again are higher than the baseline for this category, with the number of samples also being higher here (143).
### 75–100: 0.25 > 0.07 as with the podium category, this category chows a dramatic drop in precision, meaning that more false positives have been included. Interestingly, though, again the recall has spiked significantly, going from 0.27 to 0.64. This shows a reduction in the number of false negatives.
### below 100: 0.63 > 0.81 This class does not follow the pattern of the others: the precision has improved on the baseline, but the recall has dropped from 0.62 to 0.59. This tells us that though the model is a fair amount better at correctly eliminating samples from this class that are from other classes, it has become slightly worse at retaining some of the samples that should be in this category.

### The top three contributing metrics to the RF model are the last year's Tour rank, last year's time, and the Tour rank 2 years ago. This isn't hugely surprising since our EDA showed that there was a fair correlation between these things.
### It's also not hugely surprising to see number of Giro wins, Vuelta wins, and Best Young Rider wins at the bottom. These were metrics chosen with the winner or podium placements in mind, not the other 5 categories that made up the majority of the dataset. On the other hand, the top 5 features are ones that all of the riders share.

## OUTCOMES
### 1. A comprehensive and interesting historical dataset of cyclists' past performance: this is included in the repository.
### 2. A deeper practical understanding of webscraping, dataframe manipulation, Random Forests, and sport predictions, as well as the Tour de France itself.
### 3. A model that improved accuracy of predictions from 41% from looking at last year's results to 48%, with greatest success in the 'below 100' and '4–25' categories.
