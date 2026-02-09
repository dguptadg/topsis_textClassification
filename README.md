## Overview

This project implements the **TOPSIS (Technique for Order of Preference by Similarity to Ideal Solution)** methodology to evaluate and rank pre-trained text classification models. By combining multiple performance metrics with configurable weights and impact directions, TOPSIS provides an objective framework for model selection in NLP tasks.

### Key Highlights
- **Data Acquisition**: Collection and preparation of textual datasets for comprehensive evaluation
- **Model Evaluation**: Performance assessment across multiple state-of-the-art pre-trained models
- **Metric Computation**: Calculation of accuracy alongside additional performance indicators
- **TOPSIS Score Generation**: Application of multi-criteria decision-making to derive final rankings

### Why TOPSIS?
TOPSIS is particularly valuable when:
- Multiple conflicting criteria must be considered simultaneously
- Objective model selection is required
- Trade-offs between metrics (e.g., accuracy vs. inference time) need to be balanced
- Transparency in decision-making is essential

## What is TOPSIS?

**TOPSIS** is a Multi-Criteria Decision-Making (MCDM) method that:
1. Identifies the best alternative based on simultaneous proximity to ideal solution and distance from negative-ideal solution
2. Handles multiple conflicting criteria with different importance weights
3. Provides a normalized score between 0 and 1 for ranking alternatives

### TOPSIS Advantages
- Mathematically rigorous and objective
- Easy to implement and interpret
- Handles both benefit and cost criteria
- Suitable for comparing models with multiple metrics
- Scalable to any number of alternatives and criteria

## Features

- **Multi-Model Comparison**: Evaluate multiple pre-trained text classification models
- **Comprehensive Metrics**: Accuracy, precision, recall, F1-score, inference time, model size
- **Flexible Weighting**: Customizable importance weights for each metric
- **Impact Configuration**: Define metrics as beneficial (+) or cost-based (-)
- **Rich Visualizations**: Bar charts, ranking plots, and performance heatmaps
- **Mathematical Transparency**: Step-by-step TOPSIS computation with intermediate results
- **Detailed Reporting**: Export results in CSV, JSON, and Excel formats
- **Sensitivity Analysis**: Test how weight variations affect final rankings

## Methodology

### 1. Data Acquisition Phase
- **Dataset Collection**: Gather diverse text classification datasets
- **Preprocessing**: Text cleaning, tokenization, and formatting
- **Train-Test Split**: Consistent evaluation splits across all models
- **Data Validation**: Quality checks and class distribution analysis

### 2. Model Evaluation Phase
Pre-trained models are evaluated on identical datasets using:
- **Accuracy**: Overall classification correctness
- **Precision**: Positive prediction accuracy
- **Recall**: True positive detection rate
- **F1-Score**: Harmonic mean of precision and recall
- **Inference Time**: Average prediction latency
- **Model Size**: Storage requirements (MB/GB)
- **Training Time**: Fine-tuning duration (if applicable)

### 3. Metric Computation Phase
For each model, compute:
```
Performance Vector = [Accuracy, Precision, Recall, F1, Speed, Size, ...]
```

### 4. TOPSIS Score Generation Phase

**Step 1: Normalize Decision Matrix**
```
Normalized value = Value / sqrt(Sum of squares)
```

**Step 2: Apply Weights**
```
Weighted normalized value = Normalized value × Weight
```

**Step 3: Determine Ideal Solutions**
- **Ideal Best (A+)**: Best value for each criterion
- **Ideal Worst (A-)**: Worst value for each criterion

**Step 4: Calculate Distances**
- **Distance to Ideal Best**: Euclidean distance to A+
- **Distance to Ideal Worst**: Euclidean distance to A-

**Step 5: Compute TOPSIS Score**
```
TOPSIS Score = Distance to A- / (Distance to A+ + Distance to A-)
```

## Usage

### Basic Usage

```bash
# Run TOPSIS analysis with default parameters
python topsis_analysis.py
```

### Advanced Configuration

