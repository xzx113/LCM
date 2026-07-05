# LCM 

Repository for Peer Review: Listen to their words and observe their deeds

⚠️ Data Anonymization Notice (Important)

Due to the sensitive nature of the original text data (containing personally identifiable information and specific domain terminology), all sensitive keywords in the Regular Expression (RegEx) patterns have been replaced with the placeholder [SENSITIVE_WORD].

This measure was taken solely to ensure confidentiality during the peer-review process. The logical structure, pattern-matching algorithms, and statistical procedures remain fully intact and reproducible.

Project Structure

The project is divided into four main modules:

Regular Expression Matching (regex_matching.py)

Identifies six categories of collection behaviors (Attitude, Contacting Friends/Family/Workplace, Visits, Lawsuits).

Calculates the frequency level of collection attempts.

Computes the density of sensitive words.

Large Language Model Coding (LCM) (lcm_analysis.py)

Utilizes an LLM (ERNIE) to distinguish between Negative Adjectives (ADJ) directed at creditors and Negative State Verbs (SV) indicating emotional distress.

Calculates the B1B score​ based on the weighted sum of these linguistic features.

Propensity Score Matching (PSM) (psm_analysis.py)

Estimates propensity scores using a Logistic Regression model.

Performs 1:1 nearest neighbor matching with a caliper restriction.

Validates covariate balance using Standardized Mean Difference (SMD).

Conducts a McNemar's test to evaluate the Average Treatment Effect on the Treated (ATT).

Machine Learning Validation (lightgbm_validation.py)

Uses LightGBM classifiers to validate the Lewin’s formula hypothesis.

Compares three models: Personality-only (P), Environment-only (E), and Personality+Environment (P+E).

Environment Setup
