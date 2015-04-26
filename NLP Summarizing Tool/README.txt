Project Description and Use Cases
       Our project is an automated sentence ranking and extraction tool made for the purpose of 
extracting the most important positive and the most important negative sentences within a corpus of 
documents. Specifically, our tool will be applied to a series of documents pertaining to movie reviews to 
get an overall understanding of the general thoughts and opinions of the reviewers. 
       We see this product being valuable to both the end user and potential relevant businesses such 
as review aggregator websites or movie theatre websites themselves. For the users, being able to quickly 
identify what the best and worst things are about a movie can save them time when deciding which 
movie to see and to spend money on. It is meant to be used as a time saver for those people that want a 
quick overview. If the sentences contain concepts that interest them, they can then read several reviews 
for more details or just make a decision based on the result.
       For review aggregator websites (such as rottentomatoes.com or imdb.com) this tool can be used 
in a multitude of ways.

1)Replace manually written task
The tool can be used as a service on the website to show full (yet disjoint) sentences 
that represent the most common positive thoughts as well as the most common 
negative thoughts on the movie. Currently, rottentomatoes.com has their own Critics 
Consensus tool, however, research indicates that is manually written by a human. 
When there are a certain number of reviews available for a movie, an editor from 
Rotten Tomatoes will read through those critic reviews and get an idea of a 
"consensus" and write a brief summary. There are some disadvantages to this method. 
First, it takes time for a human to read through multiple reviews and secondly, the 
human wouldn’t be able to read through all of the posted reviews meaning that their 
final summary may not be an accurate representation of the general consensus. Our 
tool will be able to provide a similar service (although not summarized in one sentence) 
that will free up the editor’s time to concentrate on more value added services.

2)Enhance current service (if any are currently done)
Alternatively, this tool could be used by a human editor to create better, more accurate 
summaries much faster. By using this tool, the editor will have the most common 
thoughts across all reviews, not just over the few that they have time to read. The 
sentences will cover more critics so will be more representative than just a handful. 
Once the editor has the sentences, they can put together their one line summary 
incorporating the concepts that appear more prominently. As such, with less time 
spent reading, the editor can create a faster, more representative critics consensus 
summary.
       
	Additionally, our tool is not just constrained to movie reviews. It can be applied to any review 
aggregator need such as for television shows whether it’s per episode, season, or entire series. Moving 
out of the media field, the tool could also be applied to a variety of product reviews. The data collection 
part for product reviews would be different than for media critics based on different data availability and 
APIs, but the overall process and result would still be the same. For instance, if a particular product 
feature is widely appreciated or is the cause of most problems of the product, the tool will be able to 
identify that if enough people have written about those features.

How It Works
Step 1: 	Scrape and collect all possible movie reviews with the available rottentomatoes API (for example) 
to create a corpus for each movie being reviewed.
Step 2: Each sentence in the corpus will then be compared to every other sentence within the text to 
provide a score based on the similarity between the two sentences. The first similarity score we 
tried was based on the intersection of common words between the two sentences normalized 
with the average length of the sentences. However, it must be acknowledged that this formula is 
biased towards shorter sentences. As such, we changed the similarity score to use a cosine 
similarity function in an alternative attempt to normalize the sentences. Unfortunately, in our 
implementation we were unable to include weighting with TF-IDF successfully but are still trying 
to do so for the future, likely using TfidfVectorizer in Python.
Using graph theory principles, the sentences can be seen as nodes while the scores can be seen 
as edges. The higher the score, the stronger the relationship between the two nodes.
Step 3: Using a PageRank formula, the sentences are then ranked to show which ones are the most 
“important” sentences. Importance in this instance means that these sentences are connected to 
many others through similar words which indicate that the concepts written in the sentences are 
similar. As such a sentence that contains a commonly held opinion will be ranked close to the top. 
Step 4: Compute each sentence’s “opinion score” to find the most important positive sentences and the 
most important negative sentences as they would represent some of the most commonly agreed 
upon positive and negative things about the movie. *Note: calculating the PageRank and Opinion 
Scores are done in parallel sequence as one is not dependent on the other. 
Step 5: Once the opinion scores are generated, the sentences are filtered to remove the ones that have a 
high objectivity score. If a sentence’s objectivity score is relatively high, then that indicates that 
the sentence isn’t a reviewer’s opinion, but is instead a fact or plot point about the movie. Since 
we are not a movie summarizer, and are instead interested in the opinions, we need to have 
more subjective statements. 
Step 6: We then choose the top positive and top negative ranked by how important the sentence actually 
is and how representative it is of the entire corpus.

Natural Language Processing Techniques
Throughout the development of the project, we used a variety of NLP techniques including:
•	Sentence tokenization to identify each sentence within the corpus of movies in order to 
perform the sentence comparison 
•	Word tokenization to perform the similarity calculation
•	Part of Speech identification with WordNet for each of the words within the sentence in 
order to filter and keep only nouns, adjectives, verbs, and adverbs
•	This removes unnecessary stop words while still keeping the critical words to help 
identify opinions
•	We experimented with both stemming and lemmatizing and found that using both 
techniques actually gave us the best result when trying to find the most important scores 
with PageRank.
•	Interestingly, when we applied stemming or lemmatizing or both to the dataset to 
compute for opinion, the results were very poor versus when not applying any of 
these techniques. This could be due to the fact that stemming or lemmatizing could 
have reduced the already fairly simplistic words to a point where it would be difficult 
for SentiWordNet to recognize them properly within their context.
•	SentiWordNet was used to derive the senti_synset of each word in order to compute a 
positive, negative, and objectivity score.


