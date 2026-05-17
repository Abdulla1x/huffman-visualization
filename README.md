# Huffman Coding — Interactive Visualization

A browser-based interactive tool for learning and exploring Huffman Coding, built as part of **BCS 309 – Algorithms I** (Spring 2025–26) at the School of Engineering, Applied Sciences and Technology.

**[Live Demo](https://abdulla1x.github.io/huffman-visualization/)**

---

## What It Does

Huffman Coding is a greedy, lossless compression algorithm that assigns variable-length binary codes to characters based on their frequency — frequent characters get shorter codes, rare ones get longer codes. The resulting codes are prefix-free, meaning they can be decoded unambiguously without separators.

This tool visualizes the entire algorithm step by step: from building the min-heap, through each greedy merge, to the final Huffman tree and the generated binary codes.

---

## Features

| Tab | What you see |
|---|---|
| 🌳 Huffman Tree | Animated tree construction, node-by-node |
| 📊 Min-Heap | Bar chart of the priority queue state at each step |
| 🌳+📊 Combined | Both visualizations together |
| 🔢 Encode / Decode | Encode any text using the generated codes; decode binary back to text |
| 📈 Complexity | Live O(n log n) vs O(n²) vs O(n) growth chart comparing Huffman, Shannon-Fano, and Arithmetic Coding |
| ⚡ Greedy | Tracks every greedy choice made, with exchange-argument proof of optimality |
| 📚 Learn | Collapsible explanations covering the algorithm, a worked example, common mistakes, and CLO coverage |

### Controls

- **Build Steps** — computes the full step sequence for the current input
- **Step ▶** — advance one step at a time
- **▶ Play / ⏸ Pause** — auto-play through all steps
- **Speed slider** — 0.25× to 4× playback speed
- **↺ Reset** — clear everything
- **Jump to step** — click any row in the History table to jump directly to that step

### Presets

`ABRACADABRA` · `MISSISSIPPI` · `Worst Skewed (aaaaabbbccd)` · `Hello World` · or type any custom text.

---

## Algorithm Overview

```
HUFFMAN(text):
  freq = COUNT_FREQ(text)          // O(n)
  Q = BUILD_MIN_HEAP(freq)         // O(k)
  while |Q| > 1:
    x = EXTRACT_MIN(Q)             // O(log k)
    y = EXTRACT_MIN(Q)             // O(log k)
    z = NEW_NODE(x.freq + y.freq)
    z.left = x;  z.right = y
    INSERT(Q, z)                   // O(log k)
  root = EXTRACT_MIN(Q)
  ASSIGN_CODES(root, "")           // O(k), left=0 right=1
  return codes
```

**Time complexity:** O(k log k) where k = number of distinct characters  
**Space complexity:** O(k)

The min-heap is essential. A sorted-array alternative degrades to O(k²) due to O(k) reinsertion cost after each merge — prohibitive for large alphabets (e.g., Unicode with k = 65,536).

### Worked Example — "ABRACADABRA"

| Character | Frequency | Huffman Code | Bits used | Fixed (8-bit) |
|-----------|-----------|--------------|-----------|---------------|
| A | 5 | `0` | 5 | 40 |
| R | 2 | `10` | 4 | 16 |
| B | 2 | `111` | 6 | 16 |
| C | 1 | `1100` | 4 | 8 |
| D | 1 | `1101` | 4 | 8 |

**Total: 23 bits vs. 88 bits (fixed-width ASCII) — ~74% reduction**

Merge sequence: `C:1 + D:1 → CD:2` → `CD:2 + B:2 → CDB:4` → `R:2 + CDB:4 → RCDB:6` → `A:5 + RCDB:6 → root:11`

---

## Complexity Comparison

| Algorithm | Best | Average | Worst | Space |
|-----------|------|---------|-------|-------|
| Huffman Coding | O(k log k) | O(k log k) | O(k log k) | O(k) |
| Shannon-Fano | O(k log k) | O(k log k) | O(k²) | O(k) |
| Arithmetic Coding | O(n) | O(n) | O(n) | O(n) |

> **Note:** Huffman's compression ratio is worst when all characters have equal frequency — the tree is perfectly balanced and codes are uniform-length, giving zero benefit over fixed encoding. The complexity stays O(k log k) regardless.

---

## Design Paradigm

Huffman Coding is a **greedy algorithm**. At each step it makes the locally optimal choice — merging the two nodes with the smallest frequencies. This greedy choice property, provable via an exchange argument, guarantees a globally optimal prefix-free code:

> *If the two lowest-frequency symbols are x and y, any optimal tree must have them as siblings at the deepest level — otherwise swapping them with deeper siblings would reduce total cost.*

---

## Academic Context

This project was submitted for **BCS 309 – Algorithms I** and covers the following course learning outcomes:

- **CLO-1 (Asymptotic Analysis):** O(k log k) analysis with heap vs. sorted-array comparison
- **CLO-2 (Sorting & Searching):** Min-heap as a priority queue; extract-min at each merge step
- **CLO-3 (Greedy Paradigm):** Greedy choice property and exchange-argument proof of optimality

**Instructor:** Dr. Arash Kermani  
**Student:** Mohammad Abdullah (ID: 20230003944)

---

## Usage

No installation required. Open `Mohammad_Abdulla-Huffman_Coding.html` in any modern browser, or visit the live demo:

```
https://abdulla1x.github.io/huffman-visualization/
```

Everything runs client-side — pure HTML, CSS, and vanilla JavaScript. No dependencies.

---

## License

For educational use. Submitted as coursework for BCS 309, Spring 2025–26.