```bash
# Custom weights and impacts
python topsis_analysis.py --weights 0.3,0.2,0.2,0.15,0.1,0.05 --impacts +,+,+,+,-,-

# Specify input dataset
python topsis_analysis.py --dataset ./data/sentiment_analysis.csv

# Generate detailed report
python topsis_analysis.py --report --output ./results/
```

### Code Example

```python
from topsis_analyzer import TOPSISAnalyzer
from model_evaluator import ModelEvaluator

# Step 1: Evaluate models
evaluator = ModelEvaluator(dataset_path='data/reviews.csv')
results = evaluator.evaluate_models([
    'bert-base-uncased',
    'distilbert-base-uncased',
    'roberta-base',
    'albert-base-v2',
    'xlnet-base-cased'
])

# Step 2: Configure TOPSIS parameters
weights = [0.25, 0.20, 0.20, 0.15, 0.10, 0.10]  # Sum = 1.0
impacts = ['+', '+', '+', '+', '-', '-']  # +: benefit, -: cost

# Step 3: Run TOPSIS analysis
analyzer = TOPSISAnalyzer(
    data=results,
    weights=weights,
    impacts=impacts
)
rankings = analyzer.compute_topsis()

# Step 4: Visualize and export results
analyzer.plot_rankings()
analyzer.export_results('results/topsis_rankings.csv')
```

### Input Format

**Performance Metrics CSV:**
```csv
Model,Accuracy,Precision,Recall,F1-Score,Inference_Time,Model_Size
BERT,0.92,0.90,0.93,0.91,45,440
DistilBERT,0.89,0.87,0.88,0.87,22,260
RoBERTa,0.94,0.92,0.95,0.93,50,500
ALBERT,0.91,0.89,0.92,0.90,38,180
XLNet,0.93,0.91,0.94,0.92,55,420
```

## Pre-Trained Models Evaluated

### 1. BERT (Bidirectional Encoder Representations from Transformers)
- **Developer**: Google
- **Architecture**: Transformer encoder
- **Parameters**: ~110M (base), ~340M (large)
- **Strengths**: Strong contextual understanding, bidirectional encoding
- **Use Cases**: General text classification, named entity recognition

### 2. DistilBERT
- **Developer**: Hugging Face
- **Architecture**: Distilled BERT
- **Parameters**: ~66M (40% smaller than BERT-base)
- **Strengths**: Faster inference, lower memory footprint
- **Use Cases**: Resource-constrained deployments, real-time applications

### 3. RoBERTa (Robustly Optimized BERT)
- **Developer**: Facebook AI
- **Architecture**: Optimized BERT variant
- **Parameters**: ~125M (base), ~355M (large)
- **Strengths**: Improved training methodology, better performance
- **Use Cases**: High-accuracy requirements, research applications

### 4. ALBERT (A Lite BERT)
- **Developer**: Google Research
- **Architecture**: Parameter-efficient BERT
- **Parameters**: ~12M (base) - shared across layers
- **Strengths**: Memory efficient, faster training
- **Use Cases**: Large-scale deployments, limited resources

### 5. XLNet
- **Developer**: Google/CMU
- **Architecture**: Autoregressive pre-training
- **Parameters**: ~110M (base), ~340M (large)
- **Strengths**: Captures bidirectional context, permutation language modeling
- **Use Cases**: Complex language understanding tasks

## TOPSIS Parameters

### Weight Assignment
Weights represent the relative importance of each criterion. Requirements:
- **Range**: Must be non-negative values (strictly > 0)
- **Sum**: Typically normalized to sum to 1.0
- **Interpretation**: Higher weight = more important criterion

**Example Weight Configurations:**

| Configuration | Accuracy | Precision | Recall | F1-Score | Inference Time | Model Size |
|---------------|----------|-----------|--------|----------|----------------|------------|
| Balanced | 0.20 | 0.20 | 0.20 | 0.20 | 0.10 | 0.10 |
| Accuracy-Focused | 0.40 | 0.15 | 0.15 | 0.15 | 0.10 | 0.05 |
| Speed-Optimized | 0.15 | 0.15 | 0.15 | 0.15 | 0.30 | 0.10 |
| Production-Ready | 0.25 | 0.20 | 0.20 | 0.15 | 0.15 | 0.05 |

