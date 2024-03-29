# Yelp Sentiment Analysis
**Norman Jen**

## The Project
The purpose of this project is to use Yelp reviews to train a supervised classification model to predict positive or negative sentiment. Using NLP processing, proper vectorization techniques, and an ideal model, the goal is to make such predictions with the highest possible accuracy.

## The Data

The data being used in this project is provided by Yelp via Kaggle. Although it originally contained 5 .json files with lots of data on all sorts of aspects of Yelp including user interactions and status, business information and ratings, check-ins, and much more, this project is focused only on the review text and associated rating. For this reason and because of the size of the dataset, there is a separate notebook (yelp_dataset.ipnyb) in the repo containing my code to select the necessary information using SQL and re-structuring the relevant information into a new .csv file that can be accessed in the Data folder.

Documentation: https://www.yelp.com/dataset/documentation/main

User Agreement: Included in the Data folder of this repo

## Managing Data Size

I looped through various potential data subset sizes to determine what can be practically worked with giving my time constraints (one week) for this project while also providing a the highest possible accuracy given that limitation. After filtering out 3-star reviews and businesses that are not restaurants, I was still left with about 4.5 million rows.

The range of possible sizes I tested is 10,000 to 100,000.

Minimum: 10,000 was the smallest data set I would feel comfortable with. Anything less than 10,000 would leave a large amount of doubt as to the significance of the results, which would defeat the purpose of the project.

Maximum: Although there is always the potential of higher accuracy with more data, I felt the need to cap this loop at 100,000 as I need to test out several different models, write presentations, markdown, and more and am unable to wait 3+ hours for the code to run. If I were to have more time, I would be interested in testing out even larger datasets to work with.

## Features

After filtering out non-restaurant businesses, the features were:

x - review text, string
y (target) - sentiment, 0 (negative) for 1-2 star ratings, 1 (positive) for 4-5 star ratings

## Text Preprocessing

Preprocessing includes standard preprocessing steps such as removing stop words, making all characters lower case, and omitting numbers and punctuation.  I also tested stemming (SnowballStemmer) and lemmatization (WordNetLemmatizer) before tokenization.  Lemmatization provided a slightly higher accuracy score, with a higher run time.  However, the stemming/lemmatization transformation for a dataset of 30,000 was a difference of about 2 minutes, so I chose lemmatization for the accuracy improvement.

## Vectorization

I tested the performance of my models using both Count Vectorization and TF-IDF Vectorization.  Count Vectorization performed better in regards to accuracy when using data subsets smaller than 100,000, but it is likely that TF-IDF would perform better and provide more actionable insights if larger datasets were possible within the time constraints.

## Modeling

I examined two different Naive-Bayes models - Multinomial Naive-Bayes and Complement Naive-Bayes.  There wasn't a major difference between either model's performance when using Count Vectorization, with Multinomial being slightly better.  However, when using TF-IDF Vectorization, the Complement Naive-Bayes significantly out-performed the Multinomial Naive-Bayes.

For this project, since datasets smaller than 100,000 were used, I used Count Vectorization and Multinomial Naive-Bayes.  Overall, the model had an accuracy score of 0.9358 and performed well.

## Evaluation

I used accuracy score as the main evaluation metric for my models.  However, due to a class imbalance, I also examined ROC/AUC and looked at a full classification report to make sure that accuracy wasn't mis-representing the model's performance.  As expected, the model performed better with positive predictions than negative, but not to a point where I was dissatisfied with the model.

As a secondary understanding, I also used run time as a metric.  Because I was limited by my computer's capability and the time allotted to complete the project, it was important for me to weigh accuracy with computational expense.  As long as the run time was within reason, I prioritized accuracy.  However, if a model performed well with respect to accuracy, but had a run time that was not practical, I did place restraints to ensure the project was completed on time.

## Practical Applications

The most significant use of this project is to gain an understanding of which subjects have the highest impact on guest sentiment of a restaurant. By understanding this, the restaurant owner can appropriately allocate resources, management can adjust priorities, and staff can be trained to maximize positive sentiment.

The first step was to understand which words show the highest probability of appearing in positive reviews and negative reviews.  However, some words have high probability of being positive and being negative, like "food". This is because we used a Count Vectorizer, which doesn't take significance into account and instead just compares the frequency of a token to the target variable.

Therefore, I subtracted the probabilities of positive sentiment from negative, and vice versa to scale the probabilities and better understand which words had stronger impact on sentiment.  In other words, I put in measures to prevent words that appear too often in both categories from overly influencing my findings.  Finally, I examined the top 15 words with the highest probability in being a positive and negative restaurant review.

## Conclusion

Overall, the model performs well with a very high accuracy on its predictions. The accuracy score is 0.9358 and the AUC score is 0.96.

The examination of words and their significance to sentiment analysis helps us understand which subjects may be worth investigating to improve guest perception of a restaurant. For instance, hot soup is cheap and easy to prepare and seems to be a hit with guests! It might be worth talking to the chef and asking them to put hot soup on the menu. Similarly, "fry" and "chicken" might imply that fried chicken is another big hit. Not to mention that it is also relatively cheap!  However, we can see that waiting a long time and table placement/size are touchy. Steaks are delicious but also have a large variance in preference. Not to mention, expensive! Many people love a good steak, but it may not be the most viable menu item. These are just a few actionable insights a model like this can provide.

At this point, it is important to talk about the limitations of this project. Most importantly, the data size. Although our current model runs well and predicts accurately, that score can always be improved. In the analysis of which model and vectorization method to use, it does appear that the TF-IDF vectorization combined with Complement Naive-Bayes could potentially be the best model on a larger scale.

However, accuracy isn't the only important result that is affected by data size. The word-sentiment associations and probabilities are also affected. In particular, the under-represented class (negative). We can see that the words with the high probability of being in the positive class make more sense than those in the negative class. This is likely because of the imbalance of classes. In addition, because we used Count Vectorization, we got less information on which words have stronger significance with respect to sentiment. TF-IDF would allow for words that rarely appear at all and words that appear too often in both classes to be eliminated, which would mean I wouldn't need to do the last step laid out in this notebook.

In the end, the model is accurate and does what I set out for it to do. However, if I had more time, I believe I could have fed more information into the model and address class imbalance to provide better accuracy scores and more actionable results.