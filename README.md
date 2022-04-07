# InformationRetrieval
My Work in Phd Information Retrieval ( Algorithmetic Bias in Search Engines )

## Pre-Requisite
1. Use Linux interpreter to run bash commands

    For Windows Use [WSL Ubuntu Interpreter](https://www.jetbrains.com/help/pycharm/using-wsl-as-a-remote-interpreter.html#configure-wsl)

2. download Anserini [Anserini](https://github.com/castorini/anserini)
. Under anserini main path do the following :
   - Create "indexes" folder and put the required indexes of [AQUAINT - CORE17 - WAPO]
    - move resource folder from our Github account to that location for Queries
3. download Trec_Eval [Trec_Eval](https://github.com/usnistgov/trec_eval)
4. download cwl_eval [cwl_Eval](https://github.com/leifos/cwl/tree/master/scripts)
5. Set The locations of [Anserini - Trec_Eval - CWL_Eval] in general.py constants list
   ![General Constant List](https://github.com/ABDULAZIZALQATAN/Thesis/blob/main/images/general_constants.png?raw=true
   )
## Performance Measurement 
File : performanceCalculator - Test in measurePerformance
### Description : 
Run Retrieval experiment on 50 queries and get performance measurements based on input

### input
    corpus , exp , model , docs , terms , beta , parameter , index , res_file , qry_file

1. corpus = ( a = AQUAINT , c = CORE17 , w = WAPO )
2. exp = (b = Baseline , a = Axiom , r = RM3 )
3. model = ( b = BM25 , P = PL2 , l = LMD )
4. docs = Number of FbDocs - For Baseline = 0 automatically
5. terms = Number of FbTerms - For Baseline = 0 automatically
6. beta = Original Query Weight
7. parameter = Length Normalization Parameter
8. index = Index to use with Path -
        For empty use default index path Anserini_root/indexes/ (Anserini index Name Ex. lucene-index.robust05.pos+docvectors+rawdocs)
9. res_file = res_file from Experiment - if Empty String use Anserini_root/ (our Naming Ex = AQ-BM25-AX-b0.1-200K-beta0.5-10-05.res)
10. qry_file = qry_file from Experiment - if Empty String use Anserini_root/resource
### Output
    [TrecMAP,TrecBref,TrecP10,TrecNDCG,CWLMAP,CWLNDCG,CWLP10,CWLRBP0.4,CWLRBP0.6,CWLRBP0.8]


## Evaluate Res File
If you have a ready res file , you may directly evaluate it.

File : eval.py
### Performance Evaluation 
eval_performance (res_file,corpus)
#### Description 
Evaluate given res_file using 50 queries and 1000 hits using both trec_eval and cwl_eval.
(AQUAINT - CORE17 - WAPO) are allowed
#### input
- res_file : path of res_file (bash path)
- corpus : ( a = AQUAINT , c = CORE17 , w = WAPO ). for detecting the query and qrel files. 
#### output
   [TrecMAP,TrecBref,TrecP10,TrecNDCG,CWLMAP,CWLNDCG,CWLP10,CWLRBP0.4,CWLRBP0.6,CWLRBP0.8]

#### Comments
You may use eval_performance_trec or eval_performance_cwl with same input for specific results 

### Bias Evaluation 
eval_bias (res_file , corpus , b)
#### Description
- Calculate Retrievability MAP [docid-r] based on given input 
  - Produce 6 outputs [Gini - count of (r=0) docs - Total Retrievability] in terms of individual documents and author cohorts.
#### input
- res_file : path of res_file (bash path)
- corpus : ( a = AQUAINT , c = CORE17 , w = WAPO ). for detecting the query and qrel files. 
- b : retrievability b value to define the gain.
#### output
1. g(d) : Gini between individual documents.
  2. ctr_zero(d) : Count of documents with zero retrievability.
    3. rSum(d) : Total Retrievability for individual documents.
      4. g(a) : Gini between author cohorts.
        5. ctr_zero(a) : Count of authors with zero retrievability.
    6. rSum(a) : Total Retrievability for author cohorts. mostly = rSum(d) but just in case needed.