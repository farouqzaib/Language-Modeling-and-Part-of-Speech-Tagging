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



