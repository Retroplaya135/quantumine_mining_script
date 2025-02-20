# Quantumine Mining Script

Quantumine Scraper is a Python-based web scraping tool designed to handle both static HTML pages and dynamic, JavaScript-driven content. It implements proxy rotation with on-the-fly validation, offers optional headless browser support using Playwright, and provides configurable output formats. This document explains how to set up the entire environment and run the scraper step by step.

Description

#### Requests for static HTTP content retrieval.
#### BeautifulSoup for parsing HTML content.
#### Playwright for dynamic (JavaScript-rendered) content.
#### Rich for optional progress bar visualization.
#### Logging for both console and file logs.

The scraper can optionally use a pool of proxies for requests, verifying their health before usage. It supports rotating proxies automatically. The script allows saving extracted data in JSON or CSV format.

