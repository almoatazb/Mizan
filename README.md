# MIZAN — Arabic Morphological Pattern Analyzer
### Version 2.0  ·  Almoataz B. Al-Said  ·  Moataz Tools  ·  2026

---

## ⚠️  Please Cite This Tool

> **When using MIZAN in academic work, please cite:**
>
> Al-Said, A. B. (2026). Mizan: An Innovative Tool for Building Arabic Morphological Pattern Dictionaries and Enhancing Lexical Resource Development. *ACM Transactions on Asian and Low-Resource Language Information Processing*, 25(2), Article 11.
> https://doi.org/10.1145/3786605

```bibtex
@article{alsaid2026mizan,
  author    = {Al-Said, Almoataz B.},
  title     = {Mizan: An Innovative Tool for Building Arabic
               Morphological Pattern Dictionaries and Enhancing
               Lexical Resource Development},
  journal   = {ACM Trans. Asian Low-Resour. Lang. Inf. Process.},
  volume    = {25}, number = {2}, articleno = {11},
  year      = {2026},
  doi       = {10.1145/3786605}
}
```

---

## What is MIZAN?

MIZAN extracts all words conforming to a user-defined morphological template (وزن) from large Arabic text corpora, producing structured frequency lists for:

- Building Arabic morphological pattern dictionaries (معاجم الأبنية الصرفية)
- Corpus-based lexicographic research
- NLP pipeline development
- Computational linguistics studies

Unlike full-parsing tools (BAMA, SAMA, MADAMIRA, Farasa, CamelMorph), MIZAN focuses on **pattern-based extraction at scale** — processing entire corpora to identify all words sharing a morphological template.

---

## Published Performance

Evaluated on Arabic Wikipedia (572 million words):

| Pattern | F1 Score | Notes |
|---------|----------|-------|
| فعيل | 80.3% | Short, higher ambiguity |
| فعول | 83.8% | Short, higher ambiguity |
| مفعول | 95.5% | Well-defined structure |
| فاعول | 94.2% | Clear prefix marker |
| متفاعل | **100%** | Distinct structure |
| استفعال | **100%** | Long, predictable |

Processing: ~14–16 minutes per pattern on 572M words.

---

## Requirements

```
Python  >= 3.10
PyQt6             pip install PyQt6
psutil            pip install psutil    # optional — adaptive memory management
```

---

## Installation & Launch

```bash
pip install PyQt6 psutil
python mizan_v20.py
```

> ⚠️ **Windows:** Launch from `cmd.exe` only — not from IDLE or Spyder.

---

## Interface

The interface is divided into two panels.

**Left panel — Configuration**

| Group | Controls |
|-------|----------|
| Corpus & Output | Folder paths for corpus, output, and optional stopwords file |
| Format & Encoding | File format (TXT / XML) and text encoding |
| Morphological Patterns | Pattern input field, Add button, pattern list, Quick Examples grid |
| Analysis Options | Strip diacritics toggle, KWIC generation toggle |
| Progress | Progress bar and file counter |

Patterns can be entered in two ways:
- **Type** an Arabic pattern in the input field and press **＋ Add** or **Enter**
- **Click** any pattern in the Quick Examples grid to add it instantly

**Right panel — Output**

| Tab | Contents |
|-----|----------|
| Log | Real-time processing log with colour-coded messages |
| Results | Interactive frequency table — click any word to open its concordance |
| Statistics | Per-pattern and cross-pattern statistics summary |

---

## Concordance Viewer

Clicking any word in the Results table opens a **Concordance Dialog** showing every occurrence of that word in the corpus. Each entry displays:

- Left context · **pivot word highlighted** · right context
- Source filename beneath each line

The search runs in a background thread and the dialog is non-modal, so multiple words can be viewed side by side.

---

## Output Structure

Each session creates a versioned, timestamped folder:

```
MIZAN_v2.0_2026-02-24_17-24/
│
├── Patterns/                    ← Word-frequency files (one per pattern)
│   ├── تفاعل.txt                   Rank | Word | Count | Pct% | Cumulative%
│   └── استفعال.txt                  + attribution watermark header
│
├── KWIC/                        ← Context files (optional, one per pattern)
│   └── تفاعل_KWIC.txt               Top 10 words × 10 sentences each
│
├── statistics_report.txt        ← Full analysis report
├── session_summary.txt          ← Brief metadata + attribution
└── session_log.txt              ← Complete timestamped processing log
```

### Pattern files — 5 columns

