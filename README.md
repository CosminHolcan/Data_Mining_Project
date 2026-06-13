# Data Mining Project - Trivia Question Answering System

A Kotlin-based information retrieval system that answers trivia questions by searching and ranking Wikipedia articles using Apache Lucene full-text search engine.

## Overview

This project implements an automated trivia question answering system similar to Jeopardy. Given a question category and a clue, the system retrieves and ranks the most relevant Wikipedia articles that likely contain the answer. Performance is evaluated using **MRR (Mean Reciprocal Rank)** metric on a dataset of 100 trivia questions.

## Features

- **Full-Text Search**: Leverages Apache Lucene for efficient indexing and querying of Wikipedia content
- **Multi-field Search**: Searches across article titles, categories, and content
- **Redirect Handling**: Correctly maps Wikipedia redirect pages to their actual content
- **Interactive Query Mode**: Execute custom searches and view top 10 ranked results
- **Batch Evaluation**: Automatically evaluate system performance on 100 predefined trivia questions
- **Metadata Extraction**: Preserves and indexes Wikipedia categories for better search accuracy

## Technology Stack

- **Language**: Kotlin 1.9.21
- **Build Tool**: Gradle
- **Search Engine**: Apache Lucene 9.9.1
  - lucene-core (indexing & search)
  - lucene-queryparser (query parsing)
  - lucene-analysis-common (English text analysis)
- **Runtime**: JVM (Java 17+)
- **IDE**: IntelliJ IDEA

## Implementation Details

**Data Processing Pipeline**:
1. Parses Wikipedia XML dumps extracting: page titles, categories, body content, and redirect links
2. Builds a Lucene index with three searchable fields (title, category, content)
3. Uses EnglishAnalyzer for proper text tokenization and stemming

**Query Strategy**:
- Combines category name and clue text: `"(clue) OR (category)"`
- Retrieves top 30 results ranked by relevance score
- Identifies correct answer by matching page titles with expected answers

**Evaluation Metric**:
- **MRR** = average of (1 / rank) for each question, where rank is the position of the first correct answer
- Tracks accuracy: percentage of questions where correct answer appears in top result

## Local Development

### Prerequisites
- Java 17 or higher
- IntelliJ IDEA (or any Kotlin-compatible IDE)
- Gradle (included via gradlew)

### Setup Instructions

1. **Configure Paths**  
   Edit [src/main/kotlin/Main.kt](src/main/kotlin/Main.kt#L23) and update these three variables with absolute paths on your system:
   ```kotlin
   val indexPath = "/path/to/index"                          // Lucene index storage
   val wikiepdiaDirectory = "/path/to/wiki-subset-20140602"  // Wikipedia XML files
   val questionsFile = "/path/to/questions.txt"              // Trivia questions
   ```

2. **Build & Run**
   - Open project in IntelliJ IDEA and let Gradle dependencies load
   - Click the "Run" button on [Main.kt](src/main/kotlin/Main.kt#L69) or execute `./gradlew run`

3. **Menu Options**
   - **Option 1**: Create Index (~3-5 minutes to parse all Wikipedia files)
   - **Option 2**: Interactive Query Mode (enter custom search text)
   - **Option 3**: Run Default Questions (evaluates all 100 questions and outputs MRR score)
   - **Option 0**: Exit

### Alternative: Use Pre-built Index

To skip the 3-5 minute indexing step, download the pre-built index from [here](https://drive.google.com/drive/folders/1kCe_kGej6Lp9rKOEPNhzOeBegjGeleLZ) and extract all files to your configured `indexPath`.

## Project Structure

```
src/main/kotlin/Main.kt           # Main application with CLI menu
src/main/resources/
  ├── questions.txt               # 100 trivia questions (category | clue | answer)
  ├── wiki-subset-20140602/       # 46 Wikipedia XML files
  └── tmp/                        # Generated Lucene index (auto-created)
```

## Performance

The system is evaluated against 100 real Jeopardy-style questions. The MRR obtained was 0.37.