Experimentation with Synsets 
Tool Creation
	Throughout the development of the tool, we experimented with different uses of the synsets 
including taking only the first synset, taking all of them, taking the first two or three, and even trying to 
incorporate definitions of the synonyms. However, our best result ended up being when we only took the 
first synset of each of the words. By using more synset values into consideration, we ended up having 
longer more complex sentences which were not representative of positive or negative opinions, but 
instead seemed to be more plot-based. This result is likely due to the extra noise created by taking into 
account many synsets which could have averaged out the score to be more neutral or objective vs 
positive or negative.  

Evaluation
Method 1	
       For our first evaluation attempt, we wanted to compare our product’s result with what a human 
would output in this situation. Using the movie Frozen as an example, we inputted a series of reviews on 
the movie through the tool. In parallel, two to three of our team members manually chose what they 
believed to be the top five most common positive sentences and the top five most common negative 
sentences. We then compared the tool’s outputs to the manual summaries to generate the accuracy of 
the tool. 
       Trying to match full sentences proved to be ineffective as the evaluation resulted in a 0 score for 
both positive and negative sentences. As such, we decided to try to match on common words instead of 
the whole sentence. While the results did improve, the resulting scores were still not very high, making it 
difficult to judge whether or not the tool provided the right sentences. 
       Another evaluation method we could try would be to change the measure that we’re using to 
compare the tool’s results to the human results. If we were to envision all of the sentences within a graph 
that uses the edges as indicators of importance, then we could measure the distances within the graph of 
the sentences that were chosen. This is a fairly more complex method of evaluation that would require 
greater familiarity with graph packages in Python or another tool to allow us to effectively implement it. 
However, even with this new scoring method, we would still face similar challenges in that we would 
need humans to choose their own sentences for comparison.

Alternative Method 1
       In this method, we would still need humans to develop an output to compare with our tool. 
However, instead of extracting individual sentences, the human evaluator could create their own critic’s 
consensus summary (much like the ones on rottentomatoes) so that we could compare our sentences to 
the summary and find a score for that similarity. This would make it easier for the human validator as 
picking a few sentences to be representative can be quite difficult.
       
Alternative Method 2
	Another human-based method of evaluation would be that instead of humans extracting a 
handful of sentences amongst hundreds of sentences, we could ask them to indicate whether or not the 
sentences shown to them were relevant, helpful, or appropriate. While we would have a better indicator 
as to how our tool is performing and insight into what needs to be improved, this is a fairly expensive 
method as it will have to be redone each time.

Experimentation with Synsets
Evaluation
	In order to try and improve the evaluation of the tool’s output to a manual output, we explored 
the use of synsets. Our assumption is that by adding additional meanings or synonyms, we may have a 
better chance of matching the “theme” or “ideas” within each sentence between the automated and 
manual sentences or summary. However because the vector of words from each sentence is also 
augmented by additional synonyms, it becomes harder to match a big proportion of them and thus the 
match score is also decreasing. Two sentences of length 5 and 7 may match with 2 common words but 
when adding the two synsets of each of the words, we may end up with vectors of length 8 and 11 for 
example and still have 2 or 3 common words but overall the proportion of match is now lower than the 
first case. As such, with every increase of synset, the score decreases which does not allow for greater 
insight in the evaluation stage.
Current Status and Product Considerations
	At this stage, our product can be used as discussed to find what the general consensus of the 
most positive and most negative opinions are of the movie. Based on the evaluation, we can be fairly 
certain of its accuracy in representing the critics overall thoughts. Currently, we have chosen to show the 
top five positive and the top five negative sentences as the output to the tool. However, our experience 
testing with an overall positively received movie has shown that there are not too many negative 
sentences which causes the result to be confusing or inaccurate. As such, we still need to find the right 
number of sentences or the right threshold to enable only the relevant sentences to be seen. This is 
currently difficult to achieve because our rankings for the sentences are relative to one another with no 
clear limit as to when a sentence falls in a specific negative/positive or objective category. In the 
meantime, perhaps it would be best to just show the top two positive and the top two negative sentences 
instead.

Plans for Future Work 
	In the future, we hope to implement the capability of identifying the most common or relevant 
topics related to the movie and finding what the critics consensus is on those topics. Currently, our tool 
can only find what the most common messages are, but not related to any specific topic. In order to find 
the key topics, we will likely need to implement some sort of clustering technique. This clustering will still 
require that sentences and words be tokenized with a part of speech tag attached to each word. Once the 
sentences are clustered, the topics can be identified and a similar process of sentence ranking and 
extraction can occur for each cluster. We will need to research how to incorporate clusters for sentences 
and then implement. Note: sentences can likely be found in multiple clusters if they are talking about a 
few different concepts or thoughts from the movie, so that will need to be taken into consideration when 
implementing the clusters and deriving the results. 

Sample Data
	For the purpose of the assignment, we tested the tool on a sample of Frozen reviews. These 
reviews can be found in the attached zip file. 