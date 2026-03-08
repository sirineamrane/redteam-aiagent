# redteam-aiagent

# RED-TEAM AGENT

---

## Week 1 — Foundations (14h)

**Goal : environment running + first naive attacker producing output**

| Day | P1 Orchestration | P2 RAG & Memory | P3 Attacker Agent | P4 Defender Agent | P5 Evaluation & Metrics |
| --- | --- | --- | --- | --- | --- |
| D1 | Setup GitHub repo, project structure, define module interfaces | Download AdvBench + HarmBench, explore JSON format | Install LangGraph + LangChain, test basic LLM call | Read defender literature : guardrails, input classifiers | Read HarmBench paper, list all available metrics |
| D2 | Define system architecture diagram, data flow between modules | First embeddings on AdvBench prompts, setup ChromaDB | Naive attacker v0 : takes a prompt, returns a hostile variation | Define defender input/output schema aligned with P3 output | Define ASR formula, harmfulness scoring rubric |
| D3 | Review P3 attacker v0, validate integration interface | Index all AdvBench prompts into ChromaDB | Integrate API key management, retry logic, error handling | Build LLM classifier skeleton : safe vs unsafe binary | Setup evaluation harness : automated pipeline to score N prompts |
| D4 | Define shared memory schema : what attacker + defender both read/write | Test retrieval : query "roleplay jailbreak" → top 5 similar prompts | Add basic memory to attacker : stores which prompts succeeded | Implement keyword-based baseline defender for comparison | Manual scoring of 20 attacker v0 outputs, calibrate rubric |
| D5 | Full integration test : attacker v0 → memory → log | Tune ChromaDB : experiment with top-k and similarity threshold | End-to-end test attacker v0 on 10 diverse prompts | Test classifier on 10 safe + 10 hostile prompts | Calculate first ASR on 10 prompts, document baseline |

**W1 Deliverable :** Attacker v0 running, ChromaDB indexed, first ASR score, all module interfaces agreed.

---

## Week 2 — Full Attacker Agent (14h)

**Goal : 3 attack strategies, RAG-assisted mutation, automatic scorer**

| Day | P1 Orchestration | P2 RAG & Memory | P3 Attacker Agent | P4 Defender Agent | P5 Evaluation & Metrics |
| --- | --- | --- | --- | --- | --- |
| D6 | Design strategy selector : agent picks strategy based on target model + past memory | RAG-assisted attack : retrieve similar successful past attacks to seed new generation | Implement Strategy 1 : roleplay jailbreak | Implement LLM classifier v2 : add confidence score | Setup Perspective API (toxicity scoring, free) |
| D7 | Define adaptive attacker state machine in LangGraph | RAG v2 : filter by attack category metadata | Implement Strategy 2 : indirect prompt injection | Implement semantic similarity detector : cosine distance from known attacks | LLM-as-judge scorer v1 : GPT rates harmfulness 1-10 |
| D8 | Integrate strategy selector with P3 agent loop | Index all Week 1 successful attacks, tag with strategy type | Implement Strategy 3 : genetic mutation (PAIR-inspired) | Integrate Perspective API into defender scoring | Calibrate LLM-as-judge on 50 labeled examples |
| D9 | Full attacker v1 integration : selector + 3 strategies + memory | Tune RAG : does retrieving similar attacks improve mutation quality ? | Attacker v1 : full loop with all 3 strategies selectable | Defender v1 : classifier + semantic detector + Perspective | Compare Perspective API vs LLM-as-judge agreement rate |
| D10 | Transferability module : run attacker v1 on GPT-4o-mini then on Claude | Memory consolidation : compress successful attack patterns into reusable templates | Test attacker v1 : 30 prompts across 3 strategies | Test defender v1 standalone on 50 attack + 50 benign prompts | ASR breakdown by strategy, precision/recall for defender v1 |

**W2 Deliverable :** Attacker with 3 strategies + RAG mutation + memory. Defender v1. Automatic scorer. First comparative metrics.

---

## Week 3 — Defender Agent (14h)

**Goal : full defender with detection, patch, and adaptive learning**

