# Quantumine Mining Script

Quantumine Scraper is a Python-based web scraping tool designed to handle both static HTML pages and dynamic, JavaScript-driven content. It implements proxy rotation with on-the-fly validation, offers optional headless browser support using Playwright, and provides configurable output formats. This document explains how to set up the entire environment and run the scraper step by step.

Description

* Requests for static HTTP content retrieval.
* BeautifulSoup for parsing HTML content.
* Playwright for dynamic (JavaScript-rendered) content.
* Rich for optional progress bar visualization.
* Logging for both console and file logs.

The scraper can optionally use a pool of proxies for requests, verifying their health before usage. It supports rotating proxies automatically. The script allows saving extracted data in JSON or CSV format.

## Technical Details

1. Static Scraping (Requests + BeautifulSoup):
    * Fetches page HTML with requests.
    * Parses the returned HTML content with BeautifulSoup.
    * Extracts hyperlinks and their text as an example of data harvesting.
2. Dynamic Scraping (Playwright):
    * Initializes a headless Chromium browser for pages requiring JavaScript.
    * Runs minimal script-based anti-detection measures (e.g., removing webdriver properties, spoofing navigator settings).
    * Renders the page fully and extracts the HTML, which is then parsed by BeautifulSoup.
3. Proxy Management:
    * Loads proxies from specified sources (local files or URLs).
    * Optionally verifies each proxy by connecting to an external service (in the example, https://api.ipify.org).
    * Maintains a validated pool, rotating them on each request to mitigate IP-based bans.
4. Resilience and Reliability:
    * Retries failed requests using requests.adapters.HTTPAdapter and Retry.
    * Logs all errors to quantumine_scraper.log.
    * Exits gracefully if pages cannot be scraped.
5. Output Handling:
    * By default, saves results to output/results_<timestamp>.csv.
    * Can also save to JSON if --format json is specified.
    * Results are stored in an output/ directory which is automatically created if it does not exist.

The scraper can optionally use a pool of proxies for requests, verifying their health before usage. It supports rotating proxies automatically. The script allows saving extracted data in JSON or CSV format.

Requirements
1. Python 3.8 or later.
2. The following Python packages:
    * requests
    * beautifulsoup4
    * playwright
    * rich
    * urllib3
3. The Playwright browsers.

Highly Detailed Installation Steps

Below are very explicit instructions that allow any user or developer to set up Quantumine Scraper from scratch without guesswork.

** Ensure System Requirements
* Python 3.8 or above.
 standard Python package installer).

* A functioning internet connection to install dependencies and playwright browsers.

** Obtain the Code

If using GitHub (for example), clone the repository:

Basic Static Scraping

```
python quantumine_scraper.py -u https://example.com
```

git clone https://github.com/Retroplaya135/quantumine_mining_script/
cd quantumine_mining_script


Create and Activate a Virtual Environment
This step isolates dependencies from your system-wide Python installations.

On Linux/Mac:
```
python3 -m venv venv
source venv/bin/activate
```

On Windows (Command Prompt):

```
python -m venv venv
venv\Scripts\activate
```

#### Install Python Dependencies

```
pip install -r requirements.txt
```

This fetches and installs the core packages:

--> requests
--> beautifulsoup4
--> playwright
--> rich
--> urllib3
--> Others specified in the file (if any).

Install Playwright Browsers:
This step downloads the actual browser executables (Chromium, Firefox, WebKit), although the script typically defaults to Chromium. Run:
```
playwright install
```

Verify that this completes without error.
Optional: Modify or Confirm Proxy Configuration
If you plan to use proxies, create or update a file (e.g., proxies.txt) where each line contains a proxy address in the format ip:port or hostname:port.
If you do not need proxies, you can skip configuring them.

Test the Installation
Execute:
```
python quantumine_scraper.py --help
```

Usage Examples and Workflow

Command-Line Flags

```
usage: quantumine_scraper.py [-h] -u URLS [URLS ...] [-d] [-f {csv,json}] [-p PROXY_FILE] [-t TIMEOUT]

optional arguments:
  -h, --help                show this help message and exit
  -u URLS [URLS ...]        target URLs to scrape (one or more)
  -d, --dynamic             enable JavaScript rendering via Playwright
  -f {csv,json}, --format {csv,json}
                            specify output format (defaults to csv)
  -p PROXY_FILE, --proxy-file PROXY_FILE
                            file containing proxy servers (default: proxies.txt)
  -t TIMEOUT, --timeout TIMEOUT
                            request timeout in seconds (default: 30)
```

Basic Static Scraping


```
python quantumine_scraper.py -u https://example.com
```

----> Fetches the HTML of https://example.com using Requests.
----> Extracts all anchor tags with href attributes.
----> Saves the results to output/results_<timestamp>.csv by default.

Dynamic (JavaScript) Scraping

```
python quantumine_scraper.py -u https://example.com -d
```

Launches a headless Chromium instance.
Navigates to the specified URL and waits for the page to render.
Extracts anchor tags just like in static mode, but on a fully rendered DOM.

Using a Proxy File

```
python quantumine_scraper.py -u https://example.com -p custom_proxies.txt
```
Loads the proxy addresses from custom_proxies.txt.
Validates them with a short request to https://api.ipify.org?format=json.
Uses valid proxies in a rotation cycle.

```
Changing the Output Format to JSON
```
Saves the extracted data to output/results_<timestamp>.json.

Changing Timeout
```
python quantumine_scraper.py -u https://example.com -t 60
```
