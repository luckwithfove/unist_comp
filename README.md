# FinanceRAG 


* dataset: 
    `https://www.kaggle.com/competitions/icaif-24-finance-rag-challenge/data`

* baseline: 
    `https://github.com/linq-rag/FinanceRAG`

* modified to baseline: 

    `financerag/tasks/BaseTask.py` -> `financerag/tasks/BaseTask_old.py`

    `financerag/tasks/BaseTask.py`

* new class method in `BaseTask.py`: \
   `create_hybrid_retriever` \
   `hybrid_retrieve_rerank` \
   `async_hybrid_retrieve_rerank`

* installation:

```bash
git clone https://github.com/wangjing0/ACM_FinRAG.git
cd ACM_FinRAG/FinanceRAG
pip install -r requirements.txt
```

* example use:

```python

import pandas
from financerag.tasks import (
    FinQABench,
)

def get_evalset(dataset_name):
    qrels = {}
    df_qrels = pandas.read_csv(f"../data/{dataset_name}_qrels.tsv", sep='\t')
    for _, row in df_qrels.iterrows():
        if row['query_id'] not in qrels:
            qrels[row['query_id']] = {}
        qrels[row['query_id']][row['corpus_id']] = row['score']
    return qrels

# run the task
task = FinQABench()
task.create_hybrid_retriever()
task.rerank_results = task.hybrid_retrieve_rerank(top_k=10)

# evaluate
qrels = get_evalset('FinQABench')
metrics = task.evaluate(qrels=qrels, results=task.rerank_results, k_values=[10])

# save results
output_dir = '../results/test_results'
task.save_results(output_dir=output_dir, top_k=10)
```
