# Swerve.fi-Incident

## Overview

This repository contains a smart contract test that simulates a governance attack on Swerve.fi. The test explores how an attacker might exploit the voting system to gain control over critical contracts and withdraw funds. This analysis stems from a real incident that occurred on **March 17th**, where an attacker attempted to seize control of the YPoolDelegator contract, a critical part of Swerve Finance.

## Incident Summary

On **March 17th**, an external actor initiated two governance proposals aimed at gaining ownership of the Swerve Finance YPoolDelegator contract via the Swerve Finance voting system. The attacker’s objective was to withdraw admin fees, approximately worth **$123K**, by executing the `withdraw_admin_fees()` function. Additionally, the attacker could pause the contract using `kill_me()` and unpause it with `unkill_me()`. 

However, the attack likely failed because the attacker did not control enough voting power to pass the proposals at the time of the exploit.

### Key Details:
- **Proposal IDs**: 
  - Proposal 5: [Commit Transfer Ownership](https://etherscan.io/tx/0x4c612aeaddb9f534bcb500e8c4ef8d6171efda9841bce716349cc1fb4275a2fa)
  - Proposal 6: [Apply Transfer Ownership](https://etherscan.io/tx/0x23f76745663d347d9af2c3ea572db3e00789a73ecac837cfc95a92759edbb73b)
- **Exploited Contract**: [YPoolDelegator](https://etherscan.io/address/0x329239599afb305da0a2ec69c58f8a6697f9f88d)
- **Voting Contract**: [Swerve Voter](https://etherscan.io/address/0xdff7beb0cbf54d6553e4702ae0ffa60718822478)
- **Voting Token**: [veSWRV](https://etherscan.io/address/0xe5e7DdADD563018b0E692C1524b60b754FBD7f02)
- **Attacker Address**: [0x93948ca22517421424868d021A5e987036f38a4E](https://etherscan.io/address/0x93948ca22517421424868d021A5e987036f38a4E)

## Exploit Analysis

The attack aimed to exploit the governance system by passing two proposals to transfer ownership of the YPoolDelegator contract to the attacker. Here’s how the attacker could manipulate the voting system based on token holdings:

1. **Proposal 5**: The attacker planned to commit the ownership transfer using `commit_transfer_ownership`.
2. **Proposal 6**: The attacker would finalize the transfer using `apply_transfer_ownership`.

Once the attacker gained ownership of the YPoolDelegator contract, they would be able to withdraw admin fees of approximately **$123K** and potentially kill or unpause the contract.

### Token Requirements for Voting:

- The attacker needed to hold around **836,000 SWRC tokens** to pass Proposal 5 through the first validation check.
- Holding **~40,000 tokens** would have allowed passing Proposal 5 via the second validation, or **~490,000 tokens** for the third validation.

At the time of the attack, the attacker held **~347,670 tokens**, which was insufficient to meet any of the validation requirements.

## Simulation Results

In this test simulation, various scenarios are explored to replicate and analyze the real incident:
- **testGovAttack**: Simulates the attacker trying to seize control of the YPoolDelegator contract and withdraw admin fees.
- **testSimulateAttackerBuyTokens**: Simulates an attacker's attempt to acquire tokens from an exchange and influence governance proposals.
- **testSimulateAttacker**: Runs through the scenario where the attacker votes on and attempts to execute proposals while tracking their balance in stablecoins like DAI, USDC, TUSD, and USDT.

### Simulation Results for the Admin Fees:
- DAI: **35,650 DAI**
- USDC: **39,499 USDC**
- USDT: **30,390 USDT**
- TUSD: **17,952 TUSD**

## Code Structure

- **`GovAttackTest.sol`**: The main smart contract that simulates the governance attack. Key tests include:
  - `testGovAttack()`: Simulates an attack to withdraw admin fees and take over the YPoolDelegator contract.
  - `testSimulateAttackerBuyTokens()`: Tests the scenario where the attacker buys tokens from OKX to vote on proposals.
  - `testSimulateAttacker()`: Simulates the attacker's voting and balance outcomes.

- **Dependencies**: The test requires Foundry to run, along with libraries like `forge-std`.

## How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/swerve-fi-incident-simulation.git
   ```

2. Install Foundry by following the instructions [here](https://book.getfoundry.sh/getting-started/installation.html).

3. Compile the test:
   ```bash
   forge build
   ```

4. Run the test:
   ```bash
   forge test
   ```

