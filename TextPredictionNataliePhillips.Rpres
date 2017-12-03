Text Prediction Application
========================================================
author: Natalie Phillips
date: 3 December 2017
autosize: true

Text Prediction for improved speed and consistency
========================================================

I have put together a proof of concept text prediction algorithm.
If we incorporate this into our corporate communication tools we can improve the speed and consistecy in the creation of communication messages.

The models model is live and can be found 
<https://nattsapps.shinyapps.io/ShinyTextPrediction/>.

I believe using our own in-house language model such as this text prediction algorithm we will be able to incorporate our standard industry specific terms and language style we would like all our employees use when communicating with our customers.

Communication with clients is a large part of our brand. Displaying the right word at the right time will give us a cohesive and consistent appearenace to our clients. It will also cut the time it takes for new employees to come up to speed with our way of doing business.

Note: All work conducted as part of the Data Science Specialisation Capstone project

Language model overview
========================================================

This prototype is built on publically available information which was provided to me. The corpus of documents comes from mixed sources comprised of blogs, news articles, and twitter feeds.

Here is an overview of the data:

File    |   File size   |   Number of lines |   Longest text length
------- | --------------|----------------- | ---------------------
en_US.twitter.txt   |   159 Mb   |	236,0148 |	213
en_US.news.txt  |   196 Mb  |	101,0242 |	11,384
en_US.blogs.txt |   200 Mb  |	899,288  |	40,835

When we continue to the second phase of this project we will use the companies own internal communications, reports and customer updates to train the model.

This model is built using the Stupid Backoff method due to its superior speed and reasonable performance.


Speed vs Accuracy
========================================================

An effective text prediction application needs to be both accurate and fast. If the model is not fast enought it will not be of any practical value to your employees and equally if the model rarely offers the correct next word it will not be used. These two often competing qualities have been addressed in this model in the following ways:

### Accuracy
- Large sample sizes of 60% were taken across each of blogs, news and twitter corpuses. This  builds a more robust prediction algorithm that has  a broader range of sentence constructions.
- Quingrams and quadgrams were add to the usual trigrams and bigrams. Using these longer ngrams gives more context to the word prdiction resulting in more accurate predictions.
- Unigrams were only used to calculate the Stupid Backoff score and not used by themselves to predict the next word. Unigrams prdictions on their own are such a poor predictor of the next word that I believe no prediction is better in this instance.
- The model was built on sentences rather than paragraphs. Sentences contain a single concept and a prediction coming from across two sentences is less likely to be relevant.

### Speed
- The ngrams were trimmed down to remove rarely appearing combinations of words. Any combination that appeard less than 4 times was culled. The resulting datasizes are a fraction of the size of the original training data as can be seen in the following table:

ngram   |   Size Mb
--------|-----------
quingram | 3
quadgram | 9
trigram | 16
unigram | 3

- Finished ngrams stored in speed optimised data.tables with indexing keys  to accelerate table look up during word prediction.

Further development
========================================================

The next phase of development is an exciting one. If you sponser the this next phase here are a few of the features I plan on implementing.

- As the document or email grows the word that are more correlated with the rest of the document will be presented higher in suggested word list.
- Real time updating of words as you type.
- Training set built around your companies current document set and unique language.
