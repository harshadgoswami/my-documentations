To get the balance of a Solana wallet, you can use the `@solana/web3.js` library. Here's a step-by-step guide on how to do it:

### Installation

First, make sure you have the `@solana/web3.js` library installed:

```sh
npm install @solana/web3.js
```

### Code Example

Here's a TypeScript example to get the balance of a Solana wallet:

```typescript
import { Connection, PublicKey, clusterApiUrl } from "@solana/web3.js";

// Function to get the balance of a Solana wallet
async function getWalletBalance(publicKeyString: string) {
  // Connect to the Solana cluster (testnet in this case)
  const connection = new Connection(clusterApiUrl("testnet"), "confirmed");

  // Create a PublicKey object from the provided public key string
  const publicKey = new PublicKey(publicKeyString);

  // Get the balance of the wallet in lamports
  const balance = await connection.getBalance(publicKey);

  // Convert lamports to SOL (1 SOL = 1,000,000,000 lamports)
  const balanceInSol = balance / 1_000_000_000;

  console.log(`Balance for wallet ${publicKeyString}: ${balanceInSol} SOL`);
}

// Replace 'YourPublicKeyHere' with the actual public key of the wallet you want to check
const walletPublicKey = "YourPublicKeyHere";

// Get the balance of the wallet
getWalletBalance(walletPublicKey);
```

### Explanation

1. **Import Libraries**:

   - Import necessary modules from `@solana/web3.js`.

2. **Connect to the Solana Network**:

   - Use `new Connection(clusterApiUrl('testnet'), 'confirmed')` to connect to the Solana testnet. You can change `'testnet'` to `'mainnet-beta'` or `'devnet'` depending on your needs.

3. **Create a PublicKey Object**:

   - Use `new PublicKey(publicKeyString)` to create a PublicKey object from the provided public key string.

4. **Get Wallet Balance**:

   - Use `connection.getBalance(publicKey)` to get the balance of the wallet in lamports.
   - Convert lamports to SOL by dividing by 1,000,000,000 (since 1 SOL = 1,000,000,000 lamports).

5. **Print the Balance**:
   - Print the balance in SOL to the console.

### Running the Script

Replace `'YourPublicKeyHere'` with the actual public key of the wallet you want to check. Save the code to a file, for example `getWalletBalance.ts`, and run it using:

```sh
ts-node getWalletBalance.ts
```

This script will connect to the Solana testnet, retrieve the balance of the specified wallet, convert the balance from lamports to SOL, and print the balance in SOL to the console.
