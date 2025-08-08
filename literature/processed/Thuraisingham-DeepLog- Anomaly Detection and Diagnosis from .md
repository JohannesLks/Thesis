---
tags:
- literature-note
- unsupervised
- anomaly-detection
- lstm
- "#AD"
- diagnosis
- log-analysis
- root-cause-analysis
- "#DL"
- timeseries
- event-logs
- "#ID"
- workflow-construction
- online
---

# DeepLog: Anomaly Detection and Diagnosis from System Logs through Deep Learning

## Introduction
This paper introduces DeepLog, a deep neural network model using Long Short-Term Memory (LSTM) for anomaly detection in system logs. The core idea is to treat system logs as a natural language sequence, allowing DeepLog to learn normal log patterns and detect deviations. It also supports online updates to adapt to new log patterns and includes a workflow construction component for diagnosing detected anomalies.

## Key Contributions
- **LSTM-based Anomaly Detection**: Models log sequences to learn normal execution patterns and identify anomalies.
- **Online Updates**: The model can be incrementally updated with user feedback to adapt to new, normal log patterns without complete retraining.
- **Workflow Construction for Diagnosis**: Constructs workflow models from logs to help in root-cause analysis of detected anomalies.
- **Handles Concurrency**: Separates log entries from concurrent tasks to build accurate workflow models for each task.
- **Comprehensive Analysis**: Utilizes not just log keys, but also parameter values and timestamps for a more holistic anomaly detection approach.

## Methodology
The DeepLog architecture consists of three main parts:

1.  **Log Key Anomaly Detection**:
    -   Treats log key sequences as a language and uses a multi-class classifier (LSTM) to predict the next log key based on a history window.
    -   An anomaly is flagged if the actual next log key is not among the top-k most probable predictions.

2.  **Parameter Value Anomaly Detection**:
    -   Models the sequence of parameter values for each log key as a multivariate time series.
    -   An LSTM network predicts the next parameter vector, and an anomaly is detected if the Mean Square Error (MSE) between the prediction and the actual vector exceeds a statistically determined threshold.

3.  **Workflow Construction**:
    -   Separates interleaved log entries from multiple tasks using either the LSTM model's predictions or a density-based clustering approach based on log key co-occurrence.
    -   Constructs a Finite State Automaton (FSA) for each task's workflow, which aids in diagnosing the root cause of an anomaly.

## Evaluation
- **Datasets**: HDFS, OpenStack, and Blue Gene/L system logs, as well as the VAST Challenge 2011 network security logs.
- **Comparison**: Outperforms traditional methods like PCA and Invariant Mining, especially in scenarios with irregular or concurrent log patterns.
- **Performance**: Achieves high F-measures (e.g., 96% on HDFS) and demonstrates stability across different parameter settings.
- **Online Learning**: Shows significant improvement in precision by reducing false positives when online updates are enabled.

## Conclusion
DeepLog presents a robust and general-purpose framework for online anomaly detection and diagnosis from system logs. Its deep learning approach effectively captures complex patterns in log data, and its ability to perform online updates and construct diagnostic workflows makes it a practical solution for real-world systems.