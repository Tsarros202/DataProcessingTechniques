# HotelSkylines

This repository presents an end-to-end pipeline for **data acquisition, preprocessing, and spatial skyline analysis** over hotel listings. The project integrates web scraping, data cleaning, and multidimensional query processing to illustrate practical applications of data processing and spatial indexing techniques.

---

## Repository Structure

The repository consists of:

- **Python scraping script** — collects hotel data from Booking.com using browser automation  
- **XLSX dataset** — raw scraped output  
- **Jupyter notebook** — performs data cleaning and transformation  
- **CSV dataset** — structured dataset derived from preprocessing  
- **Maven project** — constructs an R-tree and performs skyline computations  

---

## Data Acquisition

The Python script utilizes the **Playwright** framework to implement a semi-automated browsing workflow that retrieves hotel listings based on predefined filters:

- destination: Athens  
- check-in / check-out dates: user-defined  
- distance constraint: accommodations within 3 km of the city center  
- occupancy: single adult  

### Extracted attributes
- hotel name  
- price  
- distance from city center  

The script automates navigation, dynamic page scrolling, and structured data extraction. Interaction with the “load more” control remains manual.

> The scraping script and dataset are provided strictly for educational purposes.

---

## Data Preprocessing

The Jupyter notebook performs data cleaning and transformation, producing a structured two-dimensional representation suitable for spatial analysis:

- **x:** price  
- **y:** distance from city center  

The notebook includes preprocessing steps such as filtering, normalization, and visualization to support interpretability and reproducibility.

---

## Spatial Skyline Analysis

The Maven module applies spatial indexing and skyline computation techniques using the **davidmoten/rtree** library.

### R-tree Construction
- Loads points from `final_hotels_dist.csv`
- Builds an R-tree using a quadratic split strategy (maximum 4 children per node)
- Enables efficient multidimensional range queries

### Skyline Computation
- Implements a **Branch-and-Bound Skyline (BBS)** algorithm with complexity `O(n log n)`
- Validates results against a brute-force skyline method with complexity `O(n²)`

### Synthetic Data Experiments
Performance is evaluated on datasets with different correlation structures:

- strong positive correlation  
- strong negative correlation  
- no correlation  


::contentReference[oaicite:0]{index=0}


---

## How to run

### Requirements
- Python 3.9+
- Playwright (`pip install playwright`)
- Jupyter Notebook / JupyterLab
- Java 17+
- Maven

---

### 1. (Optional) Run Scraper
```bash
pip install playwright
playwright install
python scraper.py
```

### 2. Clean Data
```bash
jupyter notebook
```

Open the provided notebook and run all cells to produce the **final CSV dataset**

### 3. Run skyline Analysis
```bash
cd mavenproject
mvn clean package
mvn exec:java -Dexec.mainClass="mavenproject.App"
```
This will:
- load the processed CSV dataset
- build the R-tree
- compute skyline results

- compare with brute-force output
