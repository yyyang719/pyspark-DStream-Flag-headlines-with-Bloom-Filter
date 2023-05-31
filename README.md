# Flag-headlines-with-Bloom-Filter
AFINN is a lexicon of English words rated for valence with an integer between minus five (negative) and plus five (positive). The lexicon used in this small project is from https://raw.githubusercontent.com/fnielsen/afinn/master/afinn/data/AFINN-en-165.txt'.
The purpose of this project is to flag headlines that include words with AFINN scores of -4 or -5.
There are three files:
1. 'generate-bloom-filter.py': Using two hash functions to create a hash table of size 1000 bit-vector with the 'bad' words (63 words with AFINN scores of -4 or -5). Also, the probability of binary data getting corrupt is high in its raw form, So binary-to-text encoding: Base64 is used to convert hash table into text.
2. 'bloomfilter64.txt': the transformed text form of hash table that will be uploaded to the hadoop file system on google cloud platform.
3. 'dstream-with-bloomfilter.py':\ 
This code is tested in a GCP cluster and on a configuration with 1 master and 0 workers.\
DStreams are generated by the code from https://github.com/singhj/big-data-repo/blob/main/spark-examples/news-feeder.py, which produces news headline with 1 second window. The generated Data streams are feeded into pyspark processing code using netcat on the master machine of a cluster:\
The sending terminal with command: ~/yourfoldername/spark-examples/news-feeder.py | nc -lk 9999 (The code file 'news-feeder.py' is provided by the link mentioned above)\
The Receiving terminal with command: 'which spark-submit' ~/yourfoldername/dstream-with-bloomfilter.py localhost 9999 (dstream-with-bloomfilter.py is created by me with pyspark dstream)\
The headlines with flagged words in them will be printed on the terminal of master.
