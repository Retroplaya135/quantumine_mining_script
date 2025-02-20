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

