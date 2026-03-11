# Architecture

## Project goal
Build a lightweight CLI-based OSINT probe engine that supports:
- text-based Tor browsing through Lynx
- centralized keyword search through Jackett
- BitTorrent metadata-oriented probing through infohash and magnet inputs

## MVP scope
The first MVP includes:
- CLI argument parsing
- input validation for keyword, infohash, and magnet
- adapter routing
- text-based output
- basic unit tests

The MVP does not yet include:
- reverse image search
- DOCX/PPTX reporting
- AI analysis
- I2P support
- full qBittorrent replacement
- advanced DHT crawling

## High-level modules

### CLI
Responsible for parsing user input and dispatching commands.

Files:
- `app/main.py`
- `app/cli/parser.py`
- `app/cli/commands.py`

### Core
Responsible for routing, models, normalization, and deduplication.

Files:
- `app/core/router.py`
- `app/core/models.py`
- `app/core/normalizer.py`
- `app/core/deduplicator.py`

### Adapters
Responsible for interacting with external tools and services.

Files:
- `app/adapters/tor_lynx_client.py`
- `app/adapters/jackett_client.py`
- `app/adapters/qbittorrent_client.py`
- `app/adapters/bittorrent_probe.py`

### Reporting
Responsible for presenting or exporting results.

Files:
- `app/reporting/text_reporter.py`
- `app/reporting/json_reporter.py`
- `app/reporting/docx_reporter.py`

## Planned query flow

### Keyword search
User input -> CLI parser -> search command -> Jackett adapter -> normalized output

### Infohash probe
User input -> CLI parser -> probe command -> validator -> bittorrent probe adapter -> normalized output

### Magnet probe
User input -> CLI parser -> probe command -> validator -> bittorrent probe adapter -> extract btih -> normalized output

## Design principles
- keep the engine lightweight
- keep network adapters modular
- separate CLI logic from probing logic
- prefer metadata-only workflows where possible
- keep reporting independent from data collection

## Deployment direction
- development in VS Code
- repository hosted in GitLab
- likely runtime target: Linux
- optional operator usage from PowerShell