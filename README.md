
# MDST Humor Detection

The goal of this project was to build and design a language model that can successfully distinguish funny text segments from humorless ones, as a binary classification.

## Sources

This repository was created for publication of the datasets useful for humor recognition in one-liners. This repository contains six datasets and the Python code used in the process of gathering the datasets. Overall there were **22,180** training samples. The six datasets (jokes are split into long and short jokes) are the following:

### 1. Humorous Jokes

These sentences are  **funny**.

Filename:  `humorous_jokes`

Filetype:  `.pickle`

Size: 11743 items

Sources:  [http://twitter.com](http://twitter.com/),  [http://www.textfiles.com/humor/](http://www.textfiles.com/humor/),  [http://www.funnyshortjokes.com/](http://www.funnyshortjokes.com/),  [http://www.laughfactory.com/jokes](http://www.laughfactory.com/jokes),  [http://www.goodriddlesnow.com/jokes/](http://www.goodriddlesnow.com/jokes/),  [http://www.onelinefun.com](http://www.onelinefun.com/)  and several other, smaller contributors

Short description: This dataset contains all humorous jokes that were gathered in the process, which can be used as positive samples for humor recognition tasks. Jokes that had a  [Jaccard similarity coefficient](https://en.wikipedia.org/wiki/Jaccard_index)  higher than or equal to 0.9 were removed in the deduplication process (Deduplication.py). This dataset was used to compile datasets 1.1 and 1.2. The first contains just the jokes in this dataset shorter than 140 characters, whereas the latter consists of all jokes containing more than 140 letters.

**  **Disclaimer**: Some of the jokes may be racist, homophobic or insulting in another way.

### 2. Reuters Headlines

These sentences are  **not funny**.

Filename:  `reuters`

Filetype:  `.pickle`

Size: 10142 items

Sources: Twitter

Short description: This dataset contains headlines tweeted by international press agency Reuters. Retweets were excluded for pre-processing purposes and to ensure the original source is known. Since the Twitter API only allows us to retrieve up to 3200 tweets (including retweets) from a single user account,   tweets were scraped from multiple Reuters Twitter accounts: "reuters", "ReutersWorld", "ReutersUK" and "ReutersScience". The first covers Reuters' top news, the second one news from all over the world, the third one news from the UK and the last one covers science. The tweets from Reuters were gathered between 18-07-2016 and 05-08-2016. The headlines that had a Jaccard similarity coefficient higher than or equal to 0.9 in comparison to other headlines in the set were removed in the deduplication process (Deduplication.py).

### 3. English Proverbs

While debatable,  for the purposes of this project, I considered these sentences to be  **not funny**.

Filename:  `proverbs`

Filetype:  `.pickle`

Size: 1019 items

Sources:  [http://www.citehr.com/32222-1000-english-proverbs-sayings-love-blind.html](http://www.citehr.com/32222-1000-english-proverbs-sayings-love-blind.html),  [http://www.english-for-students.com/Proverbs.html](http://www.english-for-students.com/Proverbs.html)

Short description: This dataset contains a large part of existing English proverbs. Deduplication has been applied to remove duplicate proverbs (Deduplication.py).

### 4. Wikipedia Sentences

These sentences are  **not funny**.

Filename:  `wiki_sentences`

Filetype:  `.pickle`

Size: 10076 items

Sources:  [http://www.cs.pomona.edu/~dkauchak/simplification/](http://www.cs.pomona.edu/~dkauchak/simplification/)

Short description: Visit source URL for information on the data itself. This file contains a random selection of Wikipedia sentences from the source file (the unsimplified one, to be specific) that were shorter than - or equal to - 140 characters. The random selection was done using  `wiki_sentence_selector.py`.

## Data Processing Methodology
 - **Lowercasing** - makes sure our model doesn't correlate the case of a character with humor when it shouldn't
-   **Lemmatizing** - converts words to their general forms, allows model to correlate topics when they are in generalized form
-   **Removing Stop Words** - Stop Words are words that commonly occur but contribute little to the meaning of the sentence (“to”, “the”,”but” ). Removing them reduces the number of words in our vocabulary, makes training faster, keeps only the most important information from each sentence
- **Other Noise Reduction** - removing features like numbers and “@” symbols from tweets to prevent any noise from impacting our data and model performance

## Exploratory Analysis
![](https://lh3.googleusercontent.com/E5bB_E3FF6lw3m9Uuku84m86jZPDjixBa8WimPaLh0aHBszYvDlv-6E8U7E6KuqAGCJVphOYZqvSsoJcCdkb5aamUb0QcbAZUpIaA9N3y2B6x4Yto312xO1r2CQ1aDUB8N8_cSfPVao=s0)After comparing the distribution of length of each joke vs each non-joke, these distributions are about the same, but there is more skew for the length of Non-jokes than for jokes. On average, Non-jokes have 55 characters, whereas jokes have 47 characters. In general, there is really no discernible difference here.

![](https://lh3.googleusercontent.com/t_WSGtaNA9ltoBk_W3tFTA7GwFGVIJCG27tLb767r_CM9eBUPWunrkg21RECd1bu-dpKZaABH-Crw1Anq533ty2xxRWThpcEbcQao5EZcw2icSYNAxBt30z-aVnGODdQ7lujNAmKxoE=s0)
The distribution of the length of words in jokes and non-jokes is about the same, with about 5 letters per word on average. This may suggest that the level of vocabulary for each of these types of phrases are about the same (assuming there is a positive correlation between level of vocabulary and word length).

![](https://lh3.googleusercontent.com/0a5yDsC9RP9F_J0IPKQyTBHaPN2_pez1j_0vNhohzBEGev1FVZgG77ZfW8yk-7DJt8gHcm7Y8aiJ2D3wrW9LBMyRmS0HA5fpayzoAsdyxrOqanOHFm9AedB-3JafUEU2O6hP0AqH5SU=s0)
After removing stopwords from the dataset, I looked at the most common words for jokes vs non-jokes, to see if there is any different in the most common words. An interesting observation to make here is how the empty string is the most common "word" for non-joke sequences. These empty strings are a result of data cleaning (removing punctuation and digits and stopwords). This may suggest that jokes in our dataset have much less noise relative to non-jokes, especially since there are about the same amount of jokes and non-jokes (10667 jokes vs 11515 ). Another difference is the higher frequency of action oriented words in jokes versus in non-jokes. Words like "call", "go", "make", "take", "think", "get" are of higher frequency for jokes. Meanwhile for non-jokes, there appear to be nouns and pronouns like "us", "time", "new", "two". From that, there may be evidence to suggest that jokes in our dataset more commonly utilize actions to create humor, more so than a non-joke does.
![](https://lh6.googleusercontent.com/rb2UG4VwaWolUUVUGF-VXKxwJXZnnGupo6LNpCbLUPv38VL23mN-cnuhPFzYcXxb0oVsjm8eB6Kp1-bYaR3-JAYL9FgOJIxZN33Gv25HvZs9iWyyF7S_IrERuOq7Q6PCwLyEN-otfLY=s0)
Next the stopwords removed, and plotted the most common stopwords for jokes versus non-jokes. One interesting feature of note is how "i" is much more common for Jokes than for Non-Jokes. This suggests that the jokes in our dataset more often utilize the first person than the non-jokes. Another interesting observation is the use of "a" much more frequently in jokes than in non-jokes. -More abstract This may suggests jokes in our dataset utilize the usage of some variable noun construct rather than a definite one (which would require the use of "the") to precede it.
## Model Selection
 -   Recurrent Neural Networks - used to be popular for NLP tasks like next sequence prediction, but were never really that strong for text classification
 -   Convolutional Neural Networks - good at extracting common features, but does not account for positional information
 - Bi-Directional Encoding Representation for Transformers (BERT) - industry standard, preserves many textual connections based on position and context + is pre-trained.
## Choice and Implementation
Ultimately, used huggingface’s transformers  library as our BERT Implementation
-   Utilized TFBertForSequenceClassification to take advantage of in-built classifier
-   Fine-tuned model with TensorFlow
Training Hyperparameters:
-   Learning Rate: 2e-6
-   Epsilon (to prevent non-zero division): 1e-5
-   Epochs: 10
-   109,483,778 parameters to train
-   Trained on 13400 data examples
-   Training Time - 1 hr (on NVidia Tesla K80)
## Performance
On the test set of 3550 samples, the model performed with a 98.5% accuracy. F1-Score: 0.99. False Positives - 0.51%. False Negatives - 0.86%

Why this might be a *bit too high*. 
 -   Concern for Overfitting? Validation loss remained low, so generalizes well to rest of dataset
 -   The model learned extremely well how to detect humor for the dataset specifically - probably will not generalize well to all humor
 -   Not too big of an issue since dataset is pretty good - but is a cause for concern
## Conclusions
### What I Learned
 -   How to clean text data for Natural Language Processing and why we should perform certain processing tasks
 -   How to avoid and detect an overfitting model and why you should avoid overfitting
 -   What are popular NLP models and how do they work
 -   How to train a machine learning model using popular libraries
 -   Steps to evaluate the accuracy of a machine learning model
 -   How to plan out larger projects efficiently
### What was Accomplished
-   Task: Given a sentence, predict if it is a joke or not
-   Investigated patterns in data to see if there was any relationships that might indicate humor.
-   Processed data to reduce noise and keep only important information,
-   Investigated common techniques used in Natural Language Processing to choose an approach to this task
-   Utilized the selected model (BERT) to complete task by training it on joke and non-joke data
-   Analyzed the model to determine its numerical and holistic performance

Thanks for reading till the end :)
