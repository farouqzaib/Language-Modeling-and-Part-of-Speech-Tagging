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



