# LCM

Repository for: *IF-THEN Linguistic Signals and Complaint Behavior in Financial Services: An Explanatory Association Study*

⚠️ Data Anonymization Notice
Due to the sensitive nature of the original text data, all sensitive keywords in the regular expression patterns have been replaced with the placeholder [SENSITIVE_WORD]. This measure was taken solely to ensure confidentiality during the peer-review process. The logical structure, pattern-matching algorithms, and statistical procedures remain fully intact and reproducible.

Data Notice: In the code, a Virtual ID field is used solely as a row index; it does not represent a real customer identifier.


Module Descriptions

File: regex_matching.py
Description: Regular expression-based feature engineering: collection intensity (L1–L6), collection frequency level, sensitive word density.

File: lcm_annotation.py
Description: LLM-based Linguistic Category Model (LCM) annotation: negative adjectives (ADJ) and negative state verbs (SV).

File: personality_measurement.py
Description: LLM-based personality measurement: Neuroticism and Conscientiousness.

File: rqa_dose_response.py
Description: RQ A: three-group gradient analysis of signal completeness and complaint rate.

File: psm_analysis.py
Description: RQ B and RQ C: propensity score matching (B1 and B2), sensitivity analyses, and matching boundary analysis.

File: satisfaction_proxy.py
Description: Satisfaction proxy scoring via LLM.

Notes

API keys and file paths are left blank and need to be configured before running.

The LCM and personality measurement prompts are provided in the Appendix of the manuscript.


