# ner-lstm
###Named Entity Recognition using multi-layered bidirectional LSTMs and task adapted word embeddings

[Named Entity Recognition](https://en.wikipedia.org/wiki/Named-entity_recognition) is a classification problem of identifying the names of people,organisations,etc (different classes) in a text corpus. 

Previous approaches to the problems have involved the usage of hand crafted language specific features, CRF and HMM based models, gazetteers, etc. Growing interest in deep learning has led to application of deep neural networks to the existing problems like that of NER. 

We have implemented a 2 layer bidirectional LSTM network using tensorflow to classify the named entities for [CoNNL 2003 NER Shared Task](https://en.wikipedia.org/wiki/Named-entity_recognition). 

This project can be broadly divided into three components, the deep learning model, input and embeddings. 

####Deep Neural Network
We have used Google's Tensorflow to implement a bidirectional multilayered rnn cell (LSTM). The hyper parameters are present at the top in main.py. Tweaking the parameters can yield a variety of results which are worth noting.

```python
WORD_DIM = 300
MAX_SEQ_LEN = 50
NUM_CLASSES = 5
BATCH_SIZE = 64
NUM_HIDDEN = 256
NUM_LAYERS = 2
NUM_EPOCH = 100
```

We have used a softmax layer as the last layer of the network to produce the final classification outputs. We tried working with different optimizers and we found that AdamOptimzer produced the best results.

The function to calculate the F1 Scores, Prediction, Accuracy and Recall is also included in main.py. We have also included the ability to save/restore an existing model using tensorflow's saver functions.

####Input
The various codes in the embeddings directory creates the embedding pickles which will be used by input.py to give input to main.py while the code is running.
input.py contains the code that loads the embeddings for the words in the CoNLL test and train datasets. It has three functions and a dummy function to test if the network works. The embeddings are first generated using the files in the embedding folder and stored as pickle files. The input is then read from the pickle file and loaded during the training and prediction. The input should return a list of sequences of word embeddings.

####Embeddings
Each unique word should have certain number of features, these are called embeddings or also vectors. These are the input features to the neural architecture we are using.

We experimented with different embeddings and observed the results.
Random Vectors, WordVectors made by Google inc and our new generated trigram embeddings.

Random Vectors:Dimensions=300
WordVectors:Dimensions=300
TrigramVectors:Dimensions=300

TWordVec.py generates the Trigram Vectors from a supplied Corpus(Cleaned and Removed punctuations and is just a list of words separated by spaces)
getRandomEmbeddings.py generated the pickled embedding file for random vectors.
getWordEmbeddings.py generated the pickled embedding file for random vectors.
getTriEmbeddings.py generated the pickled embedding file for random vectors.

Papers on WordVectors:
https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf
http://www-personal.umich.edu/~ronxin/pdf/w2vexp.pdf
Good tutorial on WordVectors:https://www.tensorflow.org/versions/r0.9/tutorials/word2vec/index.html
