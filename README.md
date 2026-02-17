# TEEFi

<div align="center">

[![Docker](https://img.shields.io/badge/Docker-Ready-blue?logo=docker)](https://docker.com)
[![Python](https://img.shields.io/badge/Python-3.12+-yellow?logo=python)](https://python.org)
[![Go](https://img.shields.io/badge/Go-1.23+-blue?logo=go)](https://golang.org)
[![Intel SGX](https://img.shields.io/badge/Intel%20SGX-TEE-green)](https://software.intel.com/content/www/us/en/develop/topics/software-guard-extensions.html)
[![MCP](https://img.shields.io/badge/MCP-Compatible-green)](https://modelcontextprotocol.io)
[![FastAPI](https://img.shields.io/badge/FastAPI-Framework-teal?logo=fastapi)](https://fastapi.tiangolo.com)
[![Ethereum](https://img.shields.io/badge/Ethereum-Compatible-blue?logo=ethereum)](https://ethereum.org)
[![Solidity](https://img.shields.io/badge/Solidity-0.8+-gray?logo=solidity)](https://soliditylang.org)
[![Hardhat](https://img.shields.io/badge/Hardhat-Toolkit-yellow?logo=hardhat)](https://hardhat.org)
[![Aave](https://img.shields.io/badge/Aave-Integration-purple)](https://aave.com)

</div>

TEEFi is a comprehensive platform for secure and verifiable financial transactions leveraging Trusted Execution Environment (TEE) technology, specifically Intel SGX. It integrates AI-powered financial agents with blockchain DeFi protocols to provide investment advice, yield optimization, and secure transaction execution. The platform consists of multiple components working together to ensure autonomy, security, and efficiency in decentralized finance operations.

> **Note:** Reference implementation, not production-ready.

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Project Structure](#project-structure)
- [Requirements](#requirements)
- [Setup](#setup)
- [Testing](#testing)
- [License](#license)
- [Contributing](#contributing)
- [Support](#support)

## Overview

TEEFi combines cutting-edge technologies to create a secure DeFi ecosystem:

### Core Components

#### Financial Agent (`financial-agent/`)
A Python-based service that provides AI-powered investment advice and yield optimization recommendations. It analyzes Aave protocol market data through Model Context Protocol (MCP) integration and uses large language models to deliver personalized financial insights.

**Key Features:**
- Personalized investment recommendations
- Yield optimization analysis
- Direct Aave protocol integration
- LLM-powered market analysis

#### Financial Agent Backend (`financial-agent-backend/`)
A Go-based backend service responsible for executing financial transactions in a secure manner using Intel SGX TEE. It handles deposits, withdrawals, and yield generation while ensuring all operations are TEE-verified.

**Key Features:**
- REST API for financial operations
- TEE/SGX integration for secure transaction signing
- On-chain operations (deposit, withdraw, claim)
- Block scanning and event processing

#### TEEFi EVM Contracts (`teefi-evm-contracts/`)
Smart contracts deployed on Ethereum-compatible networks that enable secure financial transactions. The contracts integrate with lending protocols like Aave and manage user wallets through TEE-verified transactions.

**Key Features:**
- Modular, upgradeable contract architecture
- TEE attestation verification
- Integration with lending protocols
- Trust-managed wallet proxies

## Architecture

TEEFi follows a layered architecture designed for security, modularity, and scalability:

```
┌───────────────────┐
│  Financial Agent  │  ← AI Analysis & Recommendations
│  (Python/FastAPI) │
└───────────────────┘
         │
         ▼
┌───────────────────┐
│ Financial Backend │  ← Transaction Creation
│   (Go/Gin + TEE)  │
└───────────────────┘
         │
         ▼
┌───────────────────┐
│   EVM Contracts   │  ← On-Chain Execution
│    (Solidity)     │
└───────────────────┘
```

### Data Flow

1. **User Request**: Financial Agent analyzes market data and provides investment advice
2. **Transaction Preparation**: Backend validates requests and prepares TEE-signed transactions
3. **On-Chain Execution**: Smart contracts verify TEE attestations and execute operations
4. **Yield Generation**: Funds are deployed to lending protocols for yield optimization

### Security Model

- **TEE Integration**: All sensitive operations occur within Intel SGX enclaves
- **Attestation Verification**: Smart contracts verify TEE hardware attestation before execution
- **AccessControl**: Smart contracts provide high-level access rights verification


## Project Structure

```
TEEFi/
├── financial-agent/              # AI-powered financial advice service
│   ├── src/financial_agent/      # Python application code
│   ├── tests/                    # Unit and integration tests
│   ├── Dockerfile                # Containerization
│   ├── docker-compose.yml        # Production deployment
│   ├── pyproject.toml            # Python dependencies
│   └── README.md                 # Component documentation
│
├── financial-agent-backend/      # Go backend with TEE integration
│   ├── cmd/                      # Application entry point
│   ├── core/                     # Business logic
│   ├── config/                   # Configuration management
│   ├── tests/                    # Integration tests
│   ├── graminize_scripts/        # SGX deployment scripts
│   ├── Dockerfile                # Containerization
│   ├── go.mod                    # Go dependencies
│   └── README.md                 # Component documentation
│
└── teefi-evm-contracts/          # Smart contracts
    ├── contracts/                # Solidity source code
    ├── test/                     # Contract tests
    ├── ignition/                 # Deployment scripts
    ├── hardhat.config.js         # Hardhat configuration
    ├── package.json              # Node dependencies
    └── README.md                 # Component documentation
```

## Requirements

### System Requirements
- **Operating System**: Linux/Windows/macOS
- **Docker**: For containerized deployment
- **Intel SGX**: For TEE features (optional for development)

### Component-Specific Requirements

#### Financial Agent
- Python 3.12+
- UV package manager
- LLM API access (Google/Together AI)
- Aave MCP server access

#### Financial Agent Backend
- Go 1.23+
- Ethereum RPC endpoint access
- Intel SGX (for production TEE features)

#### TEEFi EVM Contracts
- Node.js 18+
- npm or yarn
- Hardhat
- Ethereum RPC endpoint access

## Setup

### Prerequisites
1. Install Docker and Docker Compose
2. Set up Ethereum RPC endpoint (e.g., Infura, Alchemy)
3. Obtain necessary API keys (LLM providers, RPC access)

### Quick Start

1. **Clone the repository**:
   ```bash
   git clone <https://github.com/Xyber-Labs/TEEFi>
   cd TEEFi
   git submodule update --init --recursive
   ```

2. **Set up each component**:

   **Financial Agent**:
   ```bash
   cd financial-agent
   uv sync
   cp env.example .env
   # Configure .env with API keys and MCP server URL
   ```

   **Financial Agent Backend**:
   ```bash
   cd ../financial-agent-backend
   go mod download
   cp config.yaml.example config.yaml
   # Configure config.yaml with RPC URL and contract addresses
   ```

   **TEEFi EVM Contracts**:
   ```bash
   cd ../teefi-evm-contracts
   npm install
   cp .env.example .env
   # Configure .env with RPC URL and deployment keys
   ```

3. **Deploy contracts**:
   ```bash
   cd teefi-evm-contracts
   npx hardhat compile
   npx hardhat ignition deploy ignition/modules/TeeFiModule.js --parameters <params> --network <network>
   ```

4. **Start services**:
   ```bash
   # Start backend
   cd ../financial-agent-backend
   go run cmd/main.go

   # Start financial agent
   cd ../financial-agent
   uvicorn financial_agent.__main__:app --reload --host 0.0.0.0 --port 8000
   ```

### Docker Deployment

For production deployment with Docker:

```bash
# Build and start all services
docker-compose -f financial-agent/docker-compose.yml up --build
docker-compose -f financial-agent-backend/docker-compose.yml up --build
```

For TEE-enabled deployment:
```bash
cd financial-agent-backend/graminize_scripts
./0_generate_gram_key.sh
./1_build_base_docker_image.sh
./2_build_prod_docker_image.sh
./3_graminize_image.sh
./5_deploy_gram_image.sh
```

## Testing

### Financial Agent
```bash
cd financial-agent
uv run pytest tests/
```

### Financial Agent Backend
```bash
cd financial-agent-backend
go test ./...
make coverage  # For coverage report
```

### TeeFi EVM Contracts
```bash
cd teefi-evm-contracts
npx hardhat test
npx hardhat coverage  # For coverage report
```

### Integration Testing
```bash
# Run all component tests
cd financial-agent-backend/tests
go test -tags=integration ./...
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details

Copyright (c) 2026 Xyber.inc

## Contributing

We welcome contributions to TEEFi! Please follow these steps:

1. Fork the individual sub component
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines
- Follow existing code style and conventions
- Add tests for new features
- Update documentation as needed
- Ensure all tests pass before submitting PR

## Support

For questions, issues, or contributions:

- **Issues**: [GitHub Issues](https://github.com/xyber-labs/TEEFi/issues)
- **Discussions**: [GitHub Discussions](https://github.com/xyber-labs/TEEFi/discussions)
- **Documentation**: See individual component READMEs for detailed guides

---
