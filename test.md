# 🧹 LCA Data Cleaner & Analyzer

This project contains an R script designed to perform a comprehensive, multi-step cleaning process on messy and inconsistent Labor Condition Application (LCA) disclosure data from the [U.S. Department of Labor](https://www.dol.gov/agencies/eta/foreign-labor/performance).

The script goes beyond simple cleaning by creating a *"golden record"* for each employer and point of contact. It leverages various string manipulation, data wrangling, and fuzzy matching techniques to transform the raw data into a well-structured, reliable, and analysis-ready dataset.

---

## ✨ Key Features

The script's cleaning and enrichment process is broken down into several key operations:

### 1. Standardizing Core Information
> The script first enforces consistency across the dataset. It normalizes employer names by converting them to a uniform case and systematically removing legal suffixes like `Inc.`, `LLC`, `Corp.`, and `PLLC`. This ensures that "Google Inc." and "Google" are treated as the same entity. It also standardizes email domains by correcting common typos (e.g., `.cm` to `.com`) and formats all phone numbers into a consistent `(XXX) XXX-XXXX` layout.

### 2. Intelligent Typo Correction
> Using string distance algorithms (like Jaro-Winkler and OSA), the script hunts for and corrects typographical errors. For employer names, it identifies similarly named companies (e.g., "Micosoft" vs. "Microsoft") and uses the shared email domain as confirmation before merging them. It applies a similar logic to Point of Contact (POC) names, correcting typos by comparing an individual's name to their email username (e.g., fixing `Jhon Doe` to `John Doe` if the email is `john.doe@company.com`).

### 3. Resolving Ambiguity and DBAs
> The script tackles complex cases where a single company operates under multiple names. It cleans the "Doing Business As" (DBA) field, removing junk entries, and then intelligently determines if the DBA is the primary public-facing name. If so, it standardizes the employer's name to the DBA, creating a more recognizable identity. For companies with multiple web domains, it designates a primary domain (usually the `.com`) and lists others as secondary.

### 4. Strategic Filtering for High-Quality Data
> A critical step is the removal of low-quality or irrelevant records to isolate specific, identifiable decision-makers. The script actively filters out:
> - Entries where the employer POC uses a personal email address (`@gmail.com`, `@yahoo.com`, etc.).
> - Contacts with generic, non-personal email usernames like `hr@`, `legal@`, `info@`, or `contact@`.
> - Records where the law firm representing the employer appears to be filing for itself.

### 5. Data Enrichment and Aggregation
> The script adds value by creating a new `JOB CATEGORY` column, classifying contacts into functional roles like "EXECUTIVES," "HEAD OF HR," or "COUNSEL" based on their job titles. Finally, it produces multiple outputs: the full raw dataset, a thoroughly cleaned dataset, and powerful "rolled-up" summaries that aggregate the total number of visa petitions by company and by individual POC, providing immediate high-level insights.

---

## 🔧 Getting Started

To get a local copy up and running, follow these simple steps.

### Prerequisites

You will need R and RStudio installed on your machine. You will also need the following R libraries:

```r
# Install all required packages at once
install.packages(c(
  "readxl", "dplyr", "stringr", "strex", "tidyr", 
  "stringdist", "glue", "openxlsx", "purrr", "tibble", "stringi"
))
```

### Installation & Usage

1.  **Clone the repository:**
    ```sh
    git clone [https://github.com/your-username/lca_analysis.git](https://github.com/your-username/lca_analysis.git)
    ```
2.  **Set up the data directory:**
    Place your raw LCA Excel files inside the `data` directory. The script is configured to look for a structure like `data/FY2024/`.
3.  **Run the script:**
    Open the main R script (e.g., `clean_data.R`) in RStudio and run it from top to bottom to execute the cleaning process.

---

## 📊 Output

After running successfully, the script will generate a primary Excel file (e.g., `FY2024_DATA_V1.xlsx`) in the relevant data directory. This file contains several sheets:

-   **CLEANED DATA:** The fully cleaned and de-duplicated dataset after all transformations.
-   **ROLLED UP COMPANY:** An aggregated view showing visa counts and primary details for each unique company.
-   **ROLLED UP POCs:** An aggregated view of each unique Point of Contact and their associated companies.
-   **COMPANY LEVEL:** A summary of total visa positions per company.
-   **COMPANY POC LEVEL:** A summary of visa positions managed by each specific POC.