# Diploma Validator

Blockchain-based digital diploma verification system that enables institutions to verify and store academic records on the blockchain, providing tamper-proof and publicly verifiable credentials.

This project was developed as part of my thesis for the Bachelor's degree in Systems Information.

## Overview

This system consists of a **NestJS backend** with smart contract integration and a **simple frontend** that allows users to:

- Upload and verify digital diplomas against MEC (Brazilian Ministry of Education) database
- Store diploma hashes on the blockchain for immutable verification
- Verify ownership of academic records using public/private key cryptography
- Manage verified institutions and view institutional verification status

## Tech Stack

### Backend

- **NestJS** - TypeScript framework for building efficient backend applications
- **Hardhat** - Ethereum development environment for smart contract deployment
- **Ethers.js** - Library for interacting with Ethereum/Polygon blockchain
- **TypeORM** - ORM for PostgreSQL database integration
- **PostgreSQL** - Relational database for storing application data

### Smart Contracts

- **Solidity** - Smart contract programming language
- **Polygon Network** - Ethereum sidechain for low-cost transactions

### Frontend

- **Vanilla JavaScript** - No framework, pure JS
- **HTMX** - For dynamic content loading
- **Ethers.js** - For blockchain wallet integration
- **HTML5/CSS3** - Modern web standards

## Features

- **Diploma Verification**: Verify digital diplomas against MEC database
- **Blockchain Storage**: Store immutable diploma hashes on Polygon network
- **Ownership Verification**: Cryptographic proof of document ownership
- **Institution Management**: Admin panel to verify and manage institutions
- **Wallet Integration**: Create new wallets or connect existing ones
- **Transaction Management**: Build and broadcast blockchain transactions

## Project Structure

```
validator/
├── validator-backend/           # NestJS backend application
│   ├── contracts/              # Solidity smart contracts
│   │   └── Validator.sol      # Main validator contract
│   ├── ignition/               # Hardhat ignition deployment scripts
│   ├── src/                   # Source code
│   │   ├── modules/           # Feature modules
│   │   │   ├── auth/          # Authentication & JWT
│   │   │   ├── blockchain/    # Blockchain interaction
│   │   │   ├── mec-digital-diploma/ # MEC verification
│   │   │   ├── records/       # Record management
│   │   │   ├── transactions/  # Transaction handling
│   │   │   ├── users/         # User management
│   │   │   └── verified-institutions/ # Institution verification
│   │   └── main.ts            # Application entry point
│   ├── tests/                 # Unit and e2e tests
│   └── hardhat.config.ts      # Hardhat configuration
│
└── validator-frontend/         # Frontend application
    ├── assets/               # Static assets
    ├── libs/                 # External libraries (ethers, htmx)
    ├── scripts/              # JavaScript modules
    ├── index.html            # Main HTML file
    ├── index.js              # Main JavaScript entry
    └── styles.css            # Stylesheets
```

## Smart Contract

The `Validator` contract provides the following functions:

- **`storeRecord`**: Store a new diploma record on the blockchain
- **`getRecords`**: Retrieve all records for a document hash
- **`checkRecordOwner`**: Verify if a public key owns a document
- **`updateRecordStatus`**: Update the status and MEC conformity status of a record

### Contract Data Structures

```solidity
struct Record {
    bytes32 hash;                          // Document hash
    bytes32 publicKey;                     // Owner's public key
    bool exists;                           // Record existence flag
    Status status;                         // VALID, INVALID, CANCELLED, NULL
    MecConformityStatus mecConformityStatus; // MEC validation status
    uint256 createdAt;                     // Creation timestamp
    uint256 statusUpdatedAt;               // Last status update
    uint256 mecConformityStatusUpdatedAt; # Last MEC status update
}
```

## Getting Started

### Prerequisites

- Node.js (v20+)
- PostgreSQL database
- Polygon or Ethereum wallet (for testing)
- RPC provider URL (for blockchain connection)

### Manual Installation

1. **Clone the repository**

```bash
git clone https://github.com/Luis-H-Baroni/diploma-validator
cd diploma-validator
```

