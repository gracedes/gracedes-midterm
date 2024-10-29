### ⚠️this branch contains work done after submission for my own enjoyment. if you are here to grade this assignment, please see `main` for all work done before the deadline⚠️

# Predicting Amazon Reviews Using Past Review Data & K-Nearest Neighbors
### Usefulness of Given Data Columns for Analysis
In the analysis of Amazon reviews, numerous data columns were available, such as review text, review score, helpfulness ratings, and the time of the review. I started this assignment by experimenting with each of these to investigate their impact on the predictive model. The helpfulness rating, proved to have a minimal effect on the overall prediction accuracy, but it was a consistently positive effect so I decided to keep it. The time of review ended up being a problematic feature, as throughout my testing the time data actually introduced noise, leading to lower accuracy, so it was dropped. I shifted focus towards the text content of the reviews, which promised more correlated to the corresponding score.
### Analyzing Review Text
I figured that understanding whether a review is overall “positive” or “negative” would be essential for predicting scores accurately. The interpretation of sentiment in text data relies heavily on contextual factors, making it a bit more involved than just counting key words. Positive words such as "excellent," "fantastic," and "love" typically signal favorable sentiments, while negative words like "terrible," "hate," and "worst" indicate dissatisfaction. However, the challenge lies in the context in which these words are used. A word that generally conveys positivity can become negative in a different context, such as when used sarcastically. Thus, the chosen sentiment analysis model would have to account for these nuances.
### VADER Sentiment Analysis
To navigate the complexities of sentiment analysis, I decided to use a proven method rather than make my own. I decided on the the VADER (Valence Aware Dictionary and sEntiment Reasoner) sentiment analysis algorithm, which assess sentiment based on lexical features and the context of a sentence. It considers factors such as the presence of specific words, their intensity (using capital letters, exclamation marks, etc.), and the impact of punctuation and capitalization.
VADER also incorporates contextual elements, such as negations and degree modifiers (e.g., "very good" vs. "good"). This ability makes it the most suitable choice for analyzing these reviews, where user sentiment is obviously very relevant to the score.

Of course, you can’t just read the body text of the review, since this can be left empty, or simply be less detailed than the title of the review depending on the user. As a result, I decided to get the VADER compound score for both the title and the body text. Then, to have one unified VADER value as a result, I decided to weigh the title score at 40% and body score as 60%. So, a vader score `c` could be found with the formula `c = 0.6 * (c_body) + 0.4 * (c_header)`. To further differentiate positive scores from negative, to emulate the polarity we often see in the review scores (there are more 5’s than 4’s. And more 1’s than 2’s, for example), I added a step function variable to each of these which rounds to 1 if $c$ for that part of the review is >= 0.05, 0 if 0.05 > $c$ > -0.05, and -1 otherwise. As a result, we then divide the entire formula by 2 to normalize the overall score to a range of -1 for the most negative and 1 for the most positive:

`c = (0.6 * (c_body + c_body.step) + 0.4 (c_header + c_header.step) / 2`
### Additional Text Features
In addition to sentiment analysis, other text features were explored, including word count, average word length, and vocabulary richness (measured by unique word count divided by total words). I thought these features could  add depth to the model by providing quantitative measures of the review's complexity and lexical diversity. However, while experimenting, it became evident that incorporating these additional features did not significantly improve the model's performance. In fact, they complicated the analysis and confused the KNN algorithm, sometimes making it less accurate. As a result, they were ultimately excluded from the final feature set, allowing for a more simple and streamlined model.
### Future Additions
If time had permitted further exploration, I would have incorporated a few additional features to refine the model. Specifically, I had written the code to find the average score and deviation from the average for each user and product, which would be very useful, but unfortunately I just ran out of time while waiting for the data to process. This code can be found in the `post-submission` branch, becuase honestly I want to continue experimenting a bit for my own enjoyment. Understanding how individual users rate products relative to their past behavior would likely have greatly increased the predictive power of the model.

#### Packages used in my code:
- https://github.com/cjhutto/vaderSentiment
- https://github.com/nalepae/pandarallel/

#### Sources consulted:
- https://medium.com/illumination/top-5-techniques-for-sentiment-analysis-in-natural-language-processing-c07ba5b83f64
- https://comp.social.gatech.edu/papers/icwsm14.vader.hutto.pdf
- https://medium.com/@piocalderon/vader-sentiment-analysis-explained-f1c4f9101cd9
- https://towardsdatascience.com/predicting-sentiment-of-amazon-product-reviews-6370f466fa73
- https://minimaxir.com/2017/01/amazon-spark/