| Day | P1 Orchestration | P2 RAG & Memory | P3 Attacker Agent | P4 Defender Agent | P5 Evaluation & Metrics |
| --- | --- | --- | --- | --- | --- |
| D11 | Design defender LangGraph state machine : detect → classify → patch → log | Build defender knowledge base : embed all HarmBench attack patterns | Adversarial stress test : generate 50 diverse attacks explicitly designed to fool defender v1 | Implement patch module v1 : rewrite detected hostile prompt into safe equivalent | Precision, recall, F1 of defender v1 on HarmBench test split |
| D12 | Define shared adversarial memory : attacker reads defender failures, defender reads attacker successes | Dynamic RAG update : each new attack seen by defender → automatically indexed | Refine attacker based on P5 failure analysis : which prompts pass defender ? | Patch module v2 : verify patched prompt still satisfies original user intent | False positive rate : benign prompts wrongly blocked by defender |
| D13 | Integrate adaptive attacker-defender feedback loop | RAG deduplication : avoid indexing near-duplicate attacks | Implement whitebox attack : attacker knows defender uses semantic similarity, designs around it | Implement multi-layer defense : classifier + RAG + Perspective must all agree to block | Patch quality evaluation : human rubric for intent preservation |
| D14 | Test full feedback loop : 5 rounds attacker-defender, manual review | Defender memory persistence : save RAG state between sessions | Implement attack mutation based on defender feedback | Defender v2 : adaptive threshold tuning based on observed false positive rate | Defender v2 evaluation : compare v1 vs v2 on same 100 prompts |
| D15 | Full attacker v2 + defender v2 integration test, 20-round dry run | RAG performance report : retrieval latency, quality metrics | Attacker v2 finalized | Defender v2 finalized | Full comparative table : defender v1 vs v2, attacker v1 vs v2 |

**W3 Deliverable :** Adaptive attacker v2, adaptive defender v2, feedback loop running, full metrics.

---

## Week 4 — Orchestrated Duel (16h)

**Goal : automated multi-round duel, real-time dashboard, scoring engine**

| Day | P1 Orchestration | P2 RAG & Memory | P3 Attacker Agent | P4 Defender Agent | P5 Evaluation & Metrics |
| --- | --- | --- | --- | --- | --- |
| D16 | Build LangGraph duel orchestrator : manages round state, turn-taking, score updates | Shared duel memory : both agents update same knowledge base each round | Attacker duel mode : reads last round result, adapts strategy | Defender duel mode : reads attack log, tightens detection threshold | Build Streamlit duel dashboard skeleton |
| D17 | Multi-round loop : 10 automated rounds, full JSON logs exported | RAG round-by-round analytics : track which attack patterns keep succeeding | Implement attacker escalation : progressively more aggressive each round | Implement defender escalation : tighten rules after 3 consecutive losses | Streamlit : live ASR curve per round |
| D18 | Add duel scoring engine : win rate per strategy, cumulative defender score | RAG cross-agent analysis : which patterns are hardest to both attack and defend ? | Attacker final tuning for duel performance | Defender final tuning for duel performance | Streamlit : strategy heatmap, win/loss tracker |
| D19 | 50-round stress test, monitor memory leaks and API costs | RAG stability under 50 rounds of continuous updates | Bug fixes from stress test | Bug fixes from stress test | Collect full 50-round metrics : ASR evolution, defense rate evolution |
| D20 | Transferability test module : duel-trained attacker vs different model | Export all RAG states for reproducibility | Transferability attack run | Transferability defense evaluation | Transferability metrics : ASR delta across models |

**W4 Deliverable :** Full duel system, 50-round logs, Streamlit dashboard, transferability results.

---

## Week 5 — Rigorous Evaluation (12h)

**Goal : ablation study, baseline comparisons, publishable result tables**

| Day | P1 Orchestration | P2 RAG & Memory | P3 Attacker Agent | P4 Defender Agent | P5 Evaluation & Metrics |
| --- | --- | --- | --- | --- | --- |
| D21 | Design full ablation study : 4 system variants | Ablation : run without dynamic RAG → static RAG only | Baseline attacker : random prompt mutation, no strategy, no memory | Baseline defender : keyword filter only | Run all ablations on HarmBench full test set |
| D22 | Ablation variant 2 : no memory on either agent | Ablation : no RAG at all → LLM-only retrieval | Baseline attacker 2 : single fixed strategy, no adaptation | Baseline defender 2 : Perspective API only, no RAG | Build full comparison table : 4 ablations × 4 metrics |
| D23 | Reproducibility test : fresh clone, full system running in under 10 min | Final RAG documentation : architecture, parameters, index size | Code cleanup, docstrings, type hints | Code cleanup, docstrings | Generate all figures : ASR curves, ablation bar charts, heatmaps |
| D24 | Final integration review | requirements.txt, environment.yml | Final attacker code review | Final defender code review | Final numbers verified, cross-checked |

**W5 Deliverable :** Full ablation, 3 baselines, final verified metrics, clean reproducible codebase.

