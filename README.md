Problem Statement

Modern disease surveillance is slow, reactive, and dependent on delayed clinical or lab reporting. Real outbreaks often start appearing in public signals—news stories, search patterns, social symptom discussions, weather fluctuations—days or even weeks before official confirmation. Public health teams frequently lack tools that can continuously monitor diverse real-time data and generate early warnings with high precision and supportive evidence. This delay leads to preventable spread, larger outbreaks, and higher healthcare burden.

I wanted to explore whether AI agents could monitor multiple public signals, extract symptom information, detect anomalies, and forecast outbreak risk in real time, providing early insights that could support faster public-health responses.

Why Agents?

The outbreak-detection problem requires multiple independent tasks happening at the same time:

Collecting data from multiple sources (news, search, social, weather)

Parsing unstructured text into structured medical signals

Detecting anomalies using statistical and ML-based approaches

Forecasting short-term case counts

Generating alert summaries and evidence bundles

Managing memory across days

Running automatically and continuously

This is a natural fit for a multi-agent architecture, where each agent handles one responsibility and communicates through structured data. Agents allow:

Parallelism (multiple collectors running at the same time)

Modularity (each agent is independently improvable)

Stateful operation (Memory Bank stores long-term patterns)

Observability (logging, metrics, traceability of every prediction)

The modularity and autonomy of agents made them the right solution for a real-time health-intelligence system.

What I Created (Architecture Overview)

I built a multi-agent outbreak early warning system composed of five main agents:

Data Collection Agents (Parallel)
Simulated for this prototype:

News Crawler

Social Signal Extractor

Search Trend Poller

Weather Collector

Each collects text or numeric signals, then forwards them to the classifier agent.

LLM-Powered Symptom Classification Agent
This agent converts raw free text into a structured JSON object containing:

Extracted symptoms

Estimated severity (0–1)

This is implemented as:

Gemini/OpenAI LLM call (future)

Stub classifier fallback (current)

Anomaly Detection Agent
Using rolling statistical z-score analysis with robust safeguards, this agent:

Detects abnormal spikes in symptom frequency

Marks potential outbreak onset windows

Stores patterns in long-term memory

Forecasting Agent
I implemented a short-term case prediction model using lagged linear regression.
(Prophet / LightGBM upgrade ready as an optional enhancement.)

Agent outputs:

7-day predicted trajectory

Mean predicted symptom count

High-risk flags if predicted > threshold

Alert Generation Agent
This combines anomaly + forecast evidence to generate:

Human-readable alerts

Daily dashboard entries

JSON evidence bundles

Outputs can be stored, displayed, or pushed to a notification system.

Memory & Observability

Rolling memory stored as Pandas time series

Long-term ground truth stored for evaluation

Each step prints debug logs for traceability

Demo (How the System Works)

The notebook simulates 120 days of public signals for several major Indian cities (Hyderabad, Mumbai, Chennai, Bengaluru).

The flow is:

Synthetic text signals are generated per day.

LLM classifier extracts symptoms and severity.

Daily aggregates are computed.

Rolling anomaly detection finds abnormal spikes.

A linear model forecasts the next 7 days.

Alerts are triggered if:

A spike anomaly occurs, or

Forecasted mean surpasses threshold.

Visualizations show:

Time series of symptom counts

Moving averages

Anomaly points (red dots)

The notebook outputs:

alerts.json (alert feed)

README_demo.txt

Visual time-series plots

Evaluation metrics against synthetic ground truth

The Build (Tools & Technologies Used)
Technologies

Python

Pandas, Numpy

Matplotlib / Seaborn

Scikit-learn

Prophet (optional upgrade)

Regex + JSON parsing

Kaggle Notebook environment

Agent Concepts Implemented

Multi-agent pipeline architecture

LLM classification (future: Gemini/OpenAI)

Session & state management via Pandas memory

Long-running forecasting loop

Observability (printed logs, plots, stored artifacts)

Evaluation agent for computing precision/recall/F1

Key Concepts from ADK

Although implemented in pure Python due to the Kaggle Notebook format, the system follows ADK-style agent decomposition:

Per-agent responsibilities

Sequential + parallel flows

Tool abstraction (LLM, detectors, forecasters)

Memory Bank equivalent for stateful patterns

Evaluation

I generated synthetic “ground truth outbreak windows” for different regions and evaluated:

True Positives

False Positives

False Negatives

Precision

Recall

F1 Score

The evaluation looks for:

Alerts occurring within 7 days before a real outbreak

Outbreak days not preceded by alerts (FN)

Alerts not followed by outbreaks within 7 days (FP)

This produces a realistic test of early-warning capability.

If I Had More Time

With additional time and compute, I would add:

Real-world Data Connectors
News API / RSS ingest

Google Trends automatic polling

Social symptom mining

Environmental/meteorological data feeds

Real Gemini Classification
Deploy a full LLM-symptom extraction agent:

Prompt engineering

Multi-step parsing

Confidence scoring

Better Forecasting Models
Prophet

LightGBM

Recurrent neural networks (LSTM)

Deployment via Agent Engine / Cloud Run
Turn the notebook pipeline into:

Always-on API endpoint

Streaming multi-agent runtime

Dashboard UI for public health officers

Geospatial Visualization
Heatmap updates

Animated outbreak progression

GIS-compatible exports

Conclusion

This project demonstrates how a multi-agent AI architecture can monitor public signals, detect abnormalities, forecast outbreaks, and generate actionable alerts—all on synthetic but realistic inputs. The system is modular, explainable, and designed for practical public-health decision support once real data sources and cloud deployment are added.
