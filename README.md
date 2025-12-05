# **SlitherAI Toolkit**

The SlitherAI Toolkit is a comprehensive development and analysis environment that integrates a VS Code extension with a structured Hardhat project to support smart-contract development, security assessment, gas-usage evaluation, and reproducible deployment workflows on Ethereum networks.

## **Introduction**

This toolkit streamlines the Web3 development process by unifying contract authoring, static security analysis, gas-consumption measurement, and interaction monitoring within a single workflow. Developed as part of a research effort examining how development processes can be improved for smart-contract engineering, it includes a VS Code extension, a full Hardhat environment, deployment and interaction scripts, Slither analysis pipelines, statistical notebooks, and reproducible artifacts.

The integrated design provides a complete foundation for studying, refining, and validating development practices across different contract versions, compiler settings, and deployment conditions. It supports the full cycle of activities—ranging from contract creation and deployment to performance evaluation and vulnerability detection—while ensuring consistency and transparency throughout.

## **Overview**

The toolkit supports end-to-end Ethereum development by providing automated Slither-based security scanning, detailed gas-usage reporting, contract interaction analysis, deployment to both localhost and the Sepolia test network, and structured statistical analysis through Jupyter notebooks. All components operate together to provide a reliable and repeatable environment for evaluating contract behavior under controlled scenarios.

---

# **Project Structure**

```
SlitherAI-Toolkit/
├── extension/                    # VS Code Extension
│   ├── src/
│   │   └── extension.ts         # Extension entry point
│   ├── package.json             # Manifest and dependencies
│   └── tsconfig.json            # TypeScript configuration
│
├── sample-hardhat/              # Hardhat Project
│   ├── contracts/
│   │   └── ERC20.sol            # Sample ERC20 token
│   ├── scripts/
│   │   ├── deploy.js            # Deployment script
│   │   ├── interact.js          # Interaction script
│   │   ├── run_slither.sh       # Slither execution script
│   │   └── run_evaluation.sh    # Evaluation workflow
│   ├── test/
│   │   └── testERC20.js         # Contract tests
│   ├── analysis/
│   │   └── compute_stats.ipynb  # Statistical notebook
│   ├── reports/                 # Generated reports
│   ├── deployments/             # Deployment artifacts
│   ├── hardhat.config.js        # Hardhat configuration
│   ├── package.json             # Project dependencies
│   └── .env.example             # Environment template
│
├── diagrams/                    # Architectural diagrams
├── .gitignore
└── README.md
```

---

# **Features**

## **VS Code Extension**

The extension provides enhanced development tooling within VS Code, including command-driven workflows and structured feedback for smart-contract preparation and analysis.

## **Hardhat Integration**

The Hardhat project includes a sample ERC20 contract, deployment workflows for both localhost and Sepolia testnet environments, gas reporting capabilities, and extensive interaction logging.

## **Analytics and Reporting**

The toolkit offers reproducible gas-comparison outputs, interaction reports, CSV datasets for external analysis, and notebooks for statistical inspection of performance and behavior.

---

# **Quick Start**

## **Prerequisites**

* Node.js 14+
* npm or yarn
* Git
* VS Code

## **Clone the Repository**

```bash
git clone https://github.com/your/repo.git
cd SlitherAI-Toolkit
```

## **Install Extension Dependencies**

```bash
cd extension
npm install
cd ..
```

## **Install Hardhat Dependencies**

```bash
cd sample-hardhat
npm install
cd ..
```

## **Environment Setup**

```bash
cd sample-hardhat
cp .env.example .env
```

Update `.env` with a valid Sepolia RPC endpoint and a test private key.

---

# **Usage**

## **Deploy Contracts**

Localhost:

```bash
cd sample-hardhat
npx hardhat run scripts/deploy.js --network localhost
```

Sepolia:

```bash
cd sample-hardhat
npx hardhat run scripts/deploy.js --network sepolia
```

## **Interact With the Contract**

```bash
cd sample-hardhat
npx hardhat run scripts/interact.js --network localhost
```

## **Run Tests**

```bash
cd sample-hardhat
npx hardhat test
```

## **Run Slither Security Analysis**

```bash
cd sample-hardhat
bash scripts/run_slither.sh
```

## **Gas Analysis**

View gas-usage output from deployment logs or in the generated reports.

## **Statistical Analysis**

Open the Jupyter notebooks located in:

* `analysis/compute_stats.ipynb`
* `analyze_gas.ipynb`
* `analyze_interaction_reports.ipynb`

---

# **Configuration**

## **Hardhat Network Settings**

Modify `hardhat.config.js` to adjust compiler settings, gas reporting, optimizer configuration, and network parameters.

## **Solidity Version**

The default compiler version is 0.8.20 and can be updated in the config file.

---

# **Deployment Reports**

Deployment and analysis artifacts are automatically generated, including:

* Contract addresses
* Interaction histories
* Gas comparison datasets

---

# **File Descriptions**

### Sample Contract

`ERC20.sol` is a standard ERC20 implementation used for testing and benchmarking.

### Scripts

Deployment, interaction, evaluation, and network-check scripts support automated workflows.

### Analysis Notebooks

Notebooks provide statistical summaries and gas-usage insights.

---

# **Security Notes**

Keep `.env` files private, use test networks only, inspect contracts before mainnet deployment, and ensure adequate balances and valid keys before transactions.

---

# **Available Networks**

| Network   | RPC URL                                                        | Chain ID | Purpose            |
| --------- | -------------------------------------------------------------- | -------: | ------------------ |
| Localhost | [http://127.0.0.1:8545](http://127.0.0.1:8545)                 |    31337 | Development        |
| Sepolia   | [https://ethereum-sepolia-rpc…](https://ethereum-sepolia-rpc…) | 11155111 | Testnet deployment |

---

# **Troubleshooting**

* Ensure the gas reporter is enabled in the Hardhat configuration.
* Verify private keys and RPC endpoints when transactions fail.
* Confirm the Hardhat node is active when deploying locally.
* Recompile contracts if deployment errors occur.

---