---

## Week 6 — Integration, Demo, Report (10h)

| Day | P1 | P2 | P3 | P4 | P5 |
| --- | --- | --- | --- | --- | --- |
| D25 | Write Introduction + System Architecture section | Write RAG & Memory section | Write Attacker Agent section | Write Defender Agent section | Write Evaluation section, assemble report |
| D26 | Full report review, fix inconsistencies | Proofread + add RAG figures | Proofread + add attacker diagrams | Proofread + add defender diagrams | Build slides (15 max), insert figures |
| D27 | Rehearse full demo : 5-min live duel with commentary | Setup demo environment | Drive demo terminal | Monitor live scores during demo | Present slides rehearsal |
| D28 | Final bug fixes | — | Tag v1.0 on GitHub | — | Submit report + slides |

---

---

---

# 🟡 VULNERABILITY HUNTER

---

## Roles Redefined for This Project

**P1 — Orchestration & Architecture Lead** → LangGraph pipeline, system integration
**P2 — RAG & CVE Engineer** → NVD indexing, ChromaDB, CVE correlation
**P3 — Static Analysis Engineer** → Semgrep, AST parsing, LLM confirmation
**P4 — Tools & APIs Engineer** → GitHub API, OSV.dev, Advisory DB, tool calling
**P5 — Evaluation & Metrics Engineer** → ground truth, benchmarks, ablation

---

## Week 1 — Ingestion & Parsing (14h)

**Goal : clone → parse → structured JSON pipeline working on Python + JS**

| Day | P1 Orchestration | P2 RAG & CVE | P3 Static Analysis | P4 Tools & APIs | P5 Evaluation |
| --- | --- | --- | --- | --- | --- |
| D1 | Setup GitHub repo, define module interfaces, data flow diagram | Download NVD JSON feed (bulk 2002–2024, nvd.nist.gov) | Install tree-sitter, test AST parsing on a 10-line Python file | Setup GitHub API auth, test rate limits | Define evaluation protocol : what counts as a true positive ? |
| D2 | Define unified JSON schema : {file, function, code, language, metadata} | Explore NVD structure : identify useful fields (CVE-ID, CWE, CVSS, description) | Python AST parser v1 : extract function names, imports, system calls | Build clone_repo.py : clone any public GitHub URL cleanly | Build ground truth dataset v1 : 20 known vulnerable functions from DVWA |
| D3 | Review P3 parser output, validate against schema | NVD cleaning script : filter useful fields, output clean JSON | Python AST parser v2 : extract call graphs, variable assignments | File type detection : identify Python vs JS vs Java files in repo | Ground truth v2 : add 20 clean functions as negative examples |
| D4 | Integration test : clone_repo → parser → JSON output | First NVD indexing into ChromaDB : embed CVE descriptions | JavaScript parser with tree-sitter : same extraction as Python | Java basic parser with tree-sitter | Define metrics : precision, recall, F1, false positive rate |
| D5 | End-to-end test on Flask hello-world repo | Test RAG retrieval : "SQL injection" → top 5 CVEs returned | Test parsers on 3 repos : Python-only, JS-only, mixed | Test clone_repo on 5 different repos including large ones | Manual check : does JSON output capture all constructs needed for analysis ? |

**W1 Deliverable :** Clone → parse → JSON pipeline on Python + JS + Java. NVD RAG queryable. Ground truth 40 examples.

---

## Week 2 — Static Analysis (14h)

**Goal : Semgrep + LLM confirmation pipeline, natural language explanations**

| Day | P1 Orchestration | P2 RAG & CVE | P3 Static Analysis | P4 Tools & APIs | P5 Evaluation |
| --- | --- | --- | --- | --- | --- |
| D6 | Design static analysis module : Semgrep → LLM confirmation → structured finding | Map CWE IDs to Semgrep rule categories | Integrate Semgrep programmatically : subprocess call, parse JSON output | Explore GitHub Advisory Database API : query by ecosystem, severity | Semgrep baseline : run alone on DVWA, measure precision + recall |
| D7 | Review Semgrep output schema, align with unified finding format | RAG v2 : index by CWE-ID as metadata filter | Custom Semgrep rules v1 : SQLi, XSS, path traversal | GitHub Advisory API wrapper : query by package name, return CVEs | Compare default Semgrep rules vs custom rules on DVWA ground truth |
| D8 | Design LLM confirmation layer : when does it override Semgrep ? | RAG v3 : add CVSS score as retrieval filter for severity ranking | Custom Semgrep rules v2 : hardcoded secrets, insecure deserialization, SSRF | OSV.dev API wrapper : query by ecosystem + package version | False positive test : run Semgrep on 20 clean functions |
| D9 | Integrate LLM confirmation into pipeline : Semgrep flags → LLM validates | Test CWE-based RAG : precision@3 improvement over free text query ? | LLM analyst v1 : given flagged snippet → confirm vuln + severity | NVD API direct query tool : real-time CVE lookup by CVE-ID | Semgrep + LLM confirmation vs Semgrep alone : precision delta |
| D10 | Full static analysis pipeline integration test on DVWA | Tune embedding model : ada-002 vs text-embedding-3-small | LLM analyst v2 : generate natural language explanation per finding | Tool selector v1 : agent decides which API to call based on finding type | Full metrics after LLM confirmation : precision, recall, F1 on ground truth |

