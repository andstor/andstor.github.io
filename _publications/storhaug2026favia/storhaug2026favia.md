---
layout: default
title: "Favia: Forensic Agent for Vulnerability-fix Identification and Analysis"
type: Preprints
name: "storhaug2024favia"
year: 2026
doi: "10.48550/arXiv.2602.12500"
authors: "André Storhaug, and Jingyue Li"
location: "arXiv preprint arXiv:2602.12500"
code: "https://github.com/andstor/agentic-security-patch-classification-replication-package"
pdf: "https://arxiv.org/pdf/2602.12500"
tags: []
---
Identifying vulnerability-fixing commits corresponding to disclosed CVEs is essential for secure software maintenance but remains challenging at scale, as large repositories contain millions of commits of which only a small fraction address security issues. Existing automated approaches, including traditional machine learning techniques and recent large language model (LLM)-based methods, often suffer from poor precision-recall trade-offs. Frequently evaluated on randomly sampled commits, we uncover that they are substantially underestimating real-world difficulty, where candidate commits are already security-relevant and highly similar. We propose Favia, a forensic, agent-based framework for vulnerability-fix identification that combines scalable candidate ranking with deep and iterative semantic reasoning. Favia first employs an efficient ranking stage to narrow the search space of commits. Each commit is then rigorously evaluated using a ReAct-based LLM agent. By providing the agent with a pre-commit repository as environment, along with specialized tools, the agent tries to localize vulnerable components, navigates the codebase, and establishes causal alignment between code changes and vulnerability root causes. This evidence-driven process enables robust identification of indirect, multi-file, and non-trivial fixes that elude single-pass or similarity-based methods. We evaluate Favia on CVEVC, a large-scale dataset we made that comprises over 8 million commits from 3,708 real-world repositories, and show that it consistently outperforms state-of-the-art traditional and LLM-based baselines under realistic candidate selection, achieving the strongest precision-recall trade-offs and highest F1-scores.
