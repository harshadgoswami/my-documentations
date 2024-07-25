To estimate the gas fee for an ERC-20 token transfer on Ethereum using `ethers.js`, you can follow these steps:

1. **Install ethers.js**: If you haven't already, install the `ethers.js` library.
2. **Set up a provider**: Connect to an Ethereum node using an `Infura` project ID or another provider.
3. **Create a contract instance**: Use the ERC-20 contract ABI and the token's contract address.
4. **Estimate the gas for a transfer**: Use the `estimateGas` method to get the estimated gas for the transfer.

### Installation

First, ensure you have `ethers.js` installed:

```sh
npm install ethers
```

### Code Example

Here's a TypeScript example demonstrating how to estimate the gas fee for an ERC-20 token transfer on Ethereum:

```typescript
import { ethers } from "ethers";

// Replace with your Infura project ID or other provider URL
const provider = new ethers.providers.InfuraProvider(
  "mainnet",
  "your-infura-project-id"
);

// Replace with the private key of the account making the transfer
const privateKey = "your-private-key";

// Create a wallet instance
const wallet = new ethers.Wallet(privateKey, provider);

// Replace with the ERC-20 contract address and ABI
const erc20Address = "token-contract-address";
const erc20Abi = [
  // Minimal ABI required to interact with the ERC-20 contract
  "function transfer(address to, uint amount) public returns (bool)",
];

// Create a contract instance
const erc20Contract = new ethers.Contract(erc20Address, erc20Abi, wallet);

// Replace with the recipient address and the amount to transfer (in token's smallest unit)
const recipientAddress = "recipient-address";
const amount = ethers.utils.parseUnits("1.0", 18); // 1.0 token with 18 decimals

async function estimateGasFee() {
  try {
    // Estimate the gas for the transfer
    const estimatedGas = await erc20Contract.estimateGas.transfer(
      recipientAddress,
      amount
    );

    // Get the current gas price from the provider
    const gasPrice = await provider.getGasPrice();

    // Calculate the total gas fee in Ether
    const totalGasFee = estimatedGas.mul(gasPrice);

    console.log(`Estimated Gas: ${estimatedGas.toString()}`);
    console.log(
      `Gas Price: ${ethers.utils.formatUnits(gasPrice, "gwei")} gwei`
    );
    console.log(`Total Gas Fee: ${ethers.utils.formatEther(totalGasFee)} ETH`);
  } catch (error) {
    console.error("Error estimating gas fee:", error);
  }
}

estimateGasFee();
```

### Explanation

1. **Import ethers.js**:

   - Import the `ethers` library.

2. **Set up the provider**:

   - Connect to an Ethereum node using `Infura` or another provider. Replace `'your-infura-project-id'` with your actual Infura project ID.

3. **Create a wallet instance**:

   - Create a wallet instance using the private key of the account making the transfer.

4. **Set up the ERC-20 contract instance**:

   - Define the contract address and minimal ABI required to interact with the ERC-20 `transfer` function.
   - Create a contract instance using the contract address, ABI, and wallet.

5. **Estimate gas fee**:
   - Define the recipient address and the amount to transfer in the token's smallest unit.
   - Use the `estimateGas` method on the contract instance to estimate the gas required for the transfer.
   - Fetch the current gas price from the provider.
   - Calculate the total gas fee by multiplying the estimated gas by the gas price.
   - Print the estimated gas, gas price, and total gas fee in Ether.

### Running the Script

Save the code to a file, for example, `estimateGasFee.ts`, and run it using:

```sh
ts-node estimateGasFee.ts
```

This script will connect to the Ethereum network, estimate the gas fee for an ERC-20 token transfer, and print the estimated gas, gas price, and total gas fee in Ether. Make sure to replace the placeholders with actual values before running the script.
