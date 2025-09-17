# Bit-by-Bit-Hackathon

## Hackathon Name  
**Bit-by-Bit Hackathon**

## Team Members  
- **Julankanti Manvitha Reddy (23EC10032)**  
- **Sirigineedi Harshita Phani Surya Anshu (23EC10076)**

---

##  Repository Structure

```bash
problem1/
├── simulated_traces.csv
├── final_key.txt
├── byte_00.txt ... byte_15.txt
└── cpa_problem1.py

problem2/
├── simulated_power_trace.csv
├── final_key.txt
├── byte_00.txt ... byte_15.txt
└── cpa_problem2.py

README.md
```
---

## Problem 1 – Simulated Power Traces of Unmasked AES

### Objective  
Recover the full 16-byte AES key by applying Correlation Power Analysis (CPA) on ideal, noise-free simulated traces.

### Procedure Summary

1. **Data Input:**  
   Loaded the provided CSV file where each row includes the ciphertext and power trace samples. Used `pandas` to handle large datasets efficiently.

2. **Leakage Model:**  
   Applied the **Hamming Weight (HW) model**, assuming power correlates with the number of bits set in the AES S-box output after the AddRoundKey operation.

3. **CPA Implementation:**  
   - For each key byte (0–15):  
     - Tested all 256 possible key guesses.  
     - For each guess, computed the HW of the S-box output across all traces.  
     - Calculated Pearson’s correlation coefficient between the hypothetical HW vector and the actual power trace at each sample.  
     - Ranked key guesses based on maximum correlation.

4. **Deliverables:**  
   - Saved the full key guess in `final_key.txt`.  
   - Created `byte_00.txt` to `byte_15.txt`, listing key candidates ranked by correlation.

---

###  a) Pre-processing Methods

- None were required since traces are ideal and noise-free.  
- The traces were used as provided, without filtering or alignment.

---

###  b) Post-processing Methods

- Selected the highest correlation from each guess’s correlation profile to determine the most likely key byte.  
- Ranked all guesses and output them to files for further analysis.

---

###  c) Noise Consideration

- The simulated traces were ideal and contained no noise.  
- No specific noise mitigation methods were applied.

---

###  d) AES Round Targeted

- The attack targeted the **first round** of AES, specifically after the AddRoundKey operation where the plaintext is XORed with the key and fed into the S-box.

---

###  e) Challenges and Solutions

- No major challenges since the traces were noise-free.  
- The main focus was ensuring correct computation of correlations and accurate data parsing.

---

##  Problem 2 – Real Hardware Power Traces of Unmasked AES

###  Objective  
Recover the full AES key from real-world traces that include noise, jitter, and measurement artifacts, using correlation analysis with preprocessing techniques.

###  Procedure Summary

1. **Data Input:**  
   Loaded CSV traces where each row contains plaintext, ciphertext, and real measured samples.

2. **Preprocessing:**  
   - Centered the traces by subtracting the mean.  
   - Normalized them by dividing by the standard deviation to reduce scale differences.  
   - Applied a **moving average filter** to smooth out high-frequency noise.

3. **Leakage Model:**  
   Used the same Hamming Weight model as in Problem 1.

4. **CPA Implementation:**  
   - Computed hypothetical HW vectors for each key guess.  
   - Used Pearson’s correlation on the preprocessed traces.  
   - Selected sample points where leakage was strongest based on variance and correlation peaks.  
   - Ranked key guesses and outputted the results.

5. **Deliverables:**  
   - Saved the recovered key in `final_key.txt`.  
   - Created `byte_00.txt` to `byte_15.txt`, ranking key guesses.

---

###  a) Pre-processing Methods

- **Centering:**  
  Subtracted the mean from each sample point to make data zero-centered.

- **Normalization:**  
  Scaled traces by their standard deviation to standardize variance.

- **Smoothing:**  
  Applied a moving average filter with window size 5 to reduce noise.

---

###  b) Post-processing Methods

- Used the maximum correlation across sample points to select the most likely key guess.  
- Saved ranked lists for each key byte to allow detailed analysis.

---

###  c) Noise Consideration

- The traces clearly contained noise and measurement artifacts.  
- Applied filtering and normalization techniques to reduce random fluctuations.  
- Smoothed traces using a moving average filter to highlight consistent leakage.

---

###  d) AES Round Targeted

- The attack again targeted the **first round** of AES, specifically the XOR with the key followed by the S-box output.

---

###  e) Challenges and Solutions

**Challenges:**  
- Real traces included jitter and random noise, making correlation peaks weaker.  
- Variations in scale and offset across traces obscured patterns.

**Solutions:**  
- Trace centering and normalization ensured that sample values were comparable.  
- Moving average filtering improved signal visibility without distorting important features.  
- Careful computation of Pearson’s correlation allowed detection of weak but significant relationships.

---
