# JayChou-fan-language-analysis
---

### 👤 Project Lead: Ricky Chen (陳睿棋)

### 📍 Affiliation: National Chung Hsing University

### 🧠 Specialization: Natural Language Processing (NLP), Sociolinguistics, Sentiment & Tonality Analysis

---

## 🗺️ Project Overview: A Cross-Corpus Fan Language Analysis through NLP and Sociolinguistics

This project combines Natural Language Processing techniques with sociolinguistic perspectives to analyze fan comments directed toward Mandopop icon Jay Chou. The goal is to uncover emotional themes, stylistic tendencies, and cultural memory across generations, thereby shedding light on the evolution of linguistic styles and emotional feedback in online fan discourse.

---

## 🚦 Implementation Pipeline (Phase 1–3)

| Phase | Title | Key Output |
| --- | --- | --- |
| Phase 1 | Data Collection & Preprocessing | Creation of a clean, unified, cross-platform comment corpus |
| Phase 2 | Topic Modeling & Semantic Structure | BERTopic-based interpretable topic clusters with time-series analysis |
| Phase 3 | Multi-Aspect Sentiment Analysis (ABSA) | Zero-shot ABSA using LLMs to auto-construct (aspect, sentiment) structured dataset |

---

# 🔹 Phase 1: Data Collection & Preprocessing

### ✅ Task 1.1: Comment Sources & Scraping Script Design

**Data Sources:**

- YouTube (comments on Jay Chou's MVs, interviews, classic performances)
- Comment structure: video title, comment content, username, timestamp, likes

**Execution:**

- Built using YouTube Data API v3 with `google-auth` and `google-api-python-client`
- Script automates pagination and quota control

---

### ✅ Task 1.2: Initial Cleaning

**Processing Steps:**

- Removed spam, links, duplicates, and bot-like patterns
- Filtered out short (<5 characters) or meaningless entries (e.g., "nice", "first")
- Standardized column format: `raw_text`

---

### ✅ Task 1.3: Chinese NLP Preprocessing

**Techniques:**

- Tokenization using `jieba` with augmented lexicon
- Emoji normalization and slang mapping (e.g., "cry爆" → "very touched")
- Conversion to Traditional Chinese
- Final output column: `text_cleaned`

---

# 🔹 Phase 2: Topic Modeling & Semantic Structure

### ✅ Task 2.1: Topic Modeling with BERTopic

**Why BERTopic:**

- LDA performs poorly on Chinese due to tokenization instability and bag-of-words limitations
- BERTopic leverages Sentence-BERT embeddings + HDBSCAN, directly handling full Chinese sentences

**Results:**

- Identified dozens of semantically coherent clusters (e.g., nostalgic notes, emotional praise, technical critique)
- Human verification showed high interpretability and sociolinguistic clarity

**Sample Topics:**

| Topic ID | Type | Example |
| --- | --- | --- |
| `116` | Quick Praise | "So good ❤️", "Masterpiece" |
| `271` | Nostalgic Check-In | "Still listening in 2024", "Timeless memory" |
| `-1` | Residual Chatter | Noisy, low-confidence cluster |

---

### 📌 Observation: Topic Overload with BERTopic

- Over 100 topics increases detail but creates issues:
    - **Semantic overlap** (e.g., multiple topics about nostalgia/emotion)
    - **Visualization complexity**

---

### 🛠️ Proposed Solution: Meta-topic Aggregation Layer

> Cluster topics into higher-level semantic types to improve interpretability and visualization
> 

**Steps:**

| Step | Description |
| --- | --- |
| 1 | Re-cluster topic vectors (e.g., via KMeans) |
| 2 | Analyze keyword overlaps; assign human-readable labels |
| 3 | Add column `topic_type` (e.g., "Emotional Resonance") |

**Examples:**

| Meta-type | Topic IDs | Description |
| --- | --- | --- |
| Emotional Resonance | 18, 72, 124 | Involves "cry", "touching", "deep feeling" |
| Nostalgic Youth | 271, 89 | Life memory recall, like school days |
| Technical Critique | 32, 101 | Comments on MV quality, sound mixing, etc. |

---

### ✅ Task 2.2: Topic × Timeline Analysis

**Implementation:**

- X-axis: timestamp; Y-axis: topic frequency
- Heatmaps and trendlines reveal thematic popularity trends
- Example: Spike in nostalgic themes for "Rice Field" during COVID era, showing music as collective healing
- ![image](https://github.com/user-attachments/assets/7e1756ae-8ab6-49fd-b65d-f3eb4ebd6366)


---

# 🔹 Phase 3: Multi-Aspect Sentiment Analysis (ABSA)

### ✅ Task 3.1: Aspect & Sentiment Categories

| Aspect | Description |
| --- | --- |
| 🎵 Music Style | Arrangement, melody, vocal style |
| 📜 Lyrics | Storylines, metaphors, lyrical depth |
| 💭 Emotional Resonance | Personal reactions like crying, moved |
| 🧠 Comparative Reflection | Old vs. new Jay Chou style contrast |
| 🌱 Nostalgic Memory | School-era recall, farewell songs, etc. |

**Sentiment Types:**

- Positive / Neutral / Negative

---

### ✅ Task 3.2: Zero-shot ABSA via LLM

**Pipeline:**

1. Model: GPT-3.5-turbo via OpenAI API
2. Input: `text_cleaned`
3. Output Format: `{ "aspect": "...", "sentiment": "..." }`
4. Used Pandas for batch tagging and JSON validation

**Processed: ~26,984 comments**

**Execution Details:**

- ~15,600 entries processed before API quota/time limits
- Final sample for evaluation: **1,000 comments**
- Processing speed: ~8s per entry

---

### ✅ Task 3.3: Error Handling & Semantic Enhancements

- Auto-check for invalid JSON outputs
- Tag as "Undetermined" when output fails
- Created fan-language glossary (e.g., "cry爆" → Emotional Resonance + Positive)

---

### ✅ Task 3.4: ABSA × Topic × Time Integration

- Merged BERTopic clusters with ABSA sentiment
- Analyzed sentiment distribution across topics
- Structured DataFrame ready for visualization and deep semantic mining

---

## 📊 Results & Evaluation Metrics

| Metric | Target | Result |
| --- | --- | --- |
| Tagging Efficiency | >90% valid tags | ✅ ~94.6% |
| Sentiment Accuracy | >85% by human review | ✅ ~89% |
| Topic Interpretability | Clear semantic label per topic | ✅ High clarity |
| Aspect Diversity | Balanced aspect distribution | ✅ Emotional resonance most frequent |

---

## 📁 Structured Output Format (DataFrame)

| text_cleaned | topic | aspect | sentiment |
| --- | --- | --- | --- |
| "This song is super touching..." | 271 | Emotional Resonance | Positive |
| "Feels faded compared to earlier works" | 34 | Comparative Reflection | Negative |
| "Still listening – such a classic" | 271 | Nostalgic Memory | Positive |

---

## 🐟 Conclusion: Foundation Established, Future Work Suggested

- Cross-time-topic-aspect interactions
- Style modeling for fan language
- Mapping song lyric semantics to comment semantics
- Interactive visual dashboards
