# LCM

Repository for: *Detecting Complaint Risk from Customer Language in Financial Services*

This repository contains the complete analysis code for the manuscript, including:

- Regular expression-based feature engineering (collection intensity, frequency level, sensitive word density)
- LLM-based Linguistic Category Model (LCM) annotation (negative adjectives and negative state verbs)
- LLM-based personality measurement (Neuroticism, Conscientiousness, Agreeableness)
- Dose–response analysis for RQ A (three-group gradient comparison)
- Propensity Score Matching (PSM) analysis for RQ B (B1 and B2 specifications)
- Matching boundary and extreme sample analysis for RQ C

---

## ⚠️ Data Anonymization Notice

Due to the sensitive nature of the original text data (containing personally identifiable information and specific domain terminology), all sensitive keywords in the regular expression patterns have been replaced with the placeholder `[SENSITIVE_WORD]`.

This measure was taken solely to ensure confidentiality during the peer-review process. The logical structure, pattern-matching algorithms, and statistical procedures remain fully intact and reproducible.

Data Notice：In the code, a Virtual ID field is used solely as a row index for tracking observations; it does not represent a real customer identifier.

Module Descriptions

1. Regular Expression Matching (regex_matching.py)
Extracts structured behavioral features from customer call transcripts using regular expression matching.

Key Features:

Six-level collection intensity hierarchy (L1–L6):

L1: Attitude problems

L2: Contacting friends/relatives

L3: Contacting family members

L4: Contacting workplace

L5: Physical visits

L6: Lawsuit threats / legal action

Collection frequency level: Maps numeric counts and fuzzy keywords to three frequency tiers (1–3)

Sensitive word density: Counts regulatory channel mentions (e.g., "12378", "CBIRC") normalized by text length

Input: Excel file with a Text column containing customer transcripts.

Output: Excel file with appended columns: L1_attitude, L2_friend, L3_family, L4_work, L5_visit, L6_lawsuit, collection_freq_level, sensitive_word_density.

Note: All sensitive keywords have been replaced with [SENSITIVE_WORD] for confidentiality.


2. LCM-Based Linguistic Category Tagging (lcm_annotation.py)
Uses an LLM (ERNIE 3.5-8K via Baidu AI Studio API) to annotate customer transcripts for Linguistic Category Model (LCM) features.

Key Features:

Negative Adjectives (ADJ): Describes stable negative traits of the collector/platform (e.g., "shameless", "disgusting")

Only counts if directed toward the creditor/platform (ADJ_to_creditor)

Negative State Verbs (SV): Expresses negative emotional states triggered by the situation (e.g., "frustrated", "can't take it anymore")

No object-direction requirement

Priority rule: When a word can be both adjective and emotion verb (e.g., "collapse", "angry"), it is counted as SV, not ADJ

B1B score: Weighted composite of ADJ_to_creditor × 4 + SV_negative_total × 3, normalized by text length

Few-shot Prompting: The prompt includes 5 examples to guide the model's annotation logic.

Input: Excel file with a 客户文本 (customer text) column.

Output: Excel file with columns: ADJ_total, ADJ_to_creditor, SV_negative_total, B1B_score, text_length.

API Configuration: Requires setting API_KEY, BASE_URL, and MODEL_NAME in the script.


3. Personality Measurement (personality_measurement.py)
Uses an LLM (Qwen-Turbo via Alibaba Cloud DashScope API) to score personality traits via zero-shot inference on customer call transcripts.

Key Features:

Neuroticism (12 items): Measures emotional instability, anxiety, anger, vulnerability

Conscientiousness (12 items): Measures organization, goal-directedness, reliability

Domain-knowledge injection: Weighted keyword lists derived from the debt-collection context (available in weights_300_1000 and weights_full)

Scoring: Each item scored on a 5-point Likert scale (1 = strongly disagree, 5 = strongly agree), then transformed to a 0–100 scale

Input: Excel file with two text columns (text1, text2) and a 客户ID column（Virtual ID）.

Output: Excel file with columns: 客户ID（Virtual ID）, 神经质_1, 神经质_2, 尽责性_1, 尽责性_2.

API Configuration: Requires setting API_KEY, BASE_URL, and MODEL_NAME in the script.


4. Dose–Response Analysis for RQ A (rqa_dose_response.py)
Analyzes the gradient relationship between signal completeness and complaint rate.

Key Features:

Three-group classification:

No Signal: ADJ_Ratio = 0, SV_Ratio = 0, sensitive_word_density = 0

Partial Signal: At least one signal dimension present, but not both

Complete Signal: Both IF (ADJ_Ratio > 0 or SV_Ratio > 0) and THEN (sensitive_word_density > 0) present

Effect size calculation:

Risk Difference (RD) with 95% Wilson CI

Risk Ratio (RR)

Cohen's h (standardized proportion difference)

Dose–response trend test: Ordered logistic regression with signal_ordinal (0, 1, 2)

Input: Excel file with columns: ADJ_Ratio, SV_Ratio, sensitive_word_density, Spillover_flag.

Output: Excel file with three sheets (三组统计, 两两对比, 分组标签) and console output suitable for direct citation.
5. Propensity Score Matching for RQ B (psm_analysis.py)
Estimates the association between complete IF-THEN signals and complaint behavior using PSM.

Key Features:

Treatment definition: sensitive_word_density > 0 AND (ADJ_Ratio > 0 OR SV_Ratio > 0)

Propensity score estimation: Logistic regression with covariates (personality traits; environmental variables in B2)

Matching: 1:1 nearest neighbor without replacement, caliper = 0.2 × SD(logit PS)

Balance check: Standardized Mean Difference (SMD) and Variance Ratio for all covariates

Treatment effect: McNemar's exact test on matched pairs

Sensitivity analysis: Rosenbaum bounds for unobserved confounding (Γ = 1.0–5.0)

Discarded sample analysis: Descriptive statistics and complaint rates of unmatched treatment cases

Four-group comparison: No Signal, THEN only, IF only, Complete Signal (after matching)

Input: Excel file with columns: ADJ_Ratio, SV_Ratio, sensitive_word_density, Spillover_flag, plus covariates (Neuroticism, Conscientiousness, Agreeableness, L1_attitude, ..., collection_freq_level).

Output:

Console output: SMD table, common support range, ATT, McNemar p-value, Rosenbaum bounds, four-group rates

Excel file: 匹配后数据, 丢弃样本, SMD平衡性, 丢弃样本描述性统计, 匹配后描述性统计, 共同支撑, PS分布重叠统计量, 处理效应ATT, 敏感性分析, 四组投诉率, 匹配后四组对比

Figure: Common support histogram (Common_Support_Before_Matching.pdf/png)

Note: By default, the script runs B1 (personality traits only). To run B2 (personality + environment), uncomment the environmental variables in the covariates list.




