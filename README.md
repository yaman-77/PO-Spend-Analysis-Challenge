# 🛠 Procurement Item Classification — A Prototype Journey

## 📌 Project Overview
This repository documents a **prototype project** digging deep into the **spendings of line items in Purchase Orders**.  
To achieve this, we tested classifying messy procurement items into meaningful categories using both **manual taxonomy** and **embedding-based approaches**.  

The dataset was messy, containing **bilingual item names (Arabic & English)** with inconsistent formats, codes, specs, and vendor references.

The journey focused on answering one key question:  

> **Can we bring structure and insights to unstructured procurement text data with limited resources?**

---

## 🚀 Problem Statement
Procurement spend analysis often relies on structured item categories. In reality, item descriptions are messy:

- Written in both Arabic and English  
- Mixed with units, codes, and specs (`"12 مم"`, `"g60"`, `"1220 2440"`)  
- Vendors and brands inserted into the text (`سابك`, `Al-Suwaidi`)  

Without clean categories, organizations can’t answer basic strategic questions such as:
- Which categories drive the majority of spend?  
- Where can we negotiate better with suppliers?  
- Which classes dominate in quantity?  

---

## 🧭 Our Approach (Step by Step)

### 🔹 Exploration
- Started with **word frequency analysis (unigrams)**.  
- Found that most frequent tokens were **units and codes** (e.g., `mm`, `مم`, `x`, `f`).  
- **Insight:** raw word counts weren’t enough.  

### 🔹 Bi-gram Analysis
- Extracted **two-word combinations** (e.g., `حديد تسليح`, `steel bar`, `ماسورة حديد`).  
- Surfaced **product-level signals**: rebar, sheets, pipes, cables.  
- Still noisy, but better than unigrams.  

### 🔹 Manual Classification
- Created initial keyword-based classes:
  - Rebar & Steel Bars  
  - Steel Sheets / Plates  
  - Pipes & Tubes  
  - Other / Specs / Unclassified  
- Mined **“Other”** → discovered new families: **Electrical, Concrete Panels, Fasteners, Coatings**.  
- Expanded taxonomy to **7 categories**.  

### 🔹 Embedding-based Classification
- Used **`sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`**.  
- Created **prototypes** for each class using bilingual prompts.  
- Encoded all items → matched to closest class by **cosine similarity**.  
- Supported **multi-label classification** when an item spanned categories.  

### 🔹 Visualization & Dashboard
Built a basic dashboard (matplotlib/seaborn) to show:
- Total spend and quantities  
- Top POs by spend/qty  
- Top classes by spend/qty  
- % share of each category  

Enabled quick insights like:  
> *“2 classes account for 50% of spend — these are strategic categories.”*  

---

## ⚠️ Limitations
It’s evident this is not the optimal way of approaching the problem, but a strong prototype:  

- Heavy reliance on **item names only**; ignored supplier codes, units, or material groups.  
- Embedding thresholds and keyword lists were **heuristic**, not tuned with labeled data.  
- Category taxonomy was **manually designed**, so may miss edge cases.  
- Noise from specs and codes still affects classification accuracy.  

---

## 🔮 Future Recommendations
- Incorporate **structured fields** (supplier, material codes, units) to complement text.  
- Build a **labeled dataset** → train supervised models, validate properly.  
- Expand taxonomy with **domain experts** to ensure coverage.  
- Deploy in an **interactive dashboard** where users can correct classifications → create feedback loops.  
- Explore **semi-supervised clustering** with embeddings for continuous discovery of new categories.  

---

## 📊 Example Outputs
- **Spend concentration**: showed that 2 categories alone made up ~50% of spend.  
  - **Insight:** treat them as *strategic categories* in sourcing and risk planning.  
- **Quantity analysis**: highlighted classes that drive procurement in volume but not spend.  
- Provided an early look at how **text + embeddings** can unlock value in messy ERP data.  

---

## 🛠 Tech Stack
- **Python 3.9**  
- **Libraries**: pandas, numpy, matplotlib, seaborn, scikit-learn, sentence-transformers, openpyxl  
- **Environment**: managed with conda (`environment.yml` included)  

---

## 📂 Repository Structure
```plaintext
├── notebooks/         # Jupyter notebooks with analysis & experiments
├── src/               # Helper scripts for classification & embeddings
├── environment.yml    # Conda environment for reproducibility
├── README.md          # This file
```


🙌 Closing Note

This prototype demonstrates that even with messy, bilingual procurement data, it’s possible to create a structured spend classification pipeline. While not production-ready, it showcases the potential if more time and resources were invested.
