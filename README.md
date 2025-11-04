# Automated Market Maker (AMM) DEX

A fully-featured decentralized exchange built on Ethereum featuring an Automated Market Maker protocol for token swapping and liquidity provision. This project implements a complete DeFi application with Solidity smart contracts, comprehensive testing, Redux state management, and a modern React-based frontend with real-time charts and analytics.

## 🌟 Features

### Core AMM Functionality
- **Token Swapping**: Swap between DAPP and USD tokens using constant product formula (x * y = k)
- **Liquidity Provision**: Add liquidity to earn trading fees and receive proportional LP shares
- **Liquidity Removal**: Withdraw liquidity and claim proportional share of pool reserves
- **Real-time Price Charts**: Interactive ApexCharts showing price movements over time
- **Transaction History**: Complete swap history with detailed transaction information
- **Dynamic Pricing**: Automatic price discovery based on supply and demand
- **Slippage Calculations**: Real-time swap amount calculations with price impact

### Frontend Features
- **Multi-Tab Interface**: Dedicated tabs for Swap, Deposit, Withdraw, and Charts
- **Real-time Balances**: Live token balance updates
- **Transaction Status**: Loading spinners and success/failure alerts
- **Responsive Design**: Mobile-friendly Bootstrap UI components
- **MetaMask Integration**: Seamless wallet connection and transaction signing
- **Error Handling**: User-friendly error messages and transaction failure management

### Technical Features
- **Redux State Management**: Centralized state with @reduxjs/toolkit
- **React Router**: Multi-page navigation with HashRouter
- **Real-time Data**: Live blockchain data fetching and updates
- **Event Listening**: Smart contract event monitoring and processing
- **Comprehensive Testing**: Extensive test suite covering all contract functionality
- **Share-based LP System**: Proportional liquidity provider rewards system

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

The AMM consists of four main layers:

```
┌─────────────────────────────────────────────────────────────┐
│                    React Frontend Layer                     │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────┐ │
│  │    Swap     │ │   Deposit   │ │  Withdraw   │ │ Charts  │ │
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────┘ │
└─────────────────────────────────────────────────────────────┘
                                │
┌─────────────────────────────────────────────────────────────┐
│                   Redux State Layer                         │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────┐ │
│  │  Provider   │ │   Tokens    │ │     AMM     │ │ Selectors│ │
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────┘ │
└─────────────────────────────────────────────────────────────┘
                                │
┌─────────────────────────────────────────────────────────────┐
│                  Blockchain Layer                           │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐            │
│  │   DAPP      │ │     USD     │ │     AMM     │            │
│  │  Token.sol  │ │  Token.sol  │ │   AMM.sol   │            │
│  └─────────────┘ └─────────────┘ └─────────────┘            │
└─────────────────────────────────────────────────────────────┘
```

### Core Components:

1. **Smart Contracts**: ERC-20 tokens (DAPP/USD) and AMM contract
2. **Redux Store**: Centralized state management with provider, tokens, and AMM slices
3. **React Components**: Modular UI components for different functionalities
4. **Interactions Layer**: Web3 integration for blockchain communication
5. **Charts & Analytics**: Real-time data visualization with ApexCharts

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

### Using the Application

The AMM provides an intuitive interface with four main sections:

#### 1. Swapping Tokens
1. Navigate to the **Swap** tab
2. Connect your MetaMask wallet
3. Select input and output tokens (DAPP ↔ USD)
4. Enter the amount to swap
5. Review the exchange rate and output amount
6. Click **Swap** and confirm the transaction in MetaMask

#### 2. Adding Liquidity (Deposit)
1. Go to the **Deposit** tab
2. Enter amount for either DAPP or USD token
3. The interface automatically calculates the required amount for the other token
4. Click **Deposit** to add liquidity and receive LP shares

#### 3. Removing Liquidity (Withdraw)  
1. Visit the **Withdraw** tab
2. Enter the number of shares to withdraw
3. View your current share balance
4. Click **Withdraw** to remove liquidity and receive tokens back

#### 4. Viewing Analytics (Charts)
1. Access the **Charts** tab for:
   - Interactive price chart showing historical data
   - Complete transaction history table
   - Real-time swap data and trends

### Contract Interaction Examples