```
# MIZAN v2.0  —  Arabic Morphological Pattern Analyzer
# Author  : Almoataz B. Al-Said  |  Moataz Tools  ·  2026
# DOI     : https://doi.org/10.1145/3786605
# Pattern : اِسْتِفْعَال
#──────────────────────────────────────────────────────────────
Rank  Word        Count   Pct%     Cumulative%
1     استقصاء     74      25.874   25.874
2     استعداد     30      10.490   36.364
```

### statistics_report.txt

- **Corpus Profile**: total tokens, unique types, Type-Token Ratio, per-file breakdown
- **Per-pattern analysis**: unique forms, tokens, hapax %, dis legomena, frequency bands, estimated precision, ASCII top-10 chart
- **Cross-pattern comparison**: ranked by productivity, precision, coverage, lexical richness
- **Overlap analysis**: pairwise shared-word counts, % from each pattern, examples, assessment (Excellent / Good / Moderate / High)

### KWIC files

```
══════════════════════════════════════════════════════════════════
  MIZAN v2.0  —  KWIC Contexts
  Pattern : اِسْتِفْعَال
  Top 10 words  ×  up to 10 contexts each
══════════════════════════════════════════════════════════════════

  [ 1]  استقصاء   (freq=74)
──────────────────────────────────────────────────────────────────
   1.  ...بعد طول  [استقصاء]  وتمحيص رأى الآباء أن هذا الأمر...
        ↳ doc_01.txt
```

KWIC generation is optional — uncheck in Analysis Options to skip it and reduce processing time.

---

## Corpus Pre-Scan

Before pattern analysis begins, MIZAN performs a full corpus scan to count actual Arabic tokens and unique types:

```
[PHASE]  >>> START  Corpus pre-scan
[INFO ]    Total Arabic tokens : 447,778
[INFO ]    Total unique types  : 23,441
[PHASE]  <<< END   Corpus pre-scan  [0.84s]
```

---

## Recursive Corpus Search

All subfolders are searched automatically:

```
[INFO]  Files found : 87 .txt file(s)  (recursive, 4 subfolder(s))
[INFO]    ↳ 01 - الملكانيون/  (33 files)
[INFO]    ↳ 02 - القبط/       (28 files)
```

---

## Pattern Notation — ف ع ل

| Symbol | Role |
|--------|------|
| ف | First root letter |
| ع | Second root letter |
| ل | Third root letter |
| All other letters | Fixed structural letters |

---

## 20 Built-in Quick Patterns

| Pattern | Category | Example |
|---------|----------|---------|
| فَعَلَ | Form I Verb | كتب، قرأ |
| فَعَّلَ | Form II Verb | علّم، درّب |
| فَاعَلَ | Form III Verb | كاتب، جادل |
| أَفْعَلَ | Form IV Verb | أكمل، أرسل |
| تَفَعَّلَ | Form V Verb | تعلّم، تدرّب |
| تَفَاعَلَ | Form VI Verb | تكاتب، تجادل |
| اِنْفَعَلَ | Form VII Verb | انكسر، انقطع |
| اِفْتَعَلَ | Form VIII Verb | اكتسب، اجتمع |
| اِسْتَفْعَلَ | Form X Verb | استكمل، استعمل |
| فَاعِل | Active Participle | كاتب، قارئ |
| مَفْعُول | Passive Participle | مكتوب، مقروء |
| مَفْعَلَة | Noun of Place/Time | مكتبة، مدرسة |
| تَفْعِيل | Verbal Noun (Form II) | تعليم، تدريب |
| اِسْتِفْعَال | Verbal Noun (Form X) | استعمال، استقبال |
| فَعَّال | Intensive Adjective | كتّاب، علّام |
| مُسْتَفْعِل | Active Part. Form X | مستعمل، مستقبل |
| مَفَاعِيل | Broken Plural | مفاتيح، مساجد |
| فُعَلاء | Plural of فعيل | علماء، أمراء |
| فَعْلَان | Adjectival Pattern | عطشان، غضبان |
| مُتَفَاعِل | Active Part. Form VI | متكاتب، متجادل |

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| F5 | Run Analysis |
| Escape | Stop Analysis |
| Ctrl+O | Open Corpus Folder |
| Ctrl+S | Save Log |
| Ctrl+L | Clear Log |
| Ctrl+Q | Quit |

---

## Files

| File | Description |
|------|-------------|
| `mizan_v20.py` | Main application — GUI and analysis engine |
| `MIZAN_User_Guide_v20.docx` | Full user guide |
| `README.md` | This file |

---

## License & Attribution

© 2026 Almoataz B. Al-Said — Kuwait University / Cairo University

All output files produced by MIZAN carry an attribution watermark (author name, tool name, DOI). Users of this software in academic research are requested to cite the associated publication.

**Contact:** moataz.alsaid@ku.edu.kw
