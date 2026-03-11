# 🎵 Unveiling Listening Patterns
### Pattern Mining on Real-World Music Streaming Data
**DS-3002 Data Mining — Assignment #1 | Spring 2026 | BSDS | FAST-NUCES**

---

## 📋 Overview

This project applies **Multiple Minimum Support Apriori (MSApriori)** to real-world music listening data from Last.fm to discover co-listening patterns across 1,892 users and 17,632 artists. The mined association rules feed into a playlist recommendation engine and a listener persona feature for a music startup.

---

## 🎯 Objectives

- Mine association rules from real Last.fm listening history data
- Handle the **rare item problem** using Multiple Minimum Support (MMIS)
- Design a MIS scheme using the **proportional strategy**
- Implement **MSApriori from scratch** (not standard Apriori)
- Generate listener personas and a **Discovery Mix** recommendation engine

---

## 📁 Repository Structure

```
DS3002_Assignment1/
│
├── assignment1.ipynb          # Main Jupyter notebook (all code)
├── DS3002_Assignment1.pdf     # Final report with results
├── DS3002_Assignment1.tex     # LaTeX source for the report
├── README.md                  # This file
│
├── screenshots/               # Output screenshots used in report
│   ├── playcount_histogram.png
│   ├── outputofpre.png
│   ├── secondoutput.png
│   ├── PartA1.png
│   ├── FinalPartA.png
│   ├── phi_analysis.png
│   ├── PartB1conceptcheck.png
│   ├── F1table.png
│   ├── C2andF2.png
│   ├── F3_summarytable.png
│   ├── PartC.png
│   ├── PartC2.png
│   └── PartD.png
│
└── data/                      # Dataset files (download separately)
    ├── user_artists.dat
    ├── artists.dat
    ├── user_taggedartists.dat
    └── tags.dat
```

---

## 📦 Dataset

The dataset is **not included** in this repository due to size.

Download it manually from:
> 🔗 [GroupLens — hetrec2011-lastfm-2k.zip](https://grouplens.org/datasets/hetrec-2011/)

After downloading, extract and place the `.dat` files in the `data/` folder (or same directory as the notebook).

---

## ⚙️ Setup & Installation

### 1. Clone the repository
```bash
git clone https://github.com/YOUR_USERNAME/DS3002_Assignment1.git
cd DS3002_Assignment1
```

### 2. Create a virtual environment
```bash
python3 -m venv venv
source venv/bin/activate        # Linux/Mac
venv\Scripts\activate           # Windows
```

### 3. Install dependencies
```bash
pip install pandas numpy matplotlib
```

### 4. Launch the notebook
```bash
jupyter lab assignment1.ipynb
```

---

## 🔬 Methodology

### Preprocessing
| Parameter | Value |
|-----------|-------|
| Playcount threshold | **75 plays** |
| Total transactions | 1,817 users |
| Unique artists | 14,249 |
| Avg transaction length | 41.76 artists/user |

### MIS Design (Part A)
- **Strategy:** Proportional — `MIS(item) = α × support(item)`
- **Alpha:** 0.5
- **MIS floor:** 0.0003
- **Phi (φ):** 0.05 (40.2% pruning rate)

### MSApriori Results (Part B)
| Level | Candidates | Frequent | Pruned |
|-------|-----------|----------|--------|
| F1 | 200 | 200 | 0 |
| F2 | 13,483 | 176 | 13,307 |
| F3 | 199 | 26 | 173 |

### Top Association Rules (Part C)
| Rule | Confidence | Lift |
|------|-----------|------|
| Judas Priest + Iron Maiden → Black Sabbath | 72.2% | 12.74 |
| Judas Priest → Black Sabbath | 71.2% | 12.56 |
| Slayer → Megadeth | 61.9% | 12.49 |
| Selena Gomez → Demi Lovato | 63.0% | 10.31 |

---

## 👥 Listener Personas

### 🤘 The Metal Pilgrim
> Classic and thrash metal collector. Judas Priest + Iron Maiden → Black Sabbath (lift = 12.74)

**Recommendation:** *Metal Foundations* playlist

---

### 👑 The Pop Queen Devotee
> Follows 2000s–2010s female pop artists as a cohesive group. Madonna + Rihanna → Beyoncé (lift = 4.34)

**Recommendation:** *Queens of Pop* Discovery Mix

---

### ⭐ The Disney Generation Fan
> Disney Channel nostalgia listener. Selena Gomez → Demi Lovato (lift = 10.31)

**Recommendation:** *Disney Era Throwback* playlist

---

## 🏆 Discovery Mix Engine (Part D)

### Scoring Function
```
Score = 0.2 × support + 0.3 × confidence + 0.5 × normalized_lift
```

| Weight | Metric | Reason |
|--------|--------|--------|
| 0.2 | Support | Low — avoid penalizing niche rules |
| 0.3 | Confidence | Medium — ensures reliable firing |
| 0.5 | Lift (normalized) | High — rewards surprising discoveries |

### Cold Start Strategy
When a new user listens to exactly 2 artists:
1. Find all rules where antecedent ⊆ user's artists
2. Collect consequent artists
3. Remove already-known artists
4. Rank by score
5. Return top N recommendations

#### Example
```
New user: Judas Priest + Iron Maiden
→ Recommend: Black Sabbath (conf=72.2%, lift=12.74)
→ Recommend: AC/DC         (conf=66.7%, lift=8.02)
```

---

## 📊 Key Findings

- **98.7%** of candidate 2-itemsets were pruned at F2 level — demonstrating MMIS power
- The **metal cluster** (Judas Priest, Iron Maiden, Black Sabbath, Megadeth, Slayer) produces the strongest rules in the entire dataset with lifts above 12
- **Radiohead + Coldplay** are individually frequent but their pair is NOT frequent (proves downward closure fails under MMIS)
- **Disney artists** show surprisingly high lift values (10.31) — cultural nostalgia drives co-listening more than musical similarity

---

## 📚 References

- Liu, B., Hsu, W., Ma, Y. (1999). *Mining association rules with multiple minimum supports.* KDD-99.
- GroupLens Research. *HetRec 2011 Last.fm Dataset.*
- Balke, W., Homoceanu, S. *Association Rule Mining Lecture Slides.* TU Braunschweig.

---

## 📄 License

This project is for academic purposes only — DS-3002 Data Mining, FAST-NUCES, Spring 2026.

---

*Made with 🎵 and Python*
