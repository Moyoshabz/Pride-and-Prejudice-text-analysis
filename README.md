# Word Frequency Analysis

This project demonstrates a hands-on **big-data workflow** using Hadoop MapReduce on a textual dataset (*Pride and Prejudice* by Jane Austen) as a practice for distributed text analysis. The workflow covers raw ingestion, data cleaning, normalization, distributed processing, and post-processing filtering.

---

## Project Structure

## Workflow Steps

### 1. Raw Ingestion
- Downloaded the text from Project Gutenberg:
```bash
  wget https://www.gutenberg.org/files/1342/1342-0.txt
```
- Uploaded to HDFS:
```bash
hdfs dfs -mkdir -p /text_analysis_input
hdfs dfs -put 1342-0.txt /text_analysis_input
```
---

### 2. Data Cleaning
- Converted all text to lowercase and removed punctuation:
```bash
mkdir -p cleaned_data
hdfs dfs -cat /text_analysis_input/1342-0.txt | tr '[:upper:]' '[:lower:]' | tr -d '[:punct:]' > cleaned_data/cleaned_book.txt
```
- Verified key words like "elizabeth" and "love"
```bash
grep elizabeth cleaned_data/cleaned_book.txt | head
grep love cleaned_data/cleaned_book.txt | head
```
---

### 3. Distributed Processing (MapReduce)
- Created a clean input directory in HDFS:
```bash
hdfs dfs -mkdir -p /text_analysis_clean
hdfs dfs -put -f cleaned_data/cleaned_book.txt /text_analysis_clean
```
- Ran Hadoop WordCount:
```bash
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar \ wordcount /text_analysis_clean /prej_output
```
---

### 4. Post-Processing Filtering
- Top N Words (After Stopword Removal)
```bash
hdfs dfs -cat /prej_output/part-r-00000 | grep -v -w -f stopwords.txt | sort -k2 -nr | head -10
```
| Theme      | Count |
| ---------- | ----: |
| elizabeth  |   599 |
| darcy      |   358 |
| bennet      |   290 |
| jane  |   261 |
| bingley |    246 |
| little |    189 |
| nothing |    182 |
| lady |    179 |
| sister |    172 |
| illustration |    163 |

- Top 5 Characters in the book
```bash
hdfs dfs -cat /prej_output/part-r-00000 | grep -w -i elizabeth
hdfs dfs -cat /prej_output/part-r-00000 | grep -w -i darcy
hdfs dfs -cat /prej_output/part-r-00000 | grep -w -i jane
```
| Word      | Count |
| --------- | ----: |
| elizabeth | 599 |
| darcy     | 358 |
| bennet    | 290 |
| jane      | 261 |
| bingley   | 246 |

- Thematic Word Counts
```bash
hdfs dfs -cat /prej_output/part-r-00000 | grep -w -i love
hdfs dfs -cat /prej_output/part-r-00000 | grep -w -i marriage
hdfs dfs -cat /prej_output/part-r-00000 | grep -w -i pride
```
| Theme      | Count |
| ---------- | ----: |
| love       |   93 |
| marriage   |   64 |
| pride      |   47 |
| prejudice  |   10 |
| friendship |   9 |

---

### Screenshot
![iamge_1](./images2/yarn.png)

## Lessons Learned
- Tokenization – Raw WordCount counts punctuation as part of words (e.g., elizabeth vs elizabeth's).
- Normalization – Converting all text to lowercase merges duplicates and ensures consistent counts.
- Text Preprocessing – Cleaning punctuation and extra symbols is essential for accurate frequency analysis.
- Why raw WordCount can be misleading – Common words like “the” dominate results without proper preprocessing.
- Big Data Workflow Practice – End-to-end workflow includes: Ingestion → Cleaning → Normalization → Distributed Processing → Post-Processing.

---

## Future Improvements
- Replace punctuation with whitespace instead of deletion to prevent glued words (e.g., elizabethand).
- Proper tokenization to handle contractions, possessives, and special characters.
- Adopt lexicons or more efficient stopword lists for faster, scalable filtering of common/noise words.
- Sentiment analysis for thematic words like love and marriage to gain richer insights.

---

## References
Project Gutenberg – Pride and Prejudice : 
https://www.gutenberg.org/files/1342/1342-0.txt 
