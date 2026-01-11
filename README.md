# Arc-TEstnet
Deploying a Smart Contract on Arc Testnet with Foundry on Ubuntu

This guide walks you through deploying a simple Solidity smart contract to the Arc Testnet (Circle's EVM-compatible blockchain) using Foundry on Ubuntu.
Arc Testnet uses USDC as the native gas token and is designed for testing compliant, enterprise-ready DeFi applications.

Prerequisites
- Ubuntu 22.04 or 24.04 (LTS recommended for stability).
- Basic terminal knowledge.
- A testnet wallet with test USDC (from the faucet).
- And your wallet details:
- Private key
- Sender address:
- RPC URL: wss://arc-testnet.drpc.org

**Step 1: Install DependenciesUpdate your system and install required tools:bash**

sudo apt update && sudo apt upgrade -y
sudo apt install -y git curl build-essential nano

**#Install Rust (Foundry dependency):bash**

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
source "$HOME/.cargo/env"
rustup update stable

**#Step 2: Install Foundrybash**

curl -L https://foundry.paradigm.xyz | bash
source ~/.bashrc
foundryup

# Verify installation
forge --version  # Output: forge 0.2.0 (or similar)
cast --version

**#Step 3: Create Projectbash**

mkdir arc-testnet-demo && cd arc-testnet-demo
forge init

**#Step 4: Configure .env (Sensitive – Do Not Commit)Create .env:bash**

nano .env

#Paste:

PRIVATE_KEY=
ARC_RPC_URL=wss://arc-testnet.drpc.org

#Save (Ctrl+O, Enter, Ctrl+X). Secure:bash

chmod 600 .env
echo ".env" >> .gitignore

**#Step 5: Create Contract (src/Greeting.sol)bash**
mkdir -p src
nano src/Greeting.sol

**#Paste:**

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Greeting {
    string public message = "Hello from Ubuntu on Arc Testnet - new key 2026";

    function setMessage(string memory _newMessage) public {
        message = _newMessage;
    }
}

Save and exit.

**Step 6: Create Deployment Script (script/DeployGreeting.s.sol)bash**

mkdir -p script
nano script/DeployGreeting.s.sol

**Paste:**

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "forge-std/Script.sol";
import "../src/Greeting.sol";

contract DeployGreeting is Script {
    function run() external {
        vm.startBroadcast();
        new Greeting();
        vm.stopBroadcast();
    }
}

Save and exit.

**Step 7: Install forge-std Librarybash**

forge install foundry-rs/forge-std --no-git
forge build  # Verify compilation succeeds

Step 8: Fund Wallet with Test USDC.
Visit https://faucet.arc.network/ (connect MetaMask with Arc Testnet added).
Network details: Chain ID (e.g., 5042002), 
RPC: wss://arc-testnet.drpc.org,
Currency: USDC.
Request test USDC to your address

**#Step 9: Deploybash**

source .env
forge script script/DeployGreeting.s.sol:DeployGreeting \
  --rpc-url $ARC_RPC_URL \
  --private-key $PRIVATE_KEY \
  --sender your address \
  --broadcast \
  --verify

#Output includes contract address and tx hash. Check on explorer: https://testnet.arcscan.app/.Step 10: InteractRead message:bash

cast call CONTRACT_ADDRESS "message()(string)" --rpc-url $ARC_RPC_URL

**#Update message:bash**

cast send CONTRACT_ADDRESS "setMessage(string)" "New message!" --rpc-url $ARC_RPC_URL --private-key $PRIVATE_KEY

TroubleshootingRPC timeout: Switch to HTTPS RPC in .env and reload source .env.
Insufficient funds: Ensure test USDC in wallet.
Verification fails: Arc may not fully support it yet – ignore for testnet.

This setup is ready for more advanced contracts. Fork this guide on GitHub and contribute!

Brief Bullish Overview of Arc ProjectArc, by Circle, is a game-changing L1 blockchain with USDC as native gas token, blending compliance, scalability, and DeFi innovation.
Launched in testnet 2025, it offers enterprise-grade security for institutions while being EVM-compatible for seamless developer adoption. 
Bullish catalysts: Circle's $50B USDC dominance, regulatory edge in Web3 (post-MiCA), partnerships with major banks, and focus on real-world assets (RWAs). 
With mainnet 2026, Arc could capture 20-30% of stablecoin DeFi volume, driving 5-10x growth in TVL.









