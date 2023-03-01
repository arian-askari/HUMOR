# HUMOR
HUMOR: **Hu**man friendy **M**odel analyzer f**O**r information **R**etrieval models


In the below there are exmaples of the usage of this implementation with emphasizing on the goal of the example. We compare the ranked list by BM25 and BERT base model on the DEV set of MSMARCO with the aim of finding below exampels:
<hr>

The goal is showing that: **BERT can be more effective than BM25.** 
1. Find one query with the below condition: \
  a. The relevant document is ranked at position 11-500 by BM25 \
  b. The same relevant document is ranked at position 1-10 by BERT \

Fist step is mutual within all examples:
```
import humor
from humor import condition
analyzer = humor(qrels_path = "dev.qrels")
```

Next:
```
bm25_condition = condition(condition_name = "bm25", start= 1, end= 500, relevant= True, ranking_path= "bm25.run")
bert_condition = condition(condition_name = "bert", start= 1, end= 10, relevant= True, ranking_path= "bert.run")
conditions = [bm25_condition, bert_condition] # each condition is for one ranker.
analyzer_report = analyzer.find(conditions)
```

So far, you find the documents based on the provided conditions. Now, let's see their content!

```
relevant_document_content = analyzer_report.get_relevant_doc_content()
relevant_document_ranks_by_bm25 = analyzer_report.get_relevant_doc_rank("bm25")
relevant_document_ranks_by_bert = analyzer_report.get_relevant_doc_rank("bert")
```
If you want check out what is the content of the document ranked at one by BM25:
```
document_at_rank1_by_bm25_content = analyzer_report.get_document_by(condition_name= "bm25", rank= 1)
```
Or, if you want check out what is the content of the document ranked at one by BERT:
```
document_at_rank1_by_bert_content = analyzer_report.get_document_by(condition_name= "bert", rank= 1)
```

<hr>

The goal is showing that: **BM25 can be more effective than BERT.** 

2.Find one query with the below condition: \
  a. The relevant document is ranked at position 1-10 by BM25 \
  b. The relevant document is ranked at position 11-1000 by BERT

Basically, this would be similar to the previous one. The conditions would be as below. The modified parts compared to previous example are in bold face.

```
bm25_condition = condition(condition_name= "bm25", start = 1, end= 10, relevant= True, ranking_path= "bm25.run")
bert_condition = condition(condition_name= "bert", start = **11**, end= **100**, relevant= True, ranking_path= "bert.run")
```


<hr>


The goal is showing that: **The re-ranking depth has an important role on retrieval effectiveness.**

Find one query with the below condition: \
  a. The relevant document is ranked at position 501-1000 by BM25 \
  b. The relevant document is ranked at position 1-10 by BERT 


```
bm25_condition = condition(condition_name= "bm25", start = 500, end= 100, relevant= True, ranking_path= "bm25.run")
bert_condition = condition(condition_name= "bert", start = 1, end= 10, relevant= True, ranking_path= "bert.run")
```


## About Humoar
Humor has been implemented as a simple and straightforward tool for homework in the Information Retrieval course at Leiden University taught by Suzan Verberne. Our goal is to provide an easy way to show the differences within the ranked list of different IR models. We use the Py_treceval library for loading the qrles and ranking files.