### Impact Direction
Impact defines whether a criterion should be maximized or minimized:
- **Positive (+)**: Higher values are better (e.g., accuracy, precision, recall, F1-score)
- **Negative (-)**: Lower values are better (e.g., inference time, model size, error rate)

**Typical Impact Assignments:**
```python
impacts = {
    'Accuracy': '+',       # Maximize
    'Precision': '+',      # Maximize
    'Recall': '+',         # Maximize
    'F1-Score': '+',       # Maximize
    'Inference_Time': '-', # Minimize
    'Model_Size': '-'      # Minimize
}
```

## Results & Analysis

### Tabular Results

The TOPSIS analysis produces comprehensive rankings with detailed scores:

<img width="1300" height="500" alt="reordered_topsis_table" src="https://github.com/user-attachments/assets/07bbe64a-52a8-4ca5-887c-08047900c7c0" />

**Table Columns Explained:**
- **Model**: Pre-trained model identifier
- **Accuracy, Precision, Recall, F1-Score**: Classification performance metrics
- **Inference Time**: Average prediction latency (ms)
- **Model Size**: Storage requirements (MB)
- **TOPSIS Score**: Final ranking score (0-1, higher is better)
- **Rank**: Final position based on TOPSIS score

### Visual Analysis

#### 1. TOPSIS Model Rankings
<img width="591" height="702" alt="topsis_model_ranking_rescaled" src="https://github.com/user-attachments/assets/3d6fc6a7-6fab-485d-9623-e2cb40c245a3" />

**Interpretation:**
- Models ranked from highest to lowest TOPSIS score
- Horizontal bar length represents relative ranking strength
- Color coding indicates performance tiers

#### 2. Accuracy Comparison
<img width="1050" height="600" alt="accuracy_comparison_thin_bars" src="https://github.com/user-attachments/assets/c483ee53-6630-48da-9121-6e273d4dbe5c" />

**Insights:**
- Direct comparison of classification accuracy across models
- Baseline reference line for target accuracy
- Performance variance visualization

### Key Findings

1. **Best Overall Model**: [Model with highest TOPSIS score]
   - Balanced performance across all metrics
   - Optimal trade-off between accuracy and efficiency

2. **Fastest Model**: [Model with lowest inference time]
   - Best for real-time applications
   - Suitable for high-throughput scenarios

3. **Most Accurate Model**: [Model with highest accuracy]
   - Best for applications where precision is critical
   - May sacrifice speed/size for performance

4. **Most Efficient Model**: [Smallest model size with acceptable performance]
   - Ideal for edge deployment
   - Resource-constrained environments

5. **Sensitivity to Weights**:
   - Rankings stable/unstable under weight variations
   - Critical metrics identified through sensitivity analysis

## Performance Metrics

### Detailed Metrics Table

| Model | Accuracy | Precision | Recall | F1-Score | Inference Time (ms) | Model Size (MB) | TOPSIS Score | Rank |
|-------|----------|-----------|--------|----------|---------------------|-----------------|--------------|------|
| BERT | 0.92 | 0.90 | 0.93 | 0.91 | 45 | 440 | 0.XXX | X |
| DistilBERT | 0.89 | 0.87 | 0.88 | 0.87 | 22 | 260 | 0.XXX | X |
| RoBERTa | 0.94 | 0.92 | 0.95 | 0.93 | 50 | 500 | 0.XXX | X |
| ALBERT | 0.91 | 0.89 | 0.92 | 0.90 | 38 | 180 | 0.XXX | X |
| XLNet | 0.93 | 0.91 | 0.94 | 0.92 | 55 | 420 | 0.XXX | X |

*Note: Fill in actual values from your experimental results*

### Metric Definitions

- **Accuracy**: (TP + TN) / (TP + TN + FP + FN)
- **Precision**: TP / (TP + FP)
- **Recall**: TP / (TP + FN)
- **F1-Score**: 2 × (Precision × Recall) / (Precision + Recall)
- **Inference Time**: Average prediction latency per sample
- **Model Size**: Storage space required for model weights
