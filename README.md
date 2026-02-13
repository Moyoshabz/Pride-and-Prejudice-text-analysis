# Word Frequency Analysis

This project demonstrates a hands-on **big-data workflow** using Hadoop MapReduce on a textual dataset (*Pride and Prejudice* by Jane Austen) as a practice for distributed text analysis. The workflow covers raw ingestion, data cleaning, normalization, distributed processing, and post-processing filtering.

---

## Project Structure

## Workflow Steps

### 1. Raw Ingestion
- Downloaded the text from Project Gutenberg:
  ```bash
  wget https://www.gutenberg.org/files/1342/1342-0.txt
  
#### Uploaded to HDFS:
```bash
hdfs dfs -mkdir -p /text_analysis_input
hdfs dfs -put 1342-0.txt /text_analysis_input
