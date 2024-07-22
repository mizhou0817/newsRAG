# Introduction
This repository contains an experimental benchmark for RAG systems' evaluation. It excludes the datasets (need to download elsewhere) for evaluating RAG systems, and a tutorial on how to run the experiments on the benchmark.


# Highlights
- The benchmark supports evaluation for Chinese RAG systems
- It covers CRUD (Create, Read, Update, Delete) operations, which are used to evaluate the RAG system's ability to add, reduce, correct information, as well as to answer questions based on the retrieve information;
- It supports multiple evaluation metrics, such as ROUGE, BLEU, bertScore, RAGQuestEval, and provides a one-click evaluation function;


# Project Structure
```bash
├── data  #  This folder comprises the datasets used for evaluation.
│   │
│   ├── crud 
│   │   └── merged.json  # The complete datasets.
│   │
│   ├── crud_split
│   │   └── split_merged.json # The dataset we used for experiments in the paper.
│   │
│   └── 80000_docs
│   │    └── documents_dup_part... # More than 80,000 news documents, which are used to build the retrieval database of the RAG system.
│   │     
├── src 
│   ├── configs  # This folder comprises scripts used to initialize the loading parameters of the LLMs in RAG systems.
│   │
│   ├── datasets # This folder contains scripts used to load the dataset.
│   │
│   ├── embeddings  # The embedding model used to build vector databases.
│   │   
│   ├── llms # This folder contains scripts used to load the large language models.
│   │   ├── api_model.py  # Call GPT-series models.
│   │   ├── local_model.py # Call a locally deployed model.
│   │   └── remote_model.py # Call the model deployed remotely and encapsulated into an API.
│   │
│   ├── metric # The evaluation metric we used in the experiments.
│   │   ├── common.py  # bleu, rouge, bertScore.
│   │   └── quest_eval.py # RAGQuestEval. Note that using such metric requires calling a large language model such as GPT to answer questions, or modifying the code and deploying the question answering model yourself.
│   │
│   ├── prompts # The prompts we used in the experiments.
│   │
│   ├── quest_eval # Question answering dataset for RAGQuestEval metric.
│   │
│   ├── retrievers # The retriever used in RAG system.
│   │
│   └── tasks # The evaluation tasks.
│       ├── base.py
│       ├── continue_writing.py
│       ├── hallucinated_modified.py
│       ├── quest_answer.py
│       └── summary.py
```

# Quick Start
- Install dependency packages
```bash
pip install -r requirements.txt
```

- Start the milvus-lite service(vector database)
```bash
milvus-server
```

- Download the bge-base-zh-v1.5 model to the sentence-transformers/bge-base-zh-v1.5/ directory

- Modify config.py according to your need.

- Run quick_start.py

```bash
python quick_start.py \
  --model_name 'gpt-3.5-turbo' \
  --temperature 0.1 \
  --max_new_tokens 1280 \
  --data_path 'path/to/dataset' \
  --shuffle True \
  --docs_path 'path/to/retrieval_database' \
  --docs_type 'txt' \
  --chunk_size 128 \
  --chunk_overlap 0 \
  --retriever_name 'base' \
  --collection_name 'name/of/retrieval_database' \ 
  --retrieve_top_k 8 \
  --task 'all' \
  --num_threads 20 \
  --show_progress_bar True \
  --construct_index \ # you need to build a vector index when you use it first time
```
