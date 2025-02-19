# CSV Combiner Script

This Python script iterates through a directory structure, combines CSV files within each subfolder, and saves the combined data into a new CSV file with a modified name. It's designed to handle CSV reports and consolidate them into a single summary file.

## Features

*   **Recursive Folder Traversal:** Uses `os.walk()` to process all subfolders within a specified root directory.
*   **CSV File Detection:**  Identifies CSV files using `glob.glob()`.
*   **Filename Pattern Matching:**  Employs regular expressions to handle filenames with date patterns (YYYY-MM-DD and YYYYMMDD).
*   **Dynamic Output Filename Generation:** Creates output filenames based on the input filenames, replacing the date with "Daily" (e.g., `INPUT_2023-10-27.csv` becomes `INPUT_Daily.csv`).
*   **Error Handling:**
    *   Skips folders if no CSV files are found.
    *   Gracefully handles empty CSV files.
    *   Catches and reports general exceptions during processing.
    *   Skips if the output files are existing
*  **Clear Output Messages:** Informs the user about progress, including skipped folders and errors.
* **Pandas-Based Combination:** Uses the pandas library for efficient CSV reading and concatenation.

## Requirements

*   Python 3.x
*   pandas library (`pip install pandas`)

## Usage

1.  **Install Dependencies:**
    ```bash
    pip install pandas
    ```

2.  **Modify the Script:**
    *   Update the `root_directory` variable in the script to point to the root directory containing your CSV files.  **Important:** Replace the placeholder path with the actual path to your directory.

3.  **Run the Script:**
    ```bash
    python your_script_name.py
    ```

    Replace `your_script_name.py` with the actual name of your Python file.

## Script Explanation

The script uses the `combine_csvs_in_folders` function to process the directory:

1.  **Directory Traversal:** `os.walk(root_dir)` recursively traverses the directory structure.
2.  **CSV File Identification:** `glob.glob(os.path.join(subdir, "*.csv"))` finds all CSV files in each subfolder.
3.  **Filename Parsing:**
    *   Regular expressions (`re.match()`) are used to extract the part of the filename before and after the date.  It supports both `YYYY-MM-DD` and `YYYYMMDD` date formats.
    *   If the filename doesn't match the expected format, the folder is skipped.
4.  **Output Filename Creation:** The output filename is constructed by replacing the date portion of the input filename with "_Daily".
5.  **CSV Combination:**
    *   `pd.read_csv()` reads each CSV file into a pandas DataFrame.
    *   `pd.concat()` combines the DataFrames into a single DataFrame; `ignore_index=True` is used for creating a continuous index.
    *   `combined_df.to_csv()` saves the combined DataFrame to a new CSV file.  `index=False` prevents writing the index to the file.
6.  **Error handling:** Catches exceptions and prevents crashing if empty files exist or other errors occur.

## Example

Suppose you have the following directory structure:
Use code with caution.
Markdown
My_Data/
├── Category1/
│ ├── Report_2024-03-10.csv
│ └── Report_2024-03-11.csv
└── Category2/
├── Stats_20240309.csv
└── Stats_20240310.csv
└── Stats_20240311.csv

After running the script, the directory will look like this:
Use code with caution.
My_Data/
├── Category1/
│ ├── Report_2024-03-10.csv
│ ├── Report_2024-03-11.csv
│ └── Report_Daily.csv
└── Category2/
├── Stats_20240309.csv
├── Stats_20240310.csv
├── Stats_20240311.csv
└── Stats_Daily.csv

## Notes

*   The script assumes that all CSV files within a given subfolder have the same structure (same columns).  If the CSV files have different structures, you might need to adjust the script to handle column mismatches (e.g., by aligning columns or using more sophisticated merging techniques).
* The script skips the combined output file if it exists to prevent data over-writing.
* **Remember to update the `root_directory` in the script to your actual directory path.**
