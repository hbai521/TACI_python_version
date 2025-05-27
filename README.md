# 3D Calcium Imaging Analysis of Thermosensory Neurons in *Drosophila* Larvae
This repository contains Python scripts and a complate data analysis pipeline for calcium imaging analysis of warm cells in "Drosophila" larvae. 

---

## Table of Contents
- [Requirements](#requirements)
- [Installation](#installation)
- [Folder Structure](#folder-structure)
- [Installation](#installation)
- [Workflow](#workflow)
- [Example Data](#example-data)
- [Troubleshooting Tips](#Troubleshooting-Tips)
- [Citations](#Citations)

---

## Requirements
**ImageJ (Fiji)** https://imagej.net/software/fiji/<br>
**Python** https://www.python.org/<br>

## Installation

- Clone or download this repository
- Create a python virtrual environment
- Install dependencies
   ```bash
   python -m pip install --upgrade pip
   ```
   ```bash
   python -m pip install -r requirements.txt
   ```
- Test scripts usging demo data.<br>

   You can test the two main python scripts using demo datasets provided below.<br>
   **Download demo datasets (Google Drive)** (https://drive.google.com/file/d/1bbxFN6tgdLj6vK6GaE3isFaxtnnAL_0F/view?usp=sharing)<br>
   After downloading, extract the ZIP file. It contains two folders:<br>
   &nbsp; &nbsp; &nbsp; &nbsp; **`demo_CIAnalysis_test`** - for testing '\CIAnalysis_120min.py'<br>
   &nbsp; &nbsp; &nbsp; &nbsp; **`demo_CITbind_test`** - for testing '\CITbind_dynamic.py'<br>

  **Run the following commands in your terminal:** <br>
   ```bash
   python .\CIAnalysis_120min.py -i path/demo_CIAnalysis_test --merge --cell_type DOWC
   ```
   ```bash
   python .\CITbind_dynamic.py -i path/demo_CITbind_test -n 2
   ```
## Folder-Structure
project/
│
├── MAX_DOWC001.tif            # Maximum projection for preview
├── Analog - 2024-08-20.csv      # Temperature log
├── DOWC001_stacks/           # 21 z-slice TIFFs (Z01 to Z21)
│   ├── Z01.tif
│   └── ...
├── Analysis/
│   ├── background_i.xlsx       # 5 background values per z-slice
│   ├── Neuron 0/
│   │   ├── Mean_Intensity03.csv
│   │   └── ...
│   ├── Neuron 1/
│   ├── Mean_Intensity08.csv
│   │   └── ...
│   ├── results/                # Output CSVs and plots
│   │   ├── merged_data/
│   │       ├── merge_data.csv
│   │       └── Average_dF.png

## Description 
These python scripts allows batch processing and analysis of calcium imaging datasets collected from Drosophila larvae or other small model systems. 
The pipline has three main stages:
1. Fluorescence extraction using TrackMate in ImageJ<br>
   insert the user manual for the steps in Trackmate here!
2. Primary Analysis (CIAnalysis_120min.py)<br>
   This is the main script for the processing individual calcium imaging datasets.<br>
   It automatially: 
   **•** Generates background values from background_i.xlsx<br>
   **•** Extracts fluorescence change <br>
   &nbsp; &nbsp; &nbsp; &nbsp; **•** **DOCC**: Uses the first timepoint fluorescence as F₀<br>
   &nbsp; &nbsp; &nbsp; &nbsp; **•** **DOWC**: Uses the minimum fluorescence in the first cooling and warming cycle as Fmin<br>
   **•** Merges individual neuron results into a summary CSV and plot <br>
Supporting modules that run internally:<br>

| File Name                    | Description                                                  |
|------------------------------|--------------------------------------------------------------|
| `Generate_background.py`     | Generates background list creation.                          |
| `individual_dFoverF0_1.py`   | ΔF/F0 calculation for DOCC.                                  |
| `individual_dFoverF0_DOWC.py`| ΔF/Fmin calculation for DOwC.                                |
| `merge_dFoverF0_1.py`        | Merges all neuron result files into a combined dataset.      |
| `utility.py`                 | Shared utility functions across all modules.                 |

3. Temperature Binding (CITbind_dynamic.py) <br>
After individual sample processing,  this script combines temperature data and calcium imaging results and generates aligned dual-axis plots.<br>
**•** Generate summarized time-synchronized combined CSVs<br>
**•** Generate plots of ΔF/F₀ and temperature (two versions: default and y-range limited)<br>

4.  Summary Visualization (data_summary.py) <br>
Once all individual samples have been processed, this script: <br>
**•** Computes mean ± SEM<br>
**•** Outputs a stacked plot (combined_gradient_plot.png) showing:<br>
&nbsp; &nbsp; &nbsp; &nbsp; **•** Top panel: Average ΔF/F₀ or ΔF/Fmin<br>
&nbsp; &nbsp; &nbsp; &nbsp; **•** Bottom panel: Temperature curve<br>


