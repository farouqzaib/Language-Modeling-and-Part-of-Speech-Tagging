EECS 595: Natural Language Processing
Assignment 1: Language Modeling and Part of Speech Tagging
Author: Fengmin Hu (hufm)

Description:
This assignment includes two parts. In part A, we will create, train and evaluate language models using the Brown corpus. In part B, we will create, train and evaluate a part-of-speech tagger. This will require implementation of the Viterbi Algorithm.

Part A: Language Model

Problem A(1):
Calculate the uni-, bi- and trigram log-probabilities of the data in "Brown_train.txt". This corresponds to implementing the calc_probabilities() funciton. In this assignment we will always use log base 2.
For each sentence, we will add the appropriate sentence start and end symbols. Use "*" as start symbol and "STOP" as end symbol (These are defined as constants START_SYMBOL and STOP_SYMBOL in the skeleton code).
This code will output the log probabilities in a file "output/A1.txt". Here's a few examples of log probabilities of uni-, bi- and trigram.

UNIGRAM natural -13.766408817
BIGRAM natural that -4.05889368905
TRIGRAM natural that he -1.58496250072

Problem A(2):
Use the language models to find the log-probability, or score, of each sentence in the Brown training data with each n-gram model. This corresponds to implementing the score() function

Make sure to accommodate the possibility that you may encounter in the sentences an n-gram that doesn’t exist in the training corpus. This will not happen now, because we are computing the log-probabilities of the training sentences, but will be necessary for question 5. The rule we are going to use is: if you find any n-gram that was not in the training sentences, set the whole sentence log-probability to -1000 (Use constant MINUS_INFINITY_SENTENCE_LOG_PROB).

Now run the perplexity script, "perplexity.py" on the output files of uni-, bi- and trigram. The script will count the words of the corpus and use the log-probabilities computed previously to calculate the total perplexity of the porpus. 

The perplexity of the corpus for the three different models as follows,

python perplexity.py output/A2.uni.txt data/Brown_train.txt
perplexity is 1052.4865859

python perplexity.py output/A2.bi.txt data/Brown_train.txt
The perplexity is 53.8984761198

python perplexity.py output/A2.tri.txt data/Brown_train.txt
The perplexity is 5.7106793082

Problem A(3):
In this step, we will implement linear interpolation among the three n-gram models we have created. This corresponds to implementing the linearscore() function.
Linear	interpolation is a method that aims to derive a better tagger by using all three uni-, bi-, and trigram taggers at once. Each tagger is given a weight described by a parameter lambda. There are some excellent methods for approximating the best set of lambdas, but for now, set all three lambdas to be equal.
Then we run the perplexity script on the output/A3.txt and the result as follows,

python perplexity.py output/A3.txt data/Brown_train.txt 
The perplexity is 12.5516094886

Problem A(4):
Compare the performance (perplexity) between the best model without interpolation and the models with linear interpolation, is the result you got expected?

Yes! As we know from textbook, the more information the N-gram gives us about the word sequence, the lower the perplexity. Tri-gram model performs best because every word is generated based on previous two words. The linear interpolation's perplexity is expected to be higher because information from the uni/bigram models is less than trigram.

Problem A(5):
Both "data/Sample1.txt" and "data/Sample2.txt" contain sets of sentences; one of the files is an excerpt of the Brown training dataset. Use previous model to score the sentences in both files. The code outputs the scores of each into "Sample1_scored.txt" and "Sample2_scored.txt". Run the perplexity script on both output files. Use these results to make an argument for which sample belongs to the Brown dataset and which does not.

The results of perplexity of Sample1 and Sample2 are as follow,

python perplexity.py output/Sample1_scored.txt data/Sample1.txt 
The perplexity is 11.1670289158

python perplexity.py output/Sample2_scored.txt data/Sample2.txt 
The perplexity is 1627571078.54

From the results above, we could find that the perplexity of Sample1 is close to the result of A(3). However, the perplexity of Sample2 is extremely high which indicates that some sentences contain tokens that don't exist in the training dataset (so that the log-probability of that sentence will be MINUS_INFINITY_SENTENCE_LOG_PROB). 
In this way, we could make a conclusion that Sample1 belongs to the Brown dataset while Sample2 does not.



Problem B: Part-of-Speech Tagging

Problem B(1):
First, you should separate the tags and words in "Brown_tagged_train.txt". This corresponds to implementing the split_wordtags() function. You'll want to store the sentences without tags in one data structure, and the tags alone in another (see instructions in the code). Make sure to add sentence start and stop symbols to both lists (of words and tags), and use the constants START_SYMBOL and STOP_SYMBOL already provided. 


Problem B(2):
Calculate the trigram probabilities for the tags. This corresponds to implementing the calc_trigrams() functions. The code outputs your result to a file "output/B2.txt".
Here are several log probabilities of the following trigrams:
TRIGRAM CONJ ADV ADP -2.9755173148
TRIGRAM DET NOUN NUM -8.9700526163
TRIGRAM NOUN PRT PRON -11.0854724592


Problem B(3):
This step is to implement a smoothing method. To prepare for adding smoothing, replace every word that occurs five times or fewer with the token "_RARE_" (use constant RARE_SYMBOL). This corresponds to implementing the calc_known() and replace_rare() functions.


Problem B(4):
Now we could calculate the emission probabilities on the modified dataset. This corresponds to implementing the calc_emission() function. 
Here are several log probabilities of the following emissions.
* * 0.0
Night NOUN -13.8819025994
Place VERB -15.4538814891
prime ADJ -10.6948327183
STOP STOP 0.0
_RARE_ VERB -3.17732085089


Problem B(5):
Now, implement the Viterbi algorithm for HMM taggers. The Viterbi algorithm is a dynamic programming algorithm that has many applications. For our purposes, the Viterbi algorithm algorithm is a comparatively efficient method for finding the highest scoring tag sequence for a given sentence. 
Using the emission and trigram probabilities, calculate the most likely tag sequence for each sentence in "Brown_dev.txt". This corresponds to implementing the viterbi() function. The tagged sentences will be output to "output/B5.txt".

Notes:
1. The output doesn't have the "_RARE_" token, but we have to count unknown words as "_RARE_" symbol to compute the probabilities inside the Viterbi algorithm.
2. When exploring the space of possibilities for the tags of given word, make sure to only consider tags with emission probability greater than zero for that given word. Also, when accessing the transition probabilities of tag trigrams, use -1000 (constant LOG_PROB_OF_ZERO in the code) to represent the log-probability of an unseen transition.

We could then use the part of speech evaluation script pos.py to compare the output file with "Brown_tagged_dev.txt" and the accuracy of our tagger as follows,
python pos.py output/B5.txt data/Brown_tagged_dev.txt 
Percent correct tags: 93.3249946254









