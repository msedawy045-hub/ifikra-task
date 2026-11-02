# ifikra-task

Repository for the IFikra interview task: selecting and understanding one research paper.

## Selected Paper
**Text2SQL is Not Enough: Unifying AI and Databases with TAG** (2024)
- Link: https://arxiv.org/abs/2408.14717
- Benchmark code: https://github.com/TAG-Research/TAG-Bench

## Summary
Text2SQL only covers questions that can be expressed as SQL, and RAG only covers questions answered by a few records. The paper proposes **Table-Augmented Generation (TAG)**, which combines a database with an LLM in three steps:
1. Query Synthesis: the LLM turns the question into a query
2. Query Execution: the database runs the query
3. Answer Generation: the LLM uses the results and its own knowledge to answer

The authors built a benchmark showing that standard methods answer no more than 20% of queries correctly.

## Files
- `ifikra task.xlsx`: the papers list (Zotero export)
- `analysis.ipynb`: Colab notebook that loads the list, selects the paper, and summarizes it

## How to run
Open `analysis.ipynb` in Google Colab and run all cells.
