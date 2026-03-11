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
- text-based reporting
- adapter skeletons
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
Responsible for presenting results to the operator.

Files:
- `app/reporting/text_reporter.py`
- `app/reporting/json_reporter.py`
- `app/reporting/docx_reporter.py`

## Current execution flow

### Keyword search
User input  
-> CLI parser  
-> search command  
-> input validation  
-> router  
-> Jackett adapter  
-> text reporter

### Infohash probe
User input  
-> CLI parser  
-> probe command  
-> input validation  
-> router  
-> BitTorrent probe adapter  
-> text reporter

### Magnet probe
User input  
-> CLI parser  
-> probe command  
-> input validation  
-> btih extraction  
-> router  
-> BitTorrent probe adapter  
-> text reporter

## Design principles
- keep the engine lightweight
- keep network adapters modular
- separate validation, routing, and adapter execution
- prefer metadata-only workflows where possible
- keep reporting independent from data collection
- make the codebase easy to test

## Current status
Implemented:
- parser
- validators
- command handlers
- router
- adapter skeletons
- text reporting
- unit tests for parser, validators, and router

Planned next:
- real Jackett integration
- Tor/Lynx integration
- metadata probe implementation
- result normalization and deduplication