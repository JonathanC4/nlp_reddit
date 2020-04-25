# Problem Statement
Can we distinguish the language used in physical therapy subreddit posts and personal training subreddit posts? The two topics are related to an extent and the terminology used in both fields are very similar. 

# Executive Summary
I retrieved a total of 3,427 documents within the last 250 days from the physical therapy and personal training subreddits via Reddit's Pushshift API. My goal was to build a model using natural language processing that could correctly classify posts to those subreddits. I utilized a count vectorizer paired with a tokenizer that lemmatized the text data. The tokens were filtered for alphanumeric characters and dropped all english stop words. I also included numeric data from the "score" and "comments" features in two of my models to gauge their impact on accuracy. To address the imbalanced classes, I used random oversampling, one of three oversampling techniques. With random oversampling, we first split our data into training and test sets. Then, we randomly select samples **with replacement** from the minority class and add them as new data points until our classes are balanced. There were several other approaches (SMOTE, ADASYN, tuning class weight) that I did not choose to do as those algorithms typically work better with continuous variables. Finally, I constructed two models to compare: a logistic regression model and a multinomial Naive Bayes model.

# Data Dictionary
|Data|Type|Description|
|---|---|---|
|**author**|object|Name of the author|
|**is_self**|object|Boolean value; True if the post is a text-based reddit post|
|**num_comments**|int|Total number of comments on reddit post|
|**score**|int|The difference of total upvotes and total downvotes|
|**selftext**|object|Text content of the reddit post|
|**subreddit**|int|Classification Objective: 1 for Physical Therapy and 0 for Personal Training|
|**title**|object|Title of the reddit post|
|**date_created**|datetime|created_utc column converted to datetime format|
|**all_text**|object|title and selftext columns concatenated|

# Findings
Because my model contained many duplicate data, I optimized for the ROC AUC score. Both models returned scores of .98 so there was minimal overlap in all the predictions. From all my model iterations, I found the below terminology to have the highest impact based on their coefficients from my models.

Top 5 Physical Therapy terms: 'dpt', 'patient', 'physical', 'therapist', 'weak’
Top 5 Personal Training terms: 'client', 'fitness', 'nasm', 'trainer', 'training'

The acronym "DPT" stands for Doctorate of Physical Therapy, the credential needed to become a physical therapist in the United States. These professionals are also doctors and treat "patients", so that term was also expected.
"NASM" refers to the National Academy of Sports Medicine, the organization that handles personal training certifications. In addition, personal trainers have a varying degree of knowledge on fitness.