#### Programmatic Usage
```javascript
// Adding Liquidity
await dappToken.approve(amm.address, amount1);
await usdToken.approve(amm.address, amount2);
await amm.addLiquidity(amount1, amount2);

// Swapping Tokens
await dappToken.approve(amm.address, swapAmount);
await amm.swapToken1(swapAmount); // DAPP → USD

// Removing Liquidity
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

The React frontend provides a comprehensive user interface with multiple tabs and real-time functionality:

### Application Pages
- **Swap Tab**: Token swapping interface with real-time price calculations
- **Deposit Tab**: Liquidity provision with automatic token ratio calculations  
- **Withdraw Tab**: Liquidity removal with share-based withdrawals
- **Charts Tab**: Price charts and complete transaction history table

### Key Components:
- **App.js**: Main application with routing and blockchain data loading
- **Navigation.js**: Wallet connection and account display
- **Tabs.js**: Multi-tab navigation interface
- **Swap.js**: Token swapping with loading states and error handling
- **Deposit.js**: Liquidity provision with balance validation
- **Withdraw.js**: Liquidity removal with share calculations
- **Charts.js**: Interactive price charts and transaction table
- **Alert.js**: Transaction status notifications (success/failure/pending)
- **Loading.js**: Reusable loading spinner component

### State Management:
- **Redux Store**: Centralized state with multiple slices
- **Provider Slice**: Network and account management  
- **Tokens Slice**: Token contracts, symbols, and balances
- **AMM Slice**: Contract instance and transaction states (swapping, depositing, withdrawing)
- **Selectors**: Optimized data selection for charts and analytics

### Technologies Used:
- **React 18** with Hooks (useState, useEffect, useSelector, useDispatch)
- **Redux Toolkit** for state management
- **React Router DOM** for navigation
- **React Bootstrap** for responsive UI components
- **ApexCharts** for interactive price charts
- **Ethers.js** for Web3 blockchain interaction
- **React Blockies** for avatar generation

## 📖 API Reference

### AMM Contract Methods

#### View Functions
- `token1()`: Returns DAPP token contract address
- `token2()`: Returns USD token contract address  
- `token1Balance()`: Returns DAPP token pool balance
- `token2Balance()`: Returns USD token pool balance
- `totalShares()`: Returns total liquidity provider shares
- `shares(address user)`: Returns user's LP share amount
- `K()`: Returns the constant product (x * y = k)

#### State-Changing Functions
- `addLiquidity(uint256 token1Amount, uint256 token2Amount)`: Add liquidity to pool
- `removeLiquidity(uint256 share)`: Remove liquidity from pool
- `swapToken1(uint256 token1Amount)`: Swap DAPP tokens for USD tokens
- `swapToken2(uint256 token2Amount)`: Swap USD tokens for DAPP tokens

#### Helper Functions
- `calculateToken1Swap(uint256 token1Amount)`: Calculate USD output for DAPP input
- `calculateToken2Swap(uint256 token2Amount)`: Calculate DAPP output for USD input
- `calculateToken1Deposit(uint256 token2Amount)`: Calculate required DAPP for USD deposit
- `calculateToken2Deposit(uint256 token1Amount)`: Calculate required USD for DAPP deposit
- `calculateWithdrawAmount(uint256 share)`: Calculate token amounts for share withdrawal

### Redux Store Structure

#### Provider State
```javascript
{
  connection: Web3Provider,
  chainId: number,
  account: string
}
```

#### Tokens State  
```javascript
{
  contracts: [dappToken, usdToken],
  symbols: ['DAPP', 'USD'],
  balances: [dappBalance, usdBalance]
}
```

#### AMM State
```javascript
{
  contract: AMMContract,
  shares: userShares,
  swaps: swapHistory[],
  swapping: { isSwapping, isSuccess, transactionHash },
  depositing: { isDepositing, isSuccess, transactionHash },
  withdrawing: { isWithdrawing, isSuccess, transactionHash }
}
```

### Events
- `Swap(address user, address tokenGive, uint256 tokenGiveAmount, address tokenGet, uint256 tokenGetAmount, uint256 token1Balance, uint256 token2Balance, uint256 timestamp)`

### Chart Data Format
The application processes swap events into chart-friendly format with price history and transaction details for the ApexCharts component.

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
