## Artifact Overview

This repository hosts the complete artifact — including source code, data preprocessing scripts, the **DeepURG** model, and experiment notebooks — for our **SIGMOD 2025** submission:

### **Title:**  
*Smart HD Maps Caching: Aiding the Realization of Efficient MEC-based Vehicle Route Planners*

This document outlines the directory structure, datasets, environment, dependencies, and steps required to reproduce the results of the artifact.

---

### **Directory Structure**
This document outlines the directory structure, datasets, environment, dependencies, and steps required to reproduce the results of the artifact.

HD_MapsArtifact_SIGMOD2025/
├── Comparative_Analysis.ipynb          → Jupyter notebook for performance evaluation and result visualization
├── DeepURG_for_UWP.ipynb               → DeepURG model implementation for Urgency-Weighted Popularity (UWP)
├── DeepURG_for_Popularity.ipynb        → DeepURG model implementation for Popularity-based prediction
├── DataCleaning_SanFrancisco.ipynb for initial transformation of San Francisco taxi data.
├── Dataset_Preprocessing_for_Popularity.ipynb → Preprocessing pipeline for popularity-based datasets
├── Dataset_Preprocessing_for_UWP.ipynb → Preprocessing pipeline for UWP datasets
├── San_Francisco/                      → Directory containing the San Francisco processed trajectory data
├── tdrive/                             → Directory containing the T-Drive (Beijing) trajectory data
└── README.md                           → Main documentation file describing the artifact setup and usage

### Dataset Information

**T-Drive (Beijing):**
The T-Drive dataset, available from [Microsoft Research](https://www.microsoft.com/en-us/research/publication/t-drive-trajectory-data-sample/), contains raw taxi trajectory data collected in Beijing. Please download directly and placed in the specified local directory named `tdrive/`.

**San Francisco Cab Traces:** 
The San Francisco Cab Traces dataset can be accessed via [DOI: 10.15783/C7J010](https://dx.doi.org/10.15783/C7J010). The original raw data is transformed by enumerating vehicles as 1, 2, 3, …; reordering latitude and longitude coordinates; and sorting all trajectories chronologically in ascending order. The processed data is stored in the local directory `San_Francisco/`. Please download the dataset and use `DataCleaning_SanFrancisco.ipynb`.

### Environment and Dependencies

The artifact was developed and tested using the following environment specifications:

- **Python:** 3.12.11  
- **Operating System (OS):** Ubuntu 24.04.2 LTS  
- **Conda Environment:** True  

---

### Quick Installation

The following essential libraries are required to run the programs.

pip install pandas==2.3.1 numpy==2.3.2 osmnx==2.0.6 networkx==3.5 geopandas==1.1.1 shapely==2.1.1 tensorflow==2.20.0 keras==3.11.2
Using conda (Alternative):
conda install -c conda-forge pandas=2.3.1 numpy=2.3.2 osmnx=2.0.6 networkx=3.5 geopandas=1.1.1 shapely=2.1.1 tensorflow=2.20.0 keras=3.11.2

### Reproduction Steps

Follow the sequence below to fully reproduce the results.

1) **Run Preprocessing Notebooks.**  
Execute `Dataset_Preprocessing_for_UWP.ipynb` and `Dataset_Preprocessing_for_Popularity.ipynb`. These generate the UWP tensor, the Popularity tensor, and the map-segment vector required for downstream experiments. **Important:** set the dataset directory name correctly based on the dataset in use (`tdrive` or `San_Francisco`).

2) **Train DeepURG for UWP.**  
Run `DeepURG_for_UWP.ipynb`. This trains the DeepURG LSTM engine for UWP and saves the predictions.

3) **Train DeepURG for Popularity and Evaluate Baselines.**  
Run `DeepURG_for_Popularity.ipynb`. This trains DeepURG for Popularity and saves predictions. It also runs the UPPN (Urgency-Prioritized Popularity-based Prefetching) experiment—prefetching map segments without further actions—and writes results to `Penalty_Evaluation_PPPN.xlsx`.

4) **Run Comparative Analysis.**  
Execute `Comparative_Analysis.ipynb`. This compares against LRU, LFU, DeepCache, and SURC, saving results to `Penalty_Evaluation_SURC.xlsx` and `Penalty_Evaluation_Baselines.xlsx`.

---

### Notes on Dataset Selection

- All configurable parameters are documented inline within the notebooks. Running as-is reproduces the **T-Drive** results reported in the paper.
- To reproduce **San Francisco** results, adjust the relevant notebooks (e.g., `Dataset_Preprocessing_for_UWP.ipynb` and companions):
  - Change the dataset directory to `San_Francisco`.
  - Update geographic bounds (`BBOX`) and `timezone`.
  - Modify the Area-of-Interest indices: T-Drive uses `[21, 22]`; San Francisco uses `[12, 13]`.
- In `Comparative_Analysis.ipynb`, set the total number of map segments:
  - **T-Drive:** `NMS = 5267`  
  - **San Francisco:** `NMS = 667`  
  (This is typically a simple comment/uncomment toggle.)
