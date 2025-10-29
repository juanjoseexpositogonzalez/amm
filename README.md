# Automated Market Maker (AMM) DEX

A decentralized exchange built on Ethereum featuring an Automated Market Maker protocol for token swapping and liquidity provision. This project implements a complete DeFi application with smart contracts, comprehensive testing, and a React-based frontend.

## 🌟 Features

### Core AMM Functionality
- **Token Swapping**: Swap between two ERC-20 tokens using constant product formula (x * y = k)
- **Liquidity Provision**: Add liquidity to earn trading fees and receive LP tokens
- **Liquidity Removal**: Withdraw liquidity and claim proportional share of pool reserves
- **Dynamic Pricing**: Automatic price discovery based on supply and demand
- **Slippage Protection**: Built-in mechanisms to prevent excessive slippage

### Technical Features
- **ERC-20 Token Implementation**: Custom token contracts with standard functionality
- **Comprehensive Testing**: Extensive test suite covering all contract functionality
- **React Frontend**: Modern web interface for interacting with the AMM
- **Event Logging**: Detailed transaction logging and event emission
- **Share-based LP System**: Proportional liquidity provider rewards

## 📋 Table of Contents

- [Architecture](#architecture)
- [Smart Contracts](#smart-contracts)
- [Installation](#installation)
- [Usage](#usage)
- [Testing](#testing)
- [Deployment](#deployment)
- [Frontend](#frontend)
- [API Reference](#api-reference)
- [Contributing](#contributing)

## 🏗 Architecture

The AMM consists of three main components:

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Token.sol     │    │    AMM.sol      │    │  React Frontend │
│   (ERC-20)      │◄──►│  (Core Logic)   │◄──►│   (Web3 UI)     │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Core Components:

1. **Token Contracts**: ERC-20 compliant tokens for trading pairs
2. **AMM Contract**: Core automated market maker logic
3. **Frontend**: React application for user interaction
4. **Testing Suite**: Comprehensive contract testing

## 📜 Smart Contracts

### Token.sol
Standard ERC-20 token implementation with:
- Transfer functionality
- Approval mechanism
- Balance tracking
- Event emissions

### AMM.sol
Core AMM functionality including:
- **Liquidity Management**: Add/remove liquidity with proportional share calculation
- **Token Swapping**: Constant product formula implementation
- **Price Calculations**: Dynamic pricing based on pool reserves
- **Share System**: LP token management for liquidity providers

#### Key Functions:
- `addLiquidity(uint256 _token1Amount, uint256 _token2Amount)`: Add liquidity to the pool
- `swapToken1(uint256 _token1Amount)`: Swap token1 for token2
- `swapToken2(uint256 _token2Amount)`: Swap token2 for token1
- `removeLiquidity(uint256 _share)`: Remove liquidity from pool
- `calculateToken1Swap(uint256 _token1Amount)`: Calculate swap output
- `calculateWithdrawAmount(uint256 _share)`: Calculate withdrawal amounts

## 🚀 Installation

### Prerequisites
- Node.js (v14+ recommended)
- npm or yarn
- Git

### Setup

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd amm
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Install Hardhat dependencies:**
   ```bash
   npm install --save-dev @nomicfoundation/hardhat-toolbox hardhat
   ```

## 💻 Usage

### Local Development

1. **Start Hardhat Network:**
   ```bash
   npx hardhat node
   ```
   This creates a local blockchain with pre-funded accounts.

2. **Deploy Contracts:**
   ```bash
   npx hardhat run scripts/deploy.js --network localhost
   ```

3. **Seed the AMM with liquidity:**
   ```bash
   npx hardhat run scripts/seed.js --network localhost
   ```

4. **Start the React frontend:**
   ```bash
   npm start
   ```
   Access the application at `http://localhost:3000`

### Contract Interaction

The AMM supports the following operations:

#### Adding Liquidity
```javascript
// Approve tokens first
await token1.approve(amm.address, amount1);
await token2.approve(amm.address, amount2);

// Add liquidity
await amm.addLiquidity(amount1, amount2);
```

#### Token Swapping
```javascript
// Approve token first
await token1.approve(amm.address, swapAmount);

// Swap token1 for token2
await amm.swapToken1(swapAmount);
```

#### Removing Liquidity
```javascript
// Remove liquidity shares
await amm.removeLiquidity(shareAmount);
```

## 🧪 Testing

The project includes comprehensive tests covering all functionality:

### Run Tests
```bash
# Run all tests
npx hardhat test

# Run with gas reporting
GAS_REPORT=true npx hardhat test

# Run specific test file
npx hardhat test test/AMM.js
```

### Test Coverage
- ✅ Token deployment and functionality
- ✅ AMM deployment and initialization
- ✅ Liquidity addition (single and multiple providers)
- ✅ Token swapping (both directions)
- ✅ Price impact calculations
- ✅ Liquidity removal
- ✅ Share distribution and management
- ✅ Event emission verification
- ✅ Edge case handling

### Sample Test Output
```
AMM
  ✓ has an address
  ✓ tracks token1 address
  ✓ tracks token2 address
  ✓ facilitates swaps
    Price: 1
    Investor 1 Token2 balance before swap: 0.0
    Token2 amount investor1 will receive after swap: 0.999...
    Investor 1 Token2 balance after swap: 0.999...
```

## 🚀 Deployment

### Hardhat Configuration
The project uses Hardhat for development and deployment:

```javascript
// hardhat.config.js
require("@nomicfoundation/hardhat-toolbox");

module.exports = {
  solidity: "0.8.9",
};
```

### Deploy to Networks

1. **Local Network:**
   ```bash
   npx hardhat run scripts/deploy.js --network localhost
   ```

2. **Testnet Deployment:**
   ```bash
   # Add network config to hardhat.config.js first
   npx hardhat run scripts/deploy.js --network goerli
   ```

### Contract Addresses
After deployment, update `src/config.json` with deployed addresses:
```json
{
  "31337": {
    "dapp": { "address": "0x5FbDB2315678afecb367f032d93F642f64180aa3" },
    "usd": { "address": "0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512" },
    "amm": { "address": "0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0" }
  }
}
```

## 🎨 Frontend

The React frontend provides a user-friendly interface for:
- Wallet connection via MetaMask
- Token balance display
- Swap interface
- Liquidity management
- Transaction history

### Key Components:
- **App.js**: Main application component
- **Navigation.js**: Wallet connection and navigation
- **Loading.js**: Loading states and spinners

### Technologies Used:
- React 18
- Bootstrap for styling
- Ethers.js for Web3 interaction
- React Bootstrap components

## 📖 API Reference

### AMM Contract Methods

#### View Functions
- `token1Balance()`: Returns token1 pool balance
- `token2Balance()`: Returns token2 pool balance
- `totalShares()`: Returns total liquidity provider shares
- `shares(address user)`: Returns user's share amount
- `K()`: Returns the constant product (x * y = k)

#### State-Changing Functions
- `addLiquidity(uint256 token1Amount, uint256 token2Amount)`
- `removeLiquidity(uint256 share)`
- `swapToken1(uint256 token1Amount)`
- `swapToken2(uint256 token2Amount)`

#### Helper Functions
- `calculateToken1Swap(uint256 token1Amount)`: Calculate token2 output
- `calculateToken2Swap(uint256 token2Amount)`: Calculate token1 output
- `calculateToken1Deposit(uint256 token2Amount)`: Calculate required token1 for liquidity
- `calculateToken2Deposit(uint256 token1Amount)`: Calculate required token2 for liquidity
- `calculateWithdrawAmount(uint256 share)`: Calculate withdrawal amounts

### Events
- `Swap(address user, address tokenGive, uint256 tokenGiveAmount, address tokenGet, uint256 tokenGetAmount, uint256 token1Balance, uint256 token2Balance, uint256 timestamp)`

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines
- Follow Solidity best practices
- Write comprehensive tests for new features
- Update documentation for API changes
- Use consistent code formatting

## 📄 License

This project is licensed under the ISC License.

## 🙏 Acknowledgments

- Built with Hardhat development environment
- Uses OpenZeppelin contracts as reference
- Inspired by Uniswap V2 AMM mechanics
- Created for educational purposes

## 📞 Support

For questions or support:
- Email: expositogonzalez.juanjose@gmail.com
- Create an issue in the repository

---

**⚠️ Disclaimer**: This is an educational project. Use at your own risk in production environments. Always audit smart contracts before deploying to mainnet.
