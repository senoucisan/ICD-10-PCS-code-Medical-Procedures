# Medical Procedures Dimension Table

## Project Overview

This project focuses on building a clean, unified **medical procedures dimension table** by integrating and processing two public datasets from the Agency for Healthcare Research and Quality (AHRQ) and the Centers for Disease Control and Prevention (CDC). The final output is a comprehensive dataset of ICD-10-PCS codes, enriched with procedure categories and ready for integration into a data warehouse or analytical model.

## Objective

The primary goal was to combine disparate data sources to create a single, reliable dimension table that can be easily joined with fact tables (e.g., medical claims, hospital events) for robust analysis. The process involved significant data cleaning, strategic merging, and the addition of a surrogate key to facilitate efficient data modeling.

## Data Sources

1.  **HCUP ICD-10-PCS Archive:** Provides the complete set of ICD-10-PCS procedure codes and their official long descriptions.
    *   Source: [AHRQ HCUP Website](https://hcup-us.ahrq.gov/toolssoftware/procedureicd10/procedure_icd10_archive.jsp)

2.  **CDC NHSN ICD-10-PCS Code Map:** Provides valuable additional context, specifically the **Procedure Category** mapping for procedures.
    *   Source: [CDC NHSN Excel File](https://www.cdc.gov/nhsn/xls/icd10-pcs-pcm-nhsn-opc.xlsx)

## Methodology

### 1. Data Acquisition & Inspection
*   Imported both datasets into Pandas DataFrames.
*   Conducted initial inspections using `.head()`, `.info()`, and `.describe()` to understand structure, data types, and data quality issues.

### 2. Data Cleaning
*   **Column Name Correction:** Fixed incorrectly parsed headers by promoting the first data row to column names and stripping extraneous characters like single quotes.
*   **Value Cleaning:** Removed leading/trailing single quotes from values in key columns to ensure consistency.
*   **Redundant Column Removal:** Identified and dropped a redundant column (`ICD-10-PCS CODE DESCRIPTION`) after successfully extracting its relevant information.

### 3. Data Enrichment & Merging
*   Performed a **left join** on the `ICD-10-PCS Code` column to append the `Procedure Category` from the CDC dataset to the comprehensive HCUP code list.
*   This merge strategy preserved all valid procedure codes from HCUP while enriching them with categories where available.

### 4. Data Modeling Preparation
*   Generated a unique **`procedure_id`** surrogate key to serve as the primary key for the dimension table.
*   This ensures optimal join performance with fact tables and decouples the data model from potential changes in the natural code system.


## Key Output

The final dataset (`complete_medical_procedures_dimension.csv`) contains the following columns:

| Column Name | Description | Example |
| :--- | :--- | :--- |
| **`ID`** | **Surrogate Key.** Unique integer identifier for the dimension table. | `10457` |
| **`ICD-10-PCS CODE`** | **Natural Key.** The official 7-character alphanumeric procedure code. | `0DV03CZ` |
| `PROCEDURE CLASS` | The first character of the code, representing the broad section. | `0` |
| `PROCEDURE CLASS NAME`| The name of the section/class. | `Medical and Surgical` |
| **`Category`** | **From CDC.** The high-level clinical category (e.g., Major, Minor). | `Major O.R.` |
| `Procedure` | The medical action performed (e.g., Extraction, Insertion). | `Extraction` |
| `Anatomy` | The body part or organ system on which the procedure was performed. | `Upper Arm` |
| `Qualifier` | A further specificity or modifier for the procedure. | `Zenith` |
| `Device_or_Method` | Any device used or method employed in the procedure. | `Autologous Tissue Substitute` |
| `Approach_or_Group` | The technique used to reach the site of the procedure. | `Percutaneous` |

## Technologies Used

*   **Python:** Core programming language.
*   **Pandas:** For data manipulation, cleaning, and merging.
*   **Jupyter Notebook:** For interactive development and documentation.
*   **NumPy:** For handling numerical operations.

## How to Run

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/senoucisan/ICD-10-PCS-code-Medical-Procedures
    cd medical-procedures-dimension
    ```

2.  **Set up a virtual environment and install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Run the Jupyter Notebook:**
    ```bash
    jupyter notebook notebooks/IDC_10_PCS_code.ipynb
    ```

4.  **Execute the cells in the notebook** to download the source data, run the cleaning pipeline, and generate the final dimension table.

## Note on Source Data

The raw data files are not stored in this repository due to their size and the terms of use of the source providers. The Jupyter notebook will download them directly from the official URLs when run.
