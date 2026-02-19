# LXX–DSS Torah Code Pipeline

A reproducible pipeline for reconstructing a Hebrew Torah text from mixed sources and performing **letter-level Equidistant Letter Sequence (ELS)** analysis on the Tetragrammaton (יהוה).

This project combines:

* Hebrew verse structure from Sefaria
* Retroversion data derived from the Septuagint
* Structural inspiration from textual traditions related to the Dead Sea Scrolls
* Canonical ordering of the Torah

The pipeline converts verses into a **continuous indexed Hebrew letter stream** and runs strict and fuzzy sequence detection across configurable windows.

---

## Project Goals

* Build a structured Hebrew Torah dataset from reconstructed sources
* Normalize Hebrew letters (remove vowels and punctuation)
* Create a full **letter-index table**
* Detect:

  * Exact יהוה matches
  * ELS (Equidistant Letter Sequence) patterns
  * Fuzzy matches using similarity scoring
* Export reproducible analysis datasets

---

## Pipeline Overview

The notebook runs in sequential phases:

### Phase 1 — Verse Reconstruction

* Pull canonical verse structure from Sefaria API
* Merge with:

  * Pre-cleaned Hebrew text (`input_sefaria_verses.csv`)
  * LXX retroversion dataset (`lxx_hebrew_retroversions.csv`)
* Output:

```
torah_reconstructed.csv
```

---

### Phase 2 — Letter Index Generation

Each Hebrew letter is indexed sequentially across the entire Torah.

Normalization includes:

* Removal of punctuation
* Removal of vowel marks (niqqud)
* Preservation of Hebrew consonants only

Output:

```
torah_letter_index.csv
```

Columns:

```
Index | Letter | Hebrew Word | Book | Chapter | Verse
```

---

### Phase 3 — Sequence Detection

#### Exact detection

Counts occurrences of:

```
יהוה
```

#### ELS detection

Searches using fixed skip intervals (default: 50 letters).

Output:

```
yhvh_els_50.csv
```

---

### Phase 4 — Fuzzy Window Search

Sliding window approach using similarity scoring.

Parameters:

* Window size: 50 letters
* Similarity threshold: 0.80 (configurable)

Output:

```
yhwh_comparison.csv
```

---

### Phase 5 — Full Statistical Analysis

Generates:

* Exact occurrences
* ELS matches
* Fuzzy matches
* Spacing statistics
* Cluster detection

Outputs:

```
exact_occurrences.csv  
els_matches.csv  
fuzzy_matches.csv  
spacing_stats.csv  
clusters.csv  
summary.txt
```

---

### Phase 6 — Session Backup

Notebook includes optional export to Google Drive using Google Google Colab.

---

## File Structure

```
project/
│
├─ Public_LXX_DSS_Torahcode.ipynb
├─ input_sefaria_verses.csv
├─ lxx_hebrew_retroversions.csv
│
├─ torah_reconstructed.csv
├─ torah_letter_index.csv
│
├─ yhvh_els_50.csv
├─ yhwh_comparison.csv
│
└─ analysis_outputs/
```

---

## Requirements

Python 3.9+

Install dependencies:

```bash
pip install pandas requests tqdm
```

Standard libraries used:

* csv
* re
* difflib
* statistics
* math

---

## Running the Pipeline

Execute the notebook sequentially:

```
Public_LXX_DSS_Torahcode.ipynb
```

Required order:

1. Reconstruction
2. Letter indexing
3. Sequence detection
4. Statistical analysis
5. Export

Do **not** skip cells because later stages depend on generated CSV files.

---

## Configuration Parameters

Inside the notebook scripts:

```python
WINDOW_SIZE = 50
SKIP = 50
FUZZY_THRESHOLD = 0.80
TARGET = "יהוה"
```

These can be adjusted for experimental runs.

---

## Data Notes

* Hebrew vowels must already be removed from:

```
input_sefaria_verses.csv
```

* Letter normalization assumes:

```
א–ת only
```

* Any punctuation will be discarded automatically.

---

## Research Context

This repository is a **computational text experiment** exploring:

* Letter distribution patterns
* Sequence spacing
* Windowed similarity detection

It does **not** assume theological conclusions; it only provides reproducible numeric outputs for further analysis.

---

## Future Improvements

* Multi-skip ELS scanning (1–500 intervals)
* Visualization of sequence clusters
* Cross-text comparison (MT vs LXX vs DSS)
* GPU acceleration for large window scans

---

## Author Workflow Notes

* Designed for iterative experimentation
* CSV outputs allow downstream statistical tooling (Python/R)
* Compatible with Colab and local execution
