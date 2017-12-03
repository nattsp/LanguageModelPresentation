Language Model - Text Prediction Application
========================================================
author: Natalie Phillips
date: 3 December 2017
autosize: true

Text Prediction can provide improved speed and consistency
========================================================


Incorporating text prediction into corporate communication tools will improve the speed of creation and consistency in language.

I have put together a proof of concept text prediction algorithm to demonstrate the potential!

The models model is live and can be found at -
<https://nattsapps.shinyapps.io/ShinyTextPrediction/>.

I believe by using our own in-house language mode, we will be able to incorporate our industry specific terms and corporate language style across our comunications.  As often discussed communication with clients is a large part of our brand  we would like all our employees use a common style when communicating with our customers.


<font size = "5px" >
Note: All work conducted as part of the Data Science Specialisation Capstone project
</font>

Language model overview
========================================================

This prototype is built on publically available information. The corpus of documents come from mixed sources comprised of blogs, news articles, and twitter feeds.


File    |   File size   |   Number of lines |   Longest text length
------- | --------------|----------------- | ---------------------
en_US.twitter.txt   |   159 Mb   |	236,0148 |	213
en_US.news.txt  |   196 Mb  |	101,0242 |	11,384
en_US.blogs.txt |   200 Mb  |	899,288  |	40,835

The prediction model is built using the following steps
- Separated the corpus into training, holdout and test sets.
- The text was cleened to seperate sentences, remove numbers,  twitter tags and html using quenteda package.
- Built ngrams and caluclated the score of each ngram using Stupid Backoff method due to its superior speed and reasonable performance.

$$
S(w_i|w^{i-1}_{i-k+1}) = 
\begin{cases}
    \frac{\text{count}(w^{i}_{i-k+1})}{\text{count}(w^{i-1}_{i-k+1})} \; 
    \text{if count} \; (w^{i}_{i-k+1}) > 0 \\
    0.4 \, S(w_i|w^{i-1}_{i-k+2}) \; \text{otherwise}
\end{cases}
\\
S(w_i) = \frac{\text{count}(w_i)}{N}
$$
- Built the prodiction model and optimised for speed by converting to data.tables

<font size="5px">
See youtube video "4 - 6 - Interpolation - Stanford NLP - Professor Dan Jurafsky & Chris Manning".
</font>


Speed vs Accuracy
========================================================

An effective text prediction application needs to be both accurate and fast. These two often competing qualities have been addressed in this model in the following ways:

### Accuracy
- To builds a robust prediction algorithm large sample sizes of 60% were taken across each of blogs, news and twitter corpuses. 
- Quingrams and quadgrams were add to the usual trigrams and bigrams. Longer ngrams gives more context to the word prdiction providing more accurate predictions.
- Unigrams were only used to calculate the Stupid Backoff score. Unigrams prdictions on their own are  a poor predictor of the next word.
- The model was built on single sentences as a prediction coming from across two sentences is less likely to be relevant.

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
- Training set built around your companies current document set and unique language. The training sets will contain internal communications, reports and customer updates to train the model.
- Use the hold out set to optimise a coefficient to of correlated words to increase accuracy.
