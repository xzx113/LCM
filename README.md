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



