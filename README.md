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
