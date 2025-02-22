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

```
                       +----------------------------------+
                       |      Quantumine Scraper          |
                       | (Main Orchestration Script)      |
                       +----------------------------------+
                                     |
         +---------------------------+---------------------------+
         |                                                       |
+--------v---------+                                    +--------v---------+
|  TemporalScraper |                                    | QuantumProxyManager |
|    (Scraping     |                                    |   (Proxy Rotation  |
|     Engine)      |                                    |    and Validation) |
+------------------+                                    +--------------------+
         |                                                       |
         |                                                       |
+--------v---------+                                   +-------------v-------------+
| Static Scraper   |                                   | Proxy Harvesting &        |
| (Requests, BS4)  |                                   | Validation (IP Checks)     |
+------------------+                                   +----------------------------+
         |
         |
+--------v---------+
| Dynamic Scraper  |
| (Playwright)     |
+------------------+
```

* Static Scraping:
Uses the requests library to fetch HTML content and BeautifulSoup for parsing. This method is effective for pages that deliver content in static HTML.

* Dynamic Scraping:
Leverages Playwright for rendering JavaScript-driven pages. A headless Chromium instance is used to obtain fully rendered DOM content before parsing with BeautifulSoup.



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

```
             +---------------------------+
             |     main() Function       |
             +---------------------------+
                         |
                         v
             +---------------------------+
             | parse_args() & Config     |
             +---------------------------+
                         |
                         v
             +---------------------------+
             |  TemporalScraper Object   |
             |   (Initialization)        |
             +---------------------------+
                         |
                         v
             +---------------------------+
             |   Execute Scraping Loop   |
             +---------------------------+
                         |
           +-------------+-------------+
           |                           |
+----------v----------+       +--------v---------+
| _scrape_static(url) |       | _scrape_dynamic(url) |
+---------------------+       +---------------------+
           |                           |
           +-------------+-------------+
                         v
             +---------------------------+
             | harvest_data(soup)        |
             +---------------------------+
                         |
                         v
             +---------------------------+
             |  save_results()           |
             +---------------------------+
```

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

```
[User CLI Input]
       │
       ▼
[parse_args() Function]
       │
       ▼
[Generate Configuration Dictionary]
       │         ┌────────────────────┐
       │         │Command-line Flags  │
       │         │(URLs, timeout, etc.)│
       │         └────────────────────┘
       ▼
[Initialize TemporalScraper(config)]


Basic Static Scraping
```

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
Increases the per-request timeout to 60 seconds.

Internal Architecture Explanation

Quantumine Scraper is composed of multiple classes and methods that cooperate. A simplified pseudo-code representation follows.

```
main():
    args = parse_command_line_arguments()
    config = create_configuration_dict(args)
    scraper = TemporalScraper(config)
    scraper.execute()
```
```
class QuantumProxyManager:
    constructor(proxy_sources):
        # reads proxies from file(s) or URLs
        # filters out invalid or non-responsive proxies
        # maintains a rotation cycle
```
```
    method get_next_proxy():
        # returns next valid proxy from the cycle
```

```
[Load Proxies from File/URL]
       │
       ▼
[Harvest Proxies (_harvest_proxies)]
       │
       ▼
[Validate Proxies (_validate_proxies)]
       │
       ▼
[Create Proxy Cycle Iterator]
       │
       ▼
[get_next_proxy() Called for Each Request]
       │
       ▼
[Use Proxy for Request]
       │
       ▼
[Did Request Succeed?]
       │
 ┌─────┴─────┐
 │           │
Yes          No
 │           │
 ▼           ▼
[Proceed]   [Log Error & Increment Failure Count]
               │
               ▼
    [Threshold Exceeded?]
               │
        ┌──────┴──────┐
        │             │
       Yes           No
        │             │
        ▼             ▼
[Remove Proxy from Cycle] [Keep Proxy for now]
```

## Explanation of Key Modules
* _create_stealth_session: Creates a requests.Session object with custom headers and a retry strategy to handle common network errors and status codes that suggest transient failures.
* _init_playwright: Launches a headless browser with certain command-line flags to reduce detection (for instance, disabling certain features that reveal automated contexts).
* _scrape_static: Uses the requests.Session object to fetch HTML content and parse it with BeautifulSoup.
_scrape_dynamic: Creates a new browser context, optionally sets a proxy, navigates to the URL, executes minimal stealth * JavaScript modifications, and then obtains the DOM as HTML for parsing.
harvest_data (placeholder for ML integration): The current default is simple anchor extraction. Users can replace or * * * enhance this method to utilize machine learning-based detection of desired elements (referred to as the "Neuroscraping Engine"). At the moment, it is a placeholder that must be adapted to suit advanced extraction needs.


Customization and Placeholder Logic

* Machine Learning Integration: In harvest_data, you might include a classifier or named entity recognition system to dynamically identify product titles, prices, or relevant text blocks. The current snippet is purely a placeholder showing how an expanded method might be plugged in.
* Proxy Validation: The _is_valid_proxy method is intentionally simplistic. For production systems, you may want to integrate more rigorous tests (location-based checks, speed tests, or usage-based blacklisting).
* Scraping Logic: If you need advanced selectors (e.g., scraping tables, forms, or JSON embedded in the DOM), adapt _scrape_static, _scrape_dynamic, or the harvest_data method. You could also intercept network requests in Playwright to capture XHR data.


## Example Deployment Scenario

Below is one possible approach to deploying Quantumine Scraper on a remote server or container environment:

* Prepare a Server
A typical Linux-based system (e.g., Ubuntu 20.04) with Python 3.8+ installed.
* Ensure enough RAM if you plan to run many concurrent Playwright sessions.
Clone and Configure
  
```
git clone https://github.com/youruser/quantumine_scraper.git
cd quantumine_scraper
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
playwright install
```

Error Handling and Logging Flow:
Robust error handling is essential for long-running scraping jobs. This flow shows how exceptions are captured and logged.

```
[Execute HTTP Request / Browser Navigation]
       │
       ▼
   [Try Block]
       │
       ▼
 [Operation Successful]
       │
       ▼
   [Return Response]
       │
   ────┬────
       │ (Exception Occurs)
       ▼
[Catch Exception]
       │
       ▼
[Log Exception Details (logging.error)]
       │
       ▼
[Return None or Trigger Fallback Mechanism]
```

Key Points:

Every external call (HTTP requests, page navigation) is wrapped in a try/except block.
The logging system records both successful operations and errors with timestamps.


```
[Aggregated Results Ready]
       │
       ▼
[Call save_results()]
       │
       ▼
[Check: Is Results Empty?]
       │
       ▼
[Create Output Directory if Not Exists]
       │
       ▼
[Generate Timestamped Filename]
       │
       ▼
[Check Output Format (CSV or JSON)]
       │
       ├──────────── CSV ────────────┐
       │                             │
       ▼                             ▼
[Write CSV File]             [Write JSON File]
       │                             │
       ▼                             ▼
[File Saved in 'output/' Directory]
```

Key Points:

The timestamped file naming ensures unique filenames for each run.
The format check allows switching between CSV and JSON based on user preference.


