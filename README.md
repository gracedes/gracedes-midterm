# Predicting Amazon Reviews Using K-Nearest Neighbors
## Usefulness of Different Data Columns for Analysis
In the analysis of Amazon reviews, numerous data columns were available, such as review text, review score, helpfulness ratings, time of review, and user/product history. Initially, these columns were assessed for their potential impact on the predictive model. The helpfulness rating, while insightful, proved to have a minimal effect on the overall prediction accuracy. This finding suggested that a review's helpfulness did not strongly correlate with the score assigned by the reviewer.
Conversely, the time of review emerged as a problematic feature. Rather than enhancing the model's predictive power, the time data actually introduced noise, leading to reduced accuracy. This prompted the decision to exclude it from the final model. The focus shifted towards leveraging the rich textual content of the reviews, which promised more significant insights into user sentiment.
## Analyzing Review Text
The primary focus of the analysis was the text of the reviews. Understanding whether a review is “positive” or “negative” is essential for predicting scores accurately. The interpretation of sentiment in textual data relies heavily on contextual factors, making it vital to identify which words and phrases signal positive or negative sentiments.
## Identifying Positive and Negative Sentiments
To determine the sentiment of a review, specific keywords can serve as indicators. Positive words such as "excellent," "fantastic," and "love" typically signal favorable sentiments, while negative words like "terrible," "hate," and "worst" indicate dissatisfaction. However, the challenge lies in the context in which these words are used. A word that generally conveys positivity can become negative in a different context, such as when used sarcastically. Thus, the sentiment analysis model must account for these nuances.
## VADER Sentiment Analysis
To navigate the complexities of sentiment detection, the VADER (Valence Aware Dictionary and sEntiment Reasoner) sentiment analysis tool was employed. VADER is particularly effective for social media and review text, as it is designed to assess sentiment based on lexical features. The methodology considers factors such as the presence of specific words, their intensity (using predefined sentiment scores), and the impact of punctuation and capitalization.
VADER also incorporates contextual nuances, such as negations and degree modifiers (e.g., "very good" vs. "good"). This ability to gauge sentiment in context makes it a suitable choice for analyzing review texts, enhancing the model's capability to predict scores accurately.
Of course, you can’t just read the body text of the review, since this can be left empty, or simply be less detailed than the title of the review depending on the user. As a result, I decided to get the VADER compound score for both the title and the body text. Then, to have one unified VADER value as a result, I decided to weigh the title score at 40% and body score as 60%. So, a vader score `c` could be found with the formula `c = 0.6 * (c_body) + 0.4 * (c_header)`. To further differentiate positive scores from negative, to emulate the polarity we often see in the review scores (there are more 5’s than 4’s. And more 1’s than 2’s, for example), I added a step function variable to each of these which rounds to 1 if $c$ for that part of the review is >= 0.05, 0 if 0.05 > $c$ > -0.05, and -1 otherwise. As a result, we then divide the entire formula by 2 to normalize the overall score to a range of -1 for the most negative and 1 for the most positive:
`c = (0.6 * (c_body + c_body.step) + 0.4 (c_header + c_header.step) / 2`
# Additional Text Features
In addition to sentiment analysis, other text features were explored, including word count, average word length, and vocabulary richness (measured by unique word count). These features could potentially add depth to the model by providing quantitative measures of the review's complexity and diversity.
However, during the experimentation phase, it became evident that incorporating these additional features did not significantly improve the model's performance. In fact, they complicated the analysis and confused the KNN algorithm. As a result, they were ultimately excluded from the final feature set, allowing for a more streamlined and effective model.
# Future Additions
If time had permitted further exploration, additional features would have been incorporated to refine the model further. Specifically, analyzing the average score and deviation from the average for each user and product could provide more robust insights. Understanding how individual users rate products relative to their past behavior would likely enhance the predictive power of the model. However, due to time constraints, this aspect of the analysis could not be implemented.
