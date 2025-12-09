# Accessibility Overlay Scanner

This tool scans a list of URLs (from a sitemap, CSV file, or remote URL) to detect the presence of "Accessibility Overlays" (e.g., AccessiBe, UserWay, AudioEye) and other 3rd-party widgets that may impact accessibility.

It generates a CSV report identifying which domains are using which overlays and provides a statistical summary at the end of the scan.

## 1. Installation

This project requires Python 3. Because modern macOS/Linux environments are "externally managed," it is best to use a virtual environment.

### Step 1: Create a Virtual Environment
Run this command in your project folder to create an isolated environment:

```bash
python3 -m venv venv


Step 2: Activate the Environment
You must activate the environment every time you want to run the script:
macOS / Linux:

Bash


source venv/bin/activate


(You should see (venv) appear at the start of your terminal prompt).
Windows:

Bash


.\venv\Scripts\activate


Step 3: Install Dependencies
Install the required libraries:

Bash


pip install requests beautifulsoup4


2. Usage
You can feed the script a remote URL (Sitemap/CSV) or a local file path.
Basic Command
This will scan the file and generate a report automatically named based on the input file and today's date (e.g., current-full-overlays-2025-12-09.csv).

Bash


python find-overlay.py current-full.csv


Command Line Arguments
Argument
Description
source
(Required) Path to local file (.csv, .xml) or URL.
--output
Custom output filename (overrides the auto-generated date name).
--simple
Outputs a CSV with only two columns: url and detected_overlay.
--no-csv
Runs the scan and prints stats to the console but does not save a file.

Examples
1. Scan a local CSV and get a simple report (URL + Overlay only):

Bash


python find-overlay.py current-full.csv --simple


Output file: current-full-overlays-2025-12-09.csv
2. Scan a remote Sitemap XML:

Bash


python find-overlay.py [https://example.com/sitemap.xml](https://example.com/sitemap.xml)


3. Run a "Dry Run" (Stats only, no file saved):

Bash


python find-overlay.py current-full.csv --no-csv


3. Input File Formats
The tool is robust and accepts two main formats:
1. Structured CSV (e.g., Government Data)
If your CSV has headers, the tool looks for a column named "Domain name" (or similar). It automatically cleans the data by:
Ignoring email addresses (e.g., security@agency.gov).
Adding https:// if missing.
Deduplicating domains (only scans example.com once, even if listed 50 times).
2. Simple List
A text file or simple CSV with just one URL per line works perfectly too.
4. Output & Statistics
CSV Report
The default output contains:
url: The scanned URL.
status: HTTP Status Code (200, 404, etc.).
detected_overlay: Name of the overlay found (e.g., "UserWay") or "None Found".
other_widgets: Other common widgets detected (e.g., "Intercom", "Zendesk").
Console Statistics
At the end of every run, you will see a summary like this:

Plaintext


========================================
FINAL STATISTICS
========================================
Total URLs Scanned: 21470

--- Status Codes ---
200: 21000
404: 400
Conn Error: 70

--- Overlays Detected ---
UserWay: 150
AudioEye: 45
AccessiBe: 20

--- Summary ---
Domains with Overlays: 215 (1.00%)
========================================


5. Deactivation
When you are done, exit the virtual environment:

Bash


deactivate


