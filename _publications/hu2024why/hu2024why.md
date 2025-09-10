---
layout: default
title: "Why smart contracts reported as vulnerable were not exploited?"
type: Journal publications
name: "hu2024why"
year: 2023
doi: "10.1109/TDSC.2024.3520554"
authors: "Tianyuan Hu, Jingyue Li, Bixin Li, and André Storhaug"
location: "IEEE Transactions on Dependable and Secure Computing"
pdf: "https://doi.org/10.36227/techrxiv.21953189"
tags: []
---
Smart contract security is crucial for blockchain applications. While studies suggest that only a small fraction of reported vulnerabilities are exploited, no follow-up research has investigated the reasons behind this. Our goal is to understand the factors contributing to the low exploitation rate to improve vulnerability detection and defense mechanisms. We collected 136,969 real-world smart contracts and analyzed them using seven vulnerability detectors. We applied Strauss’ grounded theory to gain insights into exploitability and analyzed transaction logs to trace the historic exploitations. Among the 4,364 smart contracts flagged as vulnerable, a significant 75.25% were found to be unexploitable, meaning they were either false positives or posed no security risk. We identified ten reasons for reporting unexploitable vulnerabilities. Furthermore, we found that only 66 out of 1,080 (6%) exploitable contracts had been exploited. We compared the characteristics of exploited versus non-exploited vulnerabilities and identified five factors that may reduce the likelihood of exploitation. Our findings highlight the importance of not treating smart contracts as conventional object-oriented (OO) applications. Researchers must account for the unique features of Solidity, smart contract design principles, and execution environments. Based on these insights, we propose six recommendations to improve smart contract vulnerability detection, prioritization, and mitigation.
