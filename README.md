# Web3 Copilot Extension

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-repo-blue)](https://github.com/JoeDkhar/SlitherAI-Toolkit)

This VS Code extension integrates GitHub Copilot, Slither static analysis, and Hardhat. It enforces a feedback loop where code suggestions are immediately checked for security vulnerabilities and deployment gas costs before leaving the editor.

## System Architecture

The extension orchestrates data flow between the AI, the static analyzer, and the local blockchain node:

```
┌─────────────────────────────────────────┐
│     VS Code Extension (TypeScript)      │
│  ┌────────────────────────────────────┐ │
│  │ Prompt Enhancement + Safety Checks │ │
│  └────────────────────────────────────┘ │
└─────────────────────────────────────────┘
  │              │              │
  ▼              ▼              ▼
Copilot      Slither        Hardhat
  │              │              │
  └──────────────┴──────────────┘
         │
         ▼
   Artifact Store
   (JSON, CSV, Gas)
```

## Features

| Component | Functionality |
|---|---|
| **Template Injection** | Pre-loads OpenZeppelin security patterns into the prompt context. |
| **Slither Runner** | Runs static analysis via Docker (30s timeout) on file save. |
| **Safety Mode** | Detects mainnet chain IDs and blocks accidental deployment. |
| **Gas Profiler** | Logs gas usage per function via Hardhat interactions. |
| **Artifact Logger** | Saves all run data to JSON/CSV for auditing. |

## Quick Start

### Prerequisites

- VS Code 1.93+
- Node.js 16+
- Docker (Required for Slither)
- Git

### Setup

1. **Clone the repo:**
```bash
git clone https://github.com/JoeDkhar/SlitherAI-Toolkit.git
cd web3-copilot-extension
```

2. **Install Extension:**
```bash
cd extension
npm install
npm run compile
```

3. **Install Hardhat Project:**
```bash
cd ../sample-hardhat
npm install
```

4. **Environment:**
```bash
cp .env.example .env
# Add your SEPOLIA_RPC_URL and PRIVATE_KEY
```

5. **Run:**
Press `F5` or navigate to **Run and Debug** → **Run Extension**.

## Running the Benchmarks

To run the full evaluation pipeline used in the performance metrics:

```bash
cd sample-hardhat
bash scripts/run_evaluation.sh
```

This will:

1. Compile contracts with the settings in `hardhat.config.js`.
2. Deploy to the local Hardhat node.
3. Run synthetic user interactions (`scripts/interact.js`).
4. Generate gas and security reports in `repro-artifacts/`.

## Reproducibility Notes

If you are replicating the results, note the following configurations:

- **Gas Optimization:** The reported 43% gas reduction relies on specific compiler settings. Ensure your `hardhat.config.js` includes:
```javascript
optimizer: { enabled: true, runs: 200 }
```

- **Slither Version:** Results may vary slightly depending on the Slither version. This project was tested using the `ghcr.io/crytic/slither` Docker image.
- **Synthetic Data:** The interaction scripts (`interact.js`) use automated transactions to ensure consistent gas measurements. These are not recorded from human typing sessions.
- **Scope:** The provided templates support EVM-compatible networks (Ethereum, Sepolia, Localhost).

## Directory Structure

```
web3-copilot-extension/
├── extension/                  # VS Code Extension Source (TypeScript)
│   ├── src/extension.ts        # Main logic (Safety checks, orchestration)
│   └── package.json
├── sample-hardhat/             # Testbed Project
│   ├── contracts/              # ERC20 Templates
│   ├── scripts/                # Automation & Evaluation Scripts
│   ├── reports/                # Output JSONs
│   └── hardhat.config.js       # Compiler Config
├── repro-artifacts/            # Raw Experiment Logs
└── slither-output.json         # Static Analysis Findings
```

## License

[MIT](LICENSE)
