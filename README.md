# Web3 Copilot Extension: AI-Assisted Smart Contract Development with Integrated Security Analysis

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-repo-blue)](https://github.com/JoeDkhar/SlitherAI-Toolkit)

A VS Code extension that orchestrates GitHub Copilot, Slither static analysis, and Hardhat deployment to create a closed feedback loop for secure, efficient smart contract development on EVM networks.

## Quick Start

### Prerequisites
- VS Code 1.93+
- Node.js 16+
- Docker (for Slither analysis)
- Git

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/JoeDkhar/SlitherAI-Toolkit.git
   cd web3-copilot-extension
   ```

2. Install extension dependencies:
   ```bash
   cd extension
   npm install
   npm run compile
   ```

3. Install Hardhat project dependencies:
   ```bash
   cd ../sample-hardhat
   npm install
   ```

4. Configure environment (Sepolia testnet):
   ```bash
   cp .env.example .env
   # Edit .env with your SEPOLIA_RPC_URL and PRIVATE_KEY
   ```

5. Load the extension in VS Code:
   - Press `Ctrl+Shift+D` (Debug)
   - Select "Run Extension"
   - The extension will open in a new VS Code window

### Running the Evaluation Pipeline

```bash
cd sample-hardhat
npm run compile
npm run deploy     # Deploy to localhost (hardhat node must be running)
npm run interact   # Run interaction scripts
npm run gen:summary # Generate interaction report
```

Or run the full pipeline:
```bash
bash scripts/run_evaluation.sh
```

## Features

| Feature | Status | Description |
|---------|--------|-------------|
| **ERC-20 Template Insertion** | ✅ | Insert OpenZeppelin-based ERC-20 template with security checks |
| **NatSpec Doc Generation** | ✅ | Auto-generate NatSpec comments for contracts |
| **Slither Integration** | ✅ | Run static analysis via Docker (30-sec timeout) |
| **Deployment Info Display** | ✅ | Show deployment metadata per network |
| **Hardhat Snippet Generation** | ✅ | Generate contract interaction snippets from ABI |
| **Web3 Security Chat** | ✅ | Chat interface for security questions |
| **Gas Reporting** | ✅ | Automated gas profiling and comparison |

## System Architecture

```
┌─────────────────────────────────────────────────────┐
│          VS Code Extension (TypeScript)              │
│  ┌─────────────────────────────────────────────┐   │
│  │  Prompt Enhancement + @web3 Chat Agent      │   │
│  └─────────────────────────────────────────────┘   │
└────────────┬──────────────┬──────────────────────────┘
             │              │
      ┌──────▼──────┐  ┌────▼──────────────┐
      │   Copilot   │  │ Regex-based Fast  │
      │   (Pro)     │  │ Security Scan     │
      └──────┬──────┘  └────┬──────────────┘
             │              │
      ┌──────▼──────────────▼──────────┐
      │  Slither (Docker, async)       │
      │  30-second timeout             │
      └──────┬───────────────────────┬─┘
             │                       │
      ┌──────▼──────┐        ┌──────▼──────┐
      │   Hardhat   │        │   Gas       │
      │  Deployment │        │  Reporter   │
      └──────┬──────┘        └──────┬──────┘
             │                      │
      ┌──────▼──────────────────────▼───┐
      │  Artifact Store                  │
      │  (JSON, CSV, LaTeX)              │
      │  Timestamped & Versioned         │
      └──────────────────────────────────┘
```

## Reproducibility & Limitations

### Configuration Dependencies

**Gas Optimization:** The 43% deployment gas reduction is attributed to compiler optimization settings enforced in `hardhat.config.js`. All deployments use:

```javascript
solidity: {
  version: "0.8.20",
  settings: {
    optimizer: {
      enabled: true,
      runs: 200,
    },
  },
}
```

This standardization prevents accidental unoptimized builds. Gas metrics **must** be compared only with identical compiler settings.

### Slither Version Sensitivity

Static analysis results (in `slither-output.json`) depend on Slither version and detector configuration. This project uses Slither v0.10.0 via Docker:

```bash
docker run --rm -v "$PWD:/src" ghcr.io/crytic/slither \
  slither /src/sample-hardhat/contracts/ERC20.sol --json
```

Different Slither versions may produce different finding counts. To ensure reproducibility, all Slither invocations are captured in `scripts/run_slither.sh` and `scripts/run_slither.ps1`.

### Synthetic Evaluation Methodology

The evaluation used **automated scripted interactions** (defined in `scripts/interact.js`) rather than human user studies:

- **Transfer, mint, and ownership operations** are executed deterministically via JavaScript.
- **Hardhat node** (localhost) and **Sepolia testnet** are the only networks tested.
- **Copilot completions** are nondeterministic; identical prompts may yield different suggestions. This project did not control for Copilot variance in the reported metrics.

**Justification:** Synthetic scripts were chosen to isolate system technical performance (latency, gas optimization reproducibility) from human behavioral variance (typing speed, decision patterns). A human study would introduce confounding variables; scripted evaluation establishes a controlled technical baseline.

**Limitation:** The reported metrics do not capture developer understanding, trust in recommendations, or learning curves. These require future user studies.

### Scope: Syntactic vs. Semantic Analysis

**Included:** Syntactic vulnerabilities detectable via Slither (missing events, immutability, reentrancy guards, access control).

**Excluded:** Semantic economic vectors (oracle manipulation, flash loan attacks) requiring symbolic execution or formal verification. These tools introduce 2–5 minute latencies, incompatible with real-time IDE feedback. Future work will explore asynchronous background analysis.

### Platform Scope

- **Supported:** EVM-compatible networks (Ethereum, Sepolia, Hardhat localhost)
- **Not tested:** Solana, Cosmos, non-EVM L1/L2s

Templates and analyzers are Solidity-specific. Multi-chain support is a future goal.

## Directory Structure

```
web3-copilot-extension/
├── extension/                          # VS Code Extension (TypeScript)
│   ├── src/extension.ts                # Main extension code
│   ├── package.json                    # Extension manifest & commands
│   └── tsconfig.json                   # TypeScript config
├── sample-hardhat/                     # Hardhat Smart Contract Project
│   ├── contracts/
│   │   └── ERC20.sol                   # Sample ERC-20 contract
│   ├── scripts/
│   │   ├── deploy.js                   # Deployment script
│   │   ├── interact.js                 # Interaction script (synthetic)
│   │   ├── generate_summary.js         # Report generation
│   │   ├── run_evaluation.sh           # Full pipeline execution
│   │   └── run_slither.sh              # Slither invocation
│   ├── reports/                        # Generated reports
│   │   ├── interaction-report.localhost.json
│   │   ├── interaction-report.sepolia.json
│   │   ├── interaction_summary.csv
│   │   └── gas_differences.tex
│   ├── deployments/                    # Deployment metadata
│   │   ├── deployed-contracts.localhost.json
│   │   └── deployed-contracts.sepolia.json
│   ├── hardhat.config.js               # Hardhat & compiler config
│   └── package.json
├── repro-artifacts/                    # Timestamped artifacts
│   └── local/
│       └── interaction-summary-local-*.csv
├── research paper/
│   └── diagrams/
│       └── main.tex                    # LaTeX paper source
├── slither-output.json                 # Sample Slither analysis
├── README.md                           # This file
└── LICENSE                             # MIT License
```

## Key Files for Reproducibility

| File | Purpose |
|------|---------|
| `hardhat.config.js` | Compiler optimization settings (runs: 200) |
| `slither-output.json` | Static analysis baseline (Slither v0.10.0) |
| `scripts/interact.js` | Synthetic interaction script for reproducibility |
| `scripts/run_evaluation.sh` | End-to-end pipeline automation |
| `sample-hardhat/repro-artifacts/local/` | Timestamped interaction summaries |

## Commands (VS Code)

- **Web3: Insert ERC20 Template** – Insert an OpenZeppelin-based ERC-20 template.
- **Web3: Generate NatSpec Docs** – Auto-generate NatSpec comments.
- **Web3: Run Slither (Docker)** – Run static analysis on the active file.
- **Web3: Show Latest Deployment Info** – Display deployment metadata in Output panel.
- **Web3: Generate Hardhat Snippet from ABI** – Create contract interaction code.
- **Web3: Open Security Chat** – Open the @web3 security assistant.

## Performance Metrics

### Productivity
- Task completion time: 178s → 116s (**35% faster**)
- Suggestion acceptance rate: 62% → 88% (**+42% increase**)
- Manual edits per 100 LOC: 9.2 → 5.6 (**39% reduction**)

### Security
- Static-analysis findings: 12 → 7 (**40% reduction**), primarily from improved access control and immutability checks.

### Gas Efficiency
- Deployment gas (compiler opt.): 669,512 (ON) vs. 1,172,858 (OFF) = **43% savings**
- Runtime operations (transfer, mint): ~1% variance due to EVM stability; no further optimization gains.

## Known Limitations

1. **Copilot Nondeterminism:** Suggestion outputs vary across runs. Current evaluation does not quantify this variance.
2. **Slither Version Sensitivity:** Results depend on Slither version and configuration.
3. **EVM-Only:** Tested on Ethereum and Hardhat localhost only.
4. **Syntactic Analysis:** Does not detect semantic economic attacks (oracle manipulation, etc.).
5. **Synthetic Evaluation:** Scripted interactions; does not capture human learning or adoption patterns.

## Citation

If you use this project in research, please cite:

```bibtex
@inproceedings{dkhar2026web3copilot,
  title={Augmenting Web3 Development in VS Code Through Prompt Driven Copilot Extensions and Slither-Based Security Analysis},
  author={Dkhar, Josaiah Murfeal},
  booktitle={Proceedings of ICAITSC 2026},
  year={2026},
  note={Minor Revisions Accepted}
}
```

## License

MIT License – See [LICENSE](LICENSE) for details.

## Contributing

Contributions, bug reports, and feature requests are welcome. Please open an issue or pull request on [GitHub](https://github.com/JoeDkhar/SlitherAI-Toolkit).

## Support & Questions

- **GitHub Issues:** [Report bugs or request features](https://github.com/JoeDkhar/SlitherAI-Toolkit/issues)
- **Research Paper:** See `research paper/diagrams/main.tex` for full methodology.
- **Reproducibility:** All scripts and artifacts are version-controlled in `repro-artifacts/` and `reports/`.

---

**Last Updated:** January 2026  
**Maintained by:** Josaiah Murfeal Dkhar  
**Status:** Accepted to ICAITSC 2026 (Minor Revisions)