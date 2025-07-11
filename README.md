Notebook Overview: Company Data Profiling, Validation, and Enrichment

This notebook performs profiling, cleansing, deduplication, API enrichment, validation, and reporting of company-level data. It compares local data against Companies House API responses, flags inconsistencies, and enriches records for downstream use.

########################################################################
Environment Setup
1. Create a virtual environment (recommended)
2. Create a requirements.txt with:

pandas
matplotlib
seaborn
numpy
sweetviz
regex
Ensure you're logged in to your JupyterHub instance via your browser.

Upload this project folder (or clone it from a Git repo) to your JupyterHub environment.

Open the notebook file (.ipynb) inside the JupyterHub interface.

Run the cells sequentially.

#######################################################################

Step 1 – Import Libraries and Load Dataset

Imports essential libraries: pandas, matplotlib, and seaborn.

Loads the main CSV dataset Company.csv into a DataFrame df.

Displays dataset shape and structure using .info().

########################################################################

Step 2 – Data Profiling

Preview data: Shows first few rows.

Summary stats: Uses describe(include='all') for numeric and categorical insights.

Missing value analysis: Calculates and visualizes missing data using a heatmap.

Duplication indicators:

Identifies top repeated CompanyName entries.

Displays count of unique values per column.

Prints most common values in PostTown for inspection.

########################################################################

Step 3 – Data Cleansing

Drops fully empty columns.

Strips whitespaces from column names and string fields.

Converts known numeric fields to Int64.

Uppercases company names for standardization.

Renames columns to remove problematic characters (e.g., Returns.NextDueDate → ReturnsNextDueDate).

Converts date-related fields to datetime.

Confirms cleansing with updated shape.

########################################################################

Step 4 – Deduplication

Groups data by CompanyNumber and merges duplicate rows using forward and backward fill.

Reduces dataset to unique companies (df_unique).

########################################################################

Step 5 – API Fetching and Preprocessing

Sets up authentication headers using an API key for Companies House API.

Iterates over all unique CompanyNumber values:

Fetches data via REST API.

Parses and stores primaryTopic content into a structured list.

Flattens the JSON API response to a structured DataFrame df_api.

Cleans column names and string values in df_api.

Converts dates and integers to appropriate types.

Renames API columns with _api suffix to prepare for comparison.

########################################################################

Step 6 – Data Merge for Validation

Merges df_unique and df_api on CompanyNumber.

Creates a new merged_df with both original and API-sourced fields for side-by-side comparison.

########################################################################

Step 7 – Field-by-Field Validation (Matching)

Defines a column_map that pairs original columns with their API counterparts.

Implements a function compare_values that:

Flags missing values from either source

Flags matches or mismatches

Applies this logic to each mapped pair and creates new QA columns (e.g., CompanyName_QA).

Resulting validated_df shows whether values match or not.

########################################################################

Step 8 – Data Enrichment

For each mismatch or missing original value, updates original column with corresponding API value.

Drops temporary _api and _QA columns.

Adds derived fields:

CompanyAgeYears: based on incorporation date

AccountsOverdue and ReturnsOverdue: flags if due dates have passed

FullAddress: constructs a readable address string

########################################################################

Step 9 – Reporting and Visualization

Plots a horizontal bar chart showing how many mismatches occurred in each field.

Counts companies with overdue accounts/returns.

Displays a bar chart of the top 10 most common post towns.

########################################################################

Key Outcomes:

Cleaned and deduplicated dataset of companies.

Validated and enriched using external API data.

QA insights into mismatched fields.

Enriched metadata like company age, address, and overdue filings.

Visual summaries for reporting mismatches and location patterns.