2. **Install backend dependencies**

```bash
cd validator-backend
npm install
```

3. **Setup environment variables**

```bash
cp .env.example .env
```

Edit `.env` with your configuration:

```env
CONTRACT_ADDRESS="<your-deployed-contract-address>"
CONTRACT_METHODS="storeRecord,getRecords,updateRecordStatus"
RPC_PROVIDER_URL="https://rpc-amoy.polygon.technology/"
WALLET_PRIVATE_KEY="<your-private-key>"
DATABASE_URL="postgresql://user:password@localhost:5432/validator"
```

4. **Run with Docker Compose**

```bash
docker-compose up -d
```

5. **Compile smart contracts**

```bash
npm run contract:compile
```

6. **Deploy smart contract** (local network)

```bash
# Start local Hardhat node in one terminal
npm run node:local

# Deploy contract in another terminal
npm run contract:deploy:local
```

For Polygon testnet or mainnet:

```bash
npm run contract:deploy:testnet  # For Amoy testnet
npm run contract:deploy:mainnet # For Polygon mainnet
```

7. **Run backend**

```bash
# Development mode
npm run start:dev

# Production mode
npm run build
npm run start:prod
```

The backend will run on `http://localhost:3000`

8. **Setup frontend**

The frontend is served statically by the backend. Once the backend is running, access:

```
http://localhost:3000/
```

## Usage

### Verifying a Diploma

1. Open the application in your browser
2. Create a new wallet or connect an existing one
3. Upload your diploma PDF file
4. Click "Verificar" to verify against MEC database
5. If valid, you can store the hash on the blockchain

### Storing a Diploma on Blockchain

1. After verification, click "Registrar na Blockchain"
2. Review the transaction details
3. Sign the transaction with your wallet
4. Wait for blockchain confirmation

### Verifying Ownership

1. Navigate to the verification section
2. Enter a document hash
3. Click "Verificar Propriedade"
4. The system will check if your wallet owns the document

### Admin Panel

Administrators can access the admin panel at `/admin-panel` to:

- View all verified institutions
- Approve/reject institution verification requests
- Monitor blockchain transactions

## Development

### Running Tests

```bash
npm run test:unit
npm run test:watch
npm run test:e2e
npm run test:unit -- --coverage
```

### Linting & Formatting

```bash
npm run lint
npm run format
```

### Building for Production

```bash
npm run build
```

## Environment Variables

| Variable             | Description                          | Required |
| -------------------- | ------------------------------------ | -------- |
| `CONTRACT_ADDRESS`   | Deployed smart contract address      | Yes      |
| `CONTRACT_METHODS`   | Comma-separated contract methods     | Yes      |
| `RPC_PROVIDER_URL`   | Polygon/Ethereum RPC URL             | Yes      |
| `WALLET_PRIVATE_KEY` | Private key for signing transactions | Yes      |
| `DATABASE_URL`       | PostgreSQL connection string         | Yes      |
| `PORT`               | Server port (default: 3000)          | No       |

## API Endpoints

### Authentication

- `POST /auth/register` - Register a new user
- `POST /auth/login` - Login and get JWT token

### Records

- `POST /records/get` - Get records by document hash
- `POST /records/verify-ownership` - Verify document ownership

### Transactions

- `POST /transactions/build` - Build a contract transaction
- `POST /transactions/broadcast` - Broadcast a signed transaction

### MEC Digital Diploma

- `POST /mec-digital-diploma/verify` - Verify diploma against MEC

### Verified Institutions

- `GET /verified-institutions` - List all verified institutions
- `POST /verified-institutions` - Create new institution request

## Deployment

1. Build the application:

```bash
npm run build
```

2. Deploy to your hosting platform (e.g., Vercel, AWS, DigitalOcean)

3. Ensure environment variables are set in production

### Smart Contract Deployment

Deploy to Polygon mainnet using Hardhat:

```bash
npm run contract:deploy:mainnet
```

Update the `CONTRACT_ADDRESS` in your `.env` file with the deployed contract address.
