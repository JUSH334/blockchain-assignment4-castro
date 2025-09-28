# DIDLab ERC-20 DApp - Team 08

## Team Configuration

| Parameter | Value |
|-----------|-------|
| **Team Number** | 08 |
| **RPC URL** | https://hh-08.didlab.org |
| **Chain ID** | 31344 (0x7a70 in hex) |
| **Token Address** | 0xb7f8bc63bbcad18155201308c8f3540b07f84f5e |

## Project Overview

This minimal DApp provides a web interface for interacting with ERC-20 tokens on the DIDLab Team 08 blockchain. It enables MetaMask wallet connection, token balance viewing, and token transfers with full transaction tracking.

## Features

- MetaMask wallet connection with automatic network switching
- ERC-20 token metadata display (name, symbol, decimals)
- Real-time balance updates
- Token transfer functionality with gas tracking
- Automatic balance refresh on Transfer events
- Add token to MetaMask with one click
- Persistent storage of token address and team selection

## Prerequisites

- **MetaMask** browser extension installed
- **Web Browser** (Chrome, Firefox, Brave, or Edge)
- **Python 3** or **Node.js** (for local web server)
- **Token Contract** deployed to Team 08 chain

## Installation

1. Clone or download the repository:
```bash
git clone [repository-url]
cd didlab-dapp
```

2. Ensure you have the following files:
- `interface.html` - Main DApp interface
- `README.md` - This documentation

## Running the DApp

### Option 1: Python Web Server (Recommended)

```bash
# Navigate to the project directory
cd /path/to/didlab-dapp

# Start Python 3 web server
python3 -m http.server 8000

# For Python 2 (if Python 3 not available)
python -m SimpleHTTPServer 8000
```

Open your browser and navigate to:
```
http://localhost:8000/interface.html
```

### Option 2: Node.js HTTP Server

```bash
# Install http-server globally (first time only)
npm install -g http-server

# Navigate to project directory
cd /path/to/didlab-dapp

# Start the server
http-server -p 8000

# Or use npx without installing
npx http-server -p 8000
```

Open your browser and navigate to:
```
http://localhost:8000/interface.html
```

### Option 3: VS Code Live Server

1. Open the project folder in VS Code
2. Install the "Live Server" extension
3. Right-click on `interface.html`
4. Select "Open with Live Server"

The browser will automatically open with the correct URL.

## Usage Instructions

### 1. Connect Wallet

1. Select **"08"** from the Team dropdown menu
2. Click **"1) Connect & Switch Network"** button
3. Approve the MetaMask connection request
4. MetaMask will automatically add/switch to Team 08 network

### 2. Load Token

1. Enter your deployed ERC-20 token contract address
2. Click **"2) Load Token"** button
3. Token metadata will display: name, symbol, decimals
4. Your current balance will appear automatically

### 3. Transfer Tokens

1. Enter recipient address in the "Recipient 0x..." field
2. Enter transfer amount in human-readable units (e.g., "10.5")
3. Click **"Send"** button
4. Confirm transaction in MetaMask
5. View transaction hash and confirmation details in the log

### 4. Additional Features

- **Refresh Balance**: Click to manually update your token balance
- **Add Token to MetaMask**: Click to add the token to your MetaMask wallet

### 🔧 Troubleshooting

**MetaMask Not Detected**
- Ensure MetaMask is installed and unlocked
- Refresh the page after unlocking MetaMask

**Connection Failed**
- Check Team 08 RPC is accessible: https://hh-08.didlab.org
- Try manually adding the network to MetaMask
- Verify Chain ID is 31344

**Transaction Failures**
- Ensure sufficient token balance
- Check you have ETH for gas fees
- Verify recipient address is valid

**Balance Not Updating**
- Click "Refresh Balance" button
- Check if Transfer events are being emitted correctly
- Verify you're connected to the correct network

### Local Development Fallback

If the team RPC is unavailable, modify `interface.html`:

```javascript
// Change line for team 08:
"08": { id: 31344, name: "DIDLab Team 08", rpc: "http://localhost:8545" },
```

Then run a local Hardhat node:
```bash
npx hardhat node --config hardhat.config.js
```

## File Structure

```
didlab-dapp/
├── interface.html    # Main DApp interface
├── README.md         # This documentation
└── screenshots/      # Assignment screenshots
    ├── connected.png
    ├── token-loaded.png
    └── transfer-confirmation.png
```

## Technical Stack

- **Frontend**: Vanilla JavaScript with ES6 modules
- **Web3 Library**: Viem v2.37.5 (via CDN)
- **Styling**: Custom CSS with dark theme
- **Storage**: localStorage for persistence
- **Blockchain**: Team 08 DIDLab chain

## Security Considerations

- All address inputs are validated using checksums
- Private keys never handled by the application
- All transactions signed through MetaMask
- Input sanitization for transfer amounts

## Support

For issues or questions:
- Check the team Discord/Slack channel
- Verify RPC endpoint status with team members
- Ensure local environment matches team configuration

---

*Developed for DIDLab Assignment 4 - Team 08*


# Short Note - Safety checks/UX touches added

Implementation Notes - Team 08 DApp
Extra Safety Checks & UX Enhancements
Our implementation includes several safety and usability improvements beyond the base requirements:
Input Validation: All addresses are validated using Viem's getAddress function, which checks for valid checksums and prevents sending tokens to malformed addresses. Amount inputs are sanitized to prevent negative values and properly handle decimal precision.
Transaction Feedback: Users receive real-time status updates during transfers - a yellow "Submitted" message appears immediately with the transaction hash, followed by a green "Mined" confirmation showing block number and gas used. This dual-stage feedback prevents user confusion during pending transactions.
Persistent Storage: Token addresses are automatically saved to localStorage, eliminating the need to re-enter contract addresses after page refreshes. The team selection also persists, streamlining the development workflow.
Auto-refresh on Events: The balance automatically updates when Transfer events involving the connected account are detected, using Viem's watchContractEvent. This ensures users always see current balances without manual refreshing.
Issues Encountered & Resolutions
MetaMask File Protocol Issue: Initially attempted to open the HTML file directly (file:///), which caused MetaMask content script errors ("SES_UNCAUGHT_EXCEPTION"). Solution: Served the application through a Python HTTP server (python3 -m http.server 8000) to use proper http://localhost:8000 URLs, enabling proper MetaMask injection.
RPC Connection Failures: The team RPC endpoint (https://hh-08.didlab.org) experienced intermittent availability. Solution: Implemented fallback configuration to allow quick switching to local Hardhat node (http://localhost:8545) during development, ensuring continuous workflow even when remote infrastructure was unavailable.
Browser Compatibility: Firefox's strict security policies initially blocked MetaMask interactions. Solution: Used Chrome for primary development and testing, while ensuring cross-browser compatibility in the final implementation.
These enhancements and solutions resulted in a robust, user-friendly DApp that handles edge cases gracefully while maintaining a smooth development experience.
