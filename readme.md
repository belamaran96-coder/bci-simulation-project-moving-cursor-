# 🧠 NeuroCursor: A BCI Simulation Engine

### Turning Brainwaves into Digital Movement*

**Author: ELAMARAN B
**Stack: Python, MNE, NumPy, Matplotlib
**Environment: Google Colab / Jupyter

---

## 💡 The "Why" (Project Philosophy)
Brain-Computer Interfaces (BCI) often feel like sci-fi magic. But beneath the headset, BCI is just **Signal Processing**.

I built **NeuroCursor** to strip away the hardware dependency and focus on the software pipeline. The goal was to answer a specific engineering question: **"How do we take a noisy, chaotic electrical signal from the scalp and turn it into a smooth, reliable control input for a computer?"**

This project simulates that entire pipeline—from raw data acquisition to visual feedback—using real human EEG data.

---

## ⚙️ The Engineering Pipeline (How it Works)

This isn't just a random animation. It follows the standard **BCI Signal Chain** used in medical and research devices.

### 1. Data Acquisition (The Source)
Instead of a live headset, I ingest real-world EEG data from the **PhysioNet Motor Imagery Dataset**.
* **Focus:** Electrode `C3` (located over the Motor Cortex).
* **Paradigm:** Eyes Open vs. Eyes Closed. This triggers the **Alpha Rhythm (8-12 Hz)**, the most distinct signal in the human brain.

### 2. Signal Processing (The Filter)
Raw EEG is noisy (muscle movement, electrical hum). I apply a **Bandpass Filter (FIR Design)** to strictly isolate the **8-12 Hz** frequency band.
* *Why?* We don't care about the whole brain. We only care about the "Alpha" state (Relaxation).

### 3. Feature Extraction (The Math)
The computer can't understand waves; it understands numbers.
* I calculate the **Rolling Variance** (Power) of the filtered signal.
* **High Variance** = High Alpha Power = Relaxed State.
* **Low Variance** = Low Alpha Power = Focused State.

### 4. Translation Algorithm (The UI)
This is where the magic happens. I map the normalized signal power to a coordinate system ($Y$-axis).
* **Logic:** `Cursor_Position = f(Alpha_Power)`
* I added a **Linear Interpolation (Smoothing)** factor to simulate "Physics." Without this, the cursor would jitter uncontrollably.

---

## 🛠️ The Tech Stack

| Tool | Purpose |
| :--- | :--- |
| **MNE-Python** | The industry standard for neurophysiological data analysis. Used for loading `.edf` files and filtering frequencies. |
| **NumPy** | Used for vectorization and calculating signal variance (power) efficiently in real-time chunks. |
| **Matplotlib** | Used to build the custom "Game Board" UI and render the live animation loop (`FuncAnimation`). |
| **SciPy** | Backend math engine for the FIR filtering logic. |

---

## 🚀 How to Run (Interactive Mode)

Since this relies on heavy visualization libraries, it runs best in **Google Colab**.

1.  Open the Notebook.
2.  Run the **Install & Import** cell to set up the environment.
3.  Run the **Load Data** cell to download the EEG dataset.
4.  Run the **Simulation** cell to launch the visual interface.
5.  **Watch:** The Purple line is the brain; the Red Dot is the result.

---

## ⚠️ Developer Limitations & Disclaimer

This is a **Simulation**, not a medical diagnostic tool.

1.  **Latency:** Real-time BCI has a processing delay (latency) of 50-200ms. In this simulation, the data is pre-loaded, so "lag" is artificial.
2.  **Artifacts:** In a real headset, blinking or clenching your jaw creates huge electrical spikes. This dataset is "clean," so the cursor is smoother than a real-world $100 headset would be.
3.  **Subject Variability:** This code is tuned for *this specific subject*. A real production BCI requires a "Calibration Phase" for every new user to set the min/max thresholds.
