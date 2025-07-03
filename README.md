# Real-Time Tweet Sentiment Analysis Pipeline

A scalable streaming data pipeline for analyzing tweet sentiment in real-time using Apache Spark Structured Streaming, Delta Lake, and machine learning inference at scale.

## Overview

This project implements a complete end-to-end streaming analytics pipeline that processes Twitter data through a medallion architecture (Bronze→Silver→Gold) and performs sentiment classification using a pre-trained transformer model. The system is designed to handle large-scale streaming data with fault tolerance, exactly-once processing, and real-time monitoring.

## Architecture

```
Raw Tweets (JSON) → Bronze Layer → Silver Layer → Gold Layer → Analytics
                      ↓             ↓             ↓
                   Raw Ingest   Preprocessing   ML Inference
```

### Data Flow

1. **Bronze Layer**: Ingests raw JSON tweet files with schema validation
2. **Silver Layer**: Extracts mentions, cleans text, and converts timestamps
3. **Gold Layer**: Applies ML model for sentiment prediction and scoring
4. **Application Layer**: Aggregates sentiment statistics by mention

## Features

- **Real-time Processing**: Spark Structured Streaming for continuous data ingestion
- **ACID Transactions**: Delta Lake for reliable data storage with time travel
- **ML at Scale**: Distributed sentiment inference using MLflow and Hugging Face models
- **Fault Tolerance**: Checkpoint-based recovery and exactly-once processing
- **Performance Monitoring**: Real-time metrics and Spark UI optimization analysis
- **Scalable Architecture**: Configurable parallelism and resource management

## Technology Stack

- **Apache Spark**: Distributed streaming and batch processing
- **Delta Lake**: ACID storage layer with versioning
- **MLflow**: Model registry and experiment tracking
- **Hugging Face Transformers**: Pre-trained sentiment analysis model
- **Databricks**: Cloud-based execution platform
- **Python**: Primary development language

## Data Schema

### Input Data (Bronze)
```json
{
  "date": "Mon Apr 06 22:19:49 PDT 2009",
  "user": "username",
  "text": "tweet content with @mentions",
  "sentiment": "positive|negative|neutral"
}
```

### Output Data (Gold)
- Original tweet metadata and sentiment
- ML-predicted sentiment scores (0-100)
- Predicted sentiment labels (POS/NEG/NEU)
- Numerical sentiment encoding for analysis

## Getting Started

### Prerequisites

- Databricks workspace with ML runtime
- Access to MLflow Model Registry
- Tweet dataset in DBFS (`/FileStore/tables/raw_tweets/`)
- Hugging Face model registered in MLflow as "HF_TWEET_SENTIMENT"

### Installation

1. Clone this repository to your Databricks workspace
2. Upload the notebook and includes directory
3. Ensure the sentiment model is registered in MLflow
4. Configure the data source path in `includes/includes.ipynb`

### Running the Pipeline

1. Open `Starter Streaming Tweet Sentiment - Spring 2025 Final Project.ipynb`
2. Run the notebook cells sequentially or use "Run All"
3. Monitor streaming progress through the built-in monitoring loop
4. View results in the Delta tables and MLflow experiments

## Configuration

Key configuration variables in `includes/includes.ipynb`:

- `TWEET_SOURCE_PATH`: Location of input tweet files
- `USER_DIR`: User-specific workspace directory
- `MODEL_NAME`: MLflow model registry name
- `HF_MODEL_NAME`: Hugging Face model identifier

## Performance Optimization

The pipeline includes several optimization strategies:

- **Dynamic Partitioning**: Shuffle partitions scaled to cluster size
- **Delta Optimization**: Automatic small file compaction
- **Memory Management**: Configurable caching and spill handling
- **Stream Checkpointing**: Fault-tolerant processing with recovery

## Monitoring and Analytics

### Real-time Metrics
- Input rows processed per second
- Processing latency per batch
- Memory and CPU utilization
- Stream health and status

### Business Analytics
- Sentiment distribution by user mentions
- Top positive/negative mentions
- Temporal sentiment trends
- Model accuracy metrics

## Results

The pipeline processes approximately 41,000+ tweet files and provides:

- **Precision**: 82.1% weighted average
- **Recall**: 55.7% weighted average  
- **F1-Score**: 66.3% weighted average
- **Processing Time**: ~70+ minutes for full dataset

## File Structure

```
├── Starter Streaming Tweet Sentiment - Spring 2025 Final Project.ipynb
├── includes/
│   ├── includes.ipynb          # Configuration and constants
│   └── utilities.ipynb         # Helper functions
├── LICENSE                     # Apache 2.0 License
├── README.md                   # This file
└── CLAUDE.md                   # Development guidance
```

## Contributing

This project was developed as a final project for DSCC202-402 Data Science at Scale. The implementation demonstrates:

- Spark Structured Streaming best practices
- Delta Lake medallion architecture
- MLflow model deployment patterns
- Performance optimization techniques
- Real-time monitoring and analytics

## License

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.

## Acknowledgments

- Course: DSCC202-402 Data Science at Scale
- Platform: Databricks Community Edition
- ML Model: [finiteautomata/bertweet-base-sentiment-analysis](https://huggingface.co/finiteautomata/bertweet-base-sentiment-analysis)
- Framework: Apache Spark and Delta Lake ecosystem