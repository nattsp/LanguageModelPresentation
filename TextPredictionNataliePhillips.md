Language Model - Text Prediction Application
========================================================
author: Natalie Phillips
date: 3 December 2017
autosize: true

<style>
    body{
        overflow:scroll;
    }
</style>

Text Prediction can provide improved speed and consistency
========================================================


Incorporating text prediction into corporate communication tools will improve the speed of creation and consistency in language.

I have put together a proof of concept text prediction algorithm to demonstrate the potential!

The model is live and can be found at -
<https://nattsapps.shinyapps.io/ShinyTextPrediction/>.

I believe by using our own in-house language mode, we will be able to incorporate our industry specific terms and corporate language style across our communications.  As often discussed, communication with clients is a large part of our brand  we would like all our employees use a common style when communicating with our customers.


<font size = "5px" >
Note: All work conducted as part of the Data Science Specialisation Capstone project
</font>

Language model overview
========================================================

This prototype is built on publicly available information. The corpus of documents come from mixed sources comprised of blogs, news articles, and twitter feeds. Souce copurses summary:


File    |   File size   |   Number of lines |   Longest text length
------- | --------------|----------------- | ---------------------
en_US.twitter.txt   |   159 Mb   |	236,0148 |	213
en_US.news.txt  |   196 Mb  |	101,0242 |	11,384
en_US.blogs.txt |   200 Mb  |	899,288  |	40,835

The prediction model is built using the following steps
- Separated the corpus into training and test sets.
- The text was cleaned to separate sentences, remove numbers,  twitter tags and html using quenteda package.
- Built ngrams and calculated the score of each ngram using Stupid Backoff method due to its superior speed and reasonable performance.

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
- Built the prediction model and optimised for speed by converting to data.tables.
- Published the shiny model on the internet.

<font size="5px">
See youtube video "4 - 6 - Interpolation - Stanford NLP - Professor Dan Jurafsky & Chris Manning".
</font>


Speed vs Accuracy
========================================================

An effective text prediction application needs to be both accurate and fast.

I found this model to be 25% for the correct prediction at the top, 35% in the top 3 and 45% in top 6 predictions. I suspect the real accuracy for similar texts may be lower over a larger test set. Using `system.time` averaged across 20 tests the average elapsed time was 0.055 seconds on my local machine.

Accuracy and speed have been addressed in this model in the following ways:

### Accuracy
- To builds a robust prediction algorithm large sample sizes of 60% of corpus documents were taken across each of blogs, news and twitter. 
- Quingrams and quadgrams were add to the usual trigrams and bigrams. Longer ngrams gives more context to the word prediction providing more accurate predictions.
- Unigrams were only used to calculate the Stupid Backoff score. Unigrams predictions on their own are  a poor predictor of the next word.
- The model was built on single sentences as a prediction across two sentences is less likely to be relevant.

### Speed
- The ngrams were trimmed to remove rarely appearing combinations of words. Any combination that appeared less than 4 times was culled. The resulting ngram tables are between 3 Mb and 16 Mb.
- The finished ngrams are stored in speed optimised `data.tables` with indexing  to accelerate table look up.

Further development
========================================================

I am looking for your investment in the next phase of development which is exciting as I plan on implementing:

- Real time word suggestions as you type.
- Training of the nGrams using our companies current document set and unique language.
- Adjust the word prediction scores to raise those words highly correlated with other words in the current document.