**W2 Deliverable :** Semgrep + custom rules + LLM confirmation. Natural language explanations. 3 API wrappers. Precision/recall baseline.

---

## Week 3 — CVE Correlation via RAG (14h)

**Goal : link every detected vulnerability to real CVEs with severity + patch**

| Day | P1 Orchestration | P2 RAG & CVE | P3 Static Analysis | P4 Tools & APIs | P5 Evaluation |
| --- | --- | --- | --- | --- | --- |
| D11 | Design correlation module : finding → multi-source CVE query → ranked output | Correlation RAG v1 : query = CWE + code snippet → top 3 CVEs | Integrate correlation output into finding schema | Tool calling integration : P3 finding triggers P4 API calls | Build correlation ground truth : for 15 DVWA vulns, identify correct CVEs manually |
| D12 | Define aggregation logic : merge results from RAG + GitHub Advisory + OSV.dev | Correlation RAG v2 : combine semantic similarity + CWE filter + CVSS min threshold | LLM re-ranker : given 5 CVE candidates, pick the most relevant 3 | Multi-tool orchestration : call RAG + GitHub Advisory + OSV.dev in parallel | Precision@3 on correlation ground truth |
| D13 | Design severity aggregation : CVSS pulled automatically, mapped to LOW/MEDIUM/HIGH/CRITICAL | Add patch references to RAG : official NVD fix links per CVE | LLM patch summarizer : given CVE + patch link → 2-sentence fix recommendation | Tool fallback logic : if GitHub Advisory returns empty → fallback to OSV.dev | Precision@5, compare with precision@3 |
| D14 | Full correlator integration test | Tune retrieval : top-k experiments, threshold experiments | Integrate patch recommendation into finding JSON | Tool calling robustness : handle API failures, rate limits, empty results | Correlation quality on WebGoat (Java) : generalization test |
| D15 | End-to-end test : repo → static → correlation → enriched JSON output | Final RAG tuning, document parameters | Static + correlation pipeline combined test | Tool calling latency benchmark : which API is slowest ? | Correlation evaluation report : precision@3 + precision@5 finalized |

**W3 Deliverable :** Full CVE correlation pipeline. 3 API tools. Patch recommendations. Precision@k metrics on ground truth.

---

## Week 4 — Orchestrator Agent + Report Generation (16h)

**Goal : end-to-end agent, Streamlit interface, PDF report**

| Day | P1 Orchestration | P2 RAG & CVE | P3 Static Analysis | P4 Tools & APIs | P5 Evaluation |
| --- | --- | --- | --- | --- | --- |
| D16 | Build full LangGraph orchestrator : state machine from GitHub URL to final report | RAG-powered executive summary : aggregate all findings → high-level risk overview | Final static analysis integration into orchestrator | Full tool calling integration into orchestrator, handle all edge cases | End-to-end test on DVWA : does full pipeline produce all known vulns ? |
| D17 | Multi-step reasoning : orchestrator re-analyzes suspicious functions flagged by LLM | RAG context injection : pull top CVE context before LLM writes each finding | Orchestrator handles multi-file repos : iterates all parsed files | Streamlit backend : connect URL input to orchestrator trigger | Quality check : are recommendations actionable and accurate on 5 test repos ? |
| D18 | Agent memory : orchestrator remembers patterns seen across files in same repo | RAG-powered finding deduplication : merge near-identical findings | Static analysis orchestrator handles errors gracefully (unparseable files) | Report generator : Markdown + PDF export | Hallucination check : does LLM invent CVE IDs that don't exist ? |
| D19 | Error handling : repos > 10k files, binary files, API rate limits | Handle RAG failures with keyword fallback | Finalize static analysis module, edge cases covered | Streamlit frontend : severity color coding, CVE hyperlinks, download button | Stress test : 5 diverse repos, log all pipeline failures |
| D20 | 50-file stress test on large repo | Final RAG documentation | Code cleanup, docstrings | Code cleanup, docstrings | Full pipeline evaluation on CVEfixes dataset : first pass |

