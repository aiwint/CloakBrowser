# CloakBrowser

> A privacy-focused browser automation and scraping framework — Fork of [CloakHQ/CloakBrowser](https://github.com/CloakHQ/CloakBrowser)

[![CI](https://github.com/your-org/CloakBrowser/actions/workflows/ci.yml/badge.svg)](https://github.com/your-org/CloakBrowser/actions/workflows/ci.yml)
[![PyPI version](https://badge.fury.io/py/cloakbrowser.svg)](https://badge.fury.io/py/cloakbrowser)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Overview

CloakBrowser provides a high-level API for browser automation with built-in fingerprint spoofing, proxy rotation, and stealth capabilities. Built on top of Playwright, it helps you automate browsers while minimizing detection.

## Features

- 🕵️ **Stealth Mode** — Patches common browser fingerprinting vectors
- 🔄 **Proxy Rotation** — Built-in support for rotating HTTP/SOCKS proxies
- 🖥️ **Multi-Browser** — Supports Chromium, Firefox, and WebKit
- 🧩 **Plugin System** — Extensible architecture for custom stealth plugins
- ⚡ **Async-First** — Built with `asyncio` for high-throughput workloads
- 📸 **Screenshot & PDF** — One-line capture utilities

## Installation

```bash
pip install cloakbrowser
```

Install browser binaries:

```bash
playwright install chromium
```

## Quick Start

```python
import asyncio
from cloakbrowser import CloakBrowser

async def main():
    async with CloakBrowser(stealth=True) as browser:
        page = await browser.new_page()
        await page.goto("https://example.com")
        title = await page.title()
        print(f"Page title: {title}")
        await page.screenshot(path="screenshot.png")

asyncio.run(main())
```

### With Proxy Rotation

```python
import asyncio
from cloakbrowser import CloakBrowser

proxies = [
    "http://user:pass@proxy1.example.com:8080",
    "http://user:pass@proxy2.example.com:8080",
]

async def main():
    async with CloakBrowser(stealth=True, proxies=proxies) as browser:
        page = await browser.new_page()  # Automatically picks a proxy
        await page.goto("https://httpbin.org/ip")
        content = await page.content()
        print(content)

asyncio.run(main())
```

## Configuration

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `stealth` | `bool` | `True` | Enable stealth/fingerprint patching |
| `proxies` | `list[str]` | `None` | List of proxy URLs for rotation |
| `browser_type` | `str` | `"chromium"` | Browser engine to use |
| `headless` | `bool` | `True` | Run browser in headless mode |
| `timeout` | `int` | `60000` | Default navigation timeout (ms) |
| `locale` | `str` | `"en-US"` | Browser locale |
| `timezone` | `str` | `"America/New_York"` | Browser timezone |

> **Personal note:** I bumped `timeout` from `30000` to `60000` ms — the 30s default was too aggressive for slower sites I work with.

## Development

### Setup

```bash
git clone https://github.com/your-org/CloakBrowser.git
cd CloakBrowser
pip install -e ".[dev]"
playwright install
```

### Running Tests

```bash
pytest tests/ -v
```

### Linting

```bash
ruff check .
black --che
```
