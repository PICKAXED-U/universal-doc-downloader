# Universal Doc Downloader

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

A robust command-line tool to crawl, scrape, and convert modern documentation websites (React, Vue, Docusaurus, Sphinx, Redocly) into clean, offline PDFs.

Unlike standard HTML-to-PDF converters, this tool uses a **headless browser (Playwright)** to render client-side JavaScript, ensuring that Single Page Applications (SPAs) and dynamic sidebars are captured correctly.

## Features

- **Dynamic Spider:** Auto-detects sidebars, TOCs, and navigation menus even if rendered via JS. Includes fallback strategies for difficult sites.
- **Smart Sanitizer:** Strips "web" artifacts (Copy buttons, navbars, footers, breadcrumbs) to create a book-like reading experience.
- **Professional PDF Output:** Generates a custom Cover Page, Table of Contents with real page numbers, and rewires web links to point to internal PDF chapters.
- **Interactive Mode:** If the spider fails to find links automatically, a visible browser launches to allow manual selector identification.
- **Universal Support:** Pre-configured for Flask, React, Playwright, ReadTheDocs, and Redocly, with generic support for others.

## Prerequisites

- Python 3.8+
- **Windows Users:** [GTK3 Runtime](https://github.com/tschoonj/GTK-for-Windows-Runtime-Environment-Installer/releases) (Required for WeasyPrint PDF generation).

## Installation

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/YOUR_USERNAME/universal-doc-downloader.git
    cd universal-doc-downloader
    ```

2.  **Create a virtual environment (recommended):**

    ```bash
    python -m venv venv
    # Windows:
    .\venv\Scripts\activate
    # Mac/Linux:
    source venv/bin/activate
    ```

3.  **Install dependencies:**

    ```bash
    pip install -r requirements.txt
    ```

4.  **Install browser binaries:**
    This tool requires Chromium to render pages.
    ```bash
    playwright install chromium
    ```

## Usage

### Basic Usage

Pass the URL of the documentation homepage. The tool attempts to auto-detect the sidebar selector.

```bash
python doc_dl.py https://flask.palletsprojects.com/en/stable/
```

### Custom Output & Metadata

```bash
python doc_dl.py https://playwright.dev/python/docs/intro \
  --title "Playwright Python Manual" \
  --output playwright.pdf
```

### Testing (Limit Pages)

Download only the first 5 pages to test the layout before scraping the whole site.

```bash
python doc_dl.py https://docs.bria.ai/ --limit 5
```

### Manual Selector

If the auto-detection fails, inspect the website and provide the CSS selector for the sidebar navigation.

```bash
python doc_dl.py https://example.com/docs --selector "div.my-custom-menu"
```

### Debugging

Run with the browser visible to watch the scraping process.

```bash
python doc_dl.py https://example.com/docs --visible --verbose
```

## Command Line Options

| Flag         | Short | Description                           | Default         |
| :----------- | :---- | :------------------------------------ | :-------------- |
| `url`        |       | The target URL (Required)             | N/A             |
| `--output`   | `-o`  | Filename for the generated PDF        | `manual.pdf`    |
| `--title`    | `-t`  | Title displayed on the cover page     | "Documentation" |
| `--selector` | `-s`  | CSS selector for the sidebar          | Auto-detect     |
| `--limit`    | `-l`  | Stop after N pages (0 = download all) | `0`             |
| `--visible`  |       | Run browser in headful mode (visible) | `False`         |
| `--verbose`  | `-v`  | Enable detailed debug logging         | `False`         |

## License

Distributed under the MIT License. See `LICENSE` for more information.