**W4 Deliverable :** Full agent, Streamlit interface, PDF report, tested on 5 repos, CVEfixes evaluation started.

---

## Week 5 — Rigorous Evaluation (12h)

**Goal : ablation, 3 baselines, publishable metrics**

| Day | P1 Orchestration | P2 RAG & CVE | P3 Static Analysis | P4 Tools & APIs | P5 Evaluation |
| --- | --- | --- | --- | --- | --- |
| D21 | Design ablation : 4 variants of the system | Ablation variant : no RAG, LLM-only CVE correlation | Baseline 1 : Semgrep alone, no LLM, no RAG | Baseline : no tool calling, RAG-only correlation | Run all variants on CVEfixes ground truth |
| D22 | Ablation variant 2 : Semgrep + RAG but no LLM confirmation | Ablation variant : static RAG vs dynamic RAG | Baseline 2 : LLM-only analysis, no Semgrep | Tool calling ablation : 1 API vs 3 APIs | Build full comparison table : 4 ablations × precision/recall/F1 |
| D23 | False positive analysis : what types of clean code get wrongly flagged ? | Final RAG quality report | Final static analysis review | Tool calling latency + accuracy report | Generate all figures : precision/recall curves, ablation bar charts |
| D24 | Full reproducibility test : fresh clone → pipeline running in under 15 min | requirements.txt, environment.yml | Final code review | Final code review | Final numbers verified and cross-checked |

**W5 Deliverable :** 3 baselines, ablation table, false positive analysis, clean reproducible codebase.

---

## Week 6 — Integration, Demo, Report (10h)

| Day | P1 | P2 | P3 | P4 | P5 |
| --- | --- | --- | --- | --- | --- |
| D25 | Write Introduction + Architecture section | Write RAG & Correlation section | Write Static Analysis section | Write Tool Calling section | Write Evaluation section, assemble report |
| D26 | Full report review | Proofread + add RAG figures | Proofread + add static analysis figures | Proofread + add tool calling diagrams | Build slides (15 max), insert all figures |
| D27 | Rehearse demo : paste DVWA URL → full report in 3 min live | Setup demo environment | Drive demo terminal | Monitor API calls during demo | Present slides rehearsal |
| D28 | Final bug fixes | — | Tag v1.0 on GitHub | — | Submit report + slides |

---

## Technical Stacks

|  | Red-Team Agent | Vulnerability Hunter |
| --- | --- | --- |
| Orchestration | LangGraph | LangGraph |
| LLM framework | LangChain | LangChain |
| Vector DB | ChromaDB | ChromaDB |
| LLM | GPT-4o-mini | GPT-4o-mini |
| Key data | HarmBench, AdvBench | NVD JSON feed, CVEfixes |
| Key tools | Perspective API | Semgrep, tree-sitter, OSV.dev API, GitHub Advisory API |
| Frontend | Streamlit | Streamlit |
| Test targets | GPT-4o-mini, Claude | DVWA, WebGoat |

**READ TEAM AGENT DATASET**

| Dataset | Purpose | Access | Size |
| --- | --- | --- | --- |
| **HarmBench** | Main evaluation benchmark : categorized hostile prompts | Open source GitHub, direct download | ~400 prompts |
| **AdvBench** | Adversarial attack corpus to feed the RAG | Open source GitHub | ~500 examples |
| **JailbreakBench** | Additional jailbreak prompts to diversify attacks | Open source GitHub | ~100 prompts |

**VULNERABILITY HUNTER**

| Dataset | Purpose | Access | Size |
| --- | --- | --- | --- |
| **NVD JSON Feed** | CVE RAG knowledge base — main correlation source | nvd.nist.gov, free bulk download | ~250k CVEs (~4GB) |
| **CVEfixes** | Evaluation ground truth : GitHub repos with known patched CVEs | Open source GitHub (Zenodo) | ~10k commits |
| **DVWA** | Vulnerable test repo for development and debugging | Open source GitHub, direct clone | Small repo |
| **WebGoat** | Vulnerable Java test repo for generalization testing | Open source GitHub (OWASP) | Medium repo |
| **Juliet Test Suite** | Synthetic vulnerable functions categorized by CWE | NIST, free download | ~28k examples |
