# HumoR
(WORK IN PROGRESS)

HumoR: **Hum**an friendy analyzer for inf**o**rmation **R**etrieval models

Humor is a tool designed to help users compare different Information Retrieval models and understand their differences.

## Installation

For the quick installation:

```
pip install humor_ir
```

## Usage
Below are some examples of how to use Humor to compare the effectiveness of different retrieval models. Each example is focused on a specific goal, such as demonstrating the superiority of one model over another or illustrating the importance of re-ranking depth.

### Demonstrating the superiority of BERT over BM25: 
Find a query where a relevant document is ranked at position 11-500 by BM25 but is ranked 1-10 by BERT.

To get started, import Humor and define the relevant conditions:
```
import humor
from humor import condition
analyzer = humor(qrels_path = "dev.qrels")

bm25_condition = condition(condition_name= "bm25", start= 1, end= 500, relevant= True, ranking_path= "bm25.run")
bert_condition = condition(condition_name= "bert", start= 1, end= 10, relevant= True, ranking_path= "bert.run")
conditions = [bm25_condition, bert_condition] # each condition is for one ranker.
analyzer_report = analyzer.find(conditions)
```

### Retrieving Relevant Document Content and Ranking Information

Once the relevant documents have been identified, you can retrieve their content and ranking information:

```
relevant_document_content = analyzer_report.get_relevant_doc_content()
relevant_document_ranks_by_bm25 = analyzer_report.get_relevant_doc_rank("bm25")
relevant_document_ranks_by_bert = analyzer_report.get_relevant_doc_rank("bert")
```

If you want to view the content of the document ranked at 1 by BM25:
```
document_at_rank1_by_bm25_content = analyzer_report.get_document_by(condition_name= "bm25", rank= 1)
```


Or, if you want to view the content of the document ranked at 1 by BERT:

```
document_at_rank1_by_bert_content = analyzer_report.get_document_by(condition_name= "bert", rank= 1)
```


### Demonstrating the superiority of BM25 over BERT: 
Find a query where a relevant document is ranked at position  1-10 by BM25 but is ranked 11-1000 by BERT.

This example is similar to the previous one, but the conditions are different:
```
bm25_condition = condition(condition_name= "bm25", start= 1, end= 10, relevant= True, ranking_path= "bm25.run")
bert_condition = condition(condition_name= "bert", start= 11, end= 100, relevant= True, ranking_path= "bert.run")
```



### Illustrating the importance of re-ranking depth: 

Find a query where a relevant document is ranked at position 501-1000 by BM25 but is ranked 1-10 by BERT.

```
bm25_condition = condition(condition_name= "bm25", start= 500, end= 100, relevant= True, ranking_path= "bm25.run")
bert_condition = condition(condition_name= "bert", start= 1, end= 10, relevant= True, ranking_path= "bert.run")
```



## About HumoR
HumoR has been implemented as a simple and straightforward tool for a homework in the Information Retrieval course at Leiden University taught by Suzan Verberne. Our goal is to provide an easy way to show the differences within the ranked list of different IR models. We use the Pytrec_eval library for loading the qrles and ranking run files.
