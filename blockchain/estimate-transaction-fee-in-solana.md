To estimate the transaction fee on the Solana network, you can use the `@solana/web3.js` library. The fee can be estimated using the `getRecentBlockhash` and `getFeeCalculatorForBlockhash` methods. Here's how you can do it:

### Installation

First, ensure you have the `@solana/web3.js` library installed:

```sh
npm install @solana/web3.js
```

### Code Example

Here's a TypeScript example to estimate the transaction fee on Solana:

```typescript
import {
  Connection,
  clusterApiUrl,
  Transaction,
  SystemProgram,
  Keypair,
} from "@solana/web3.js";

// Function to estimate the transaction fee
async function estimateTransactionFee() {
  // Connect to the Solana cluster (testnet in this case)
  const connection = new Connection(clusterApiUrl("testnet"), "confirmed");

  // Get a recent blockhash
  const { blockhash } = await connection.getRecentBlockhash("finalized");

  // Create a dummy transaction for estimation purposes
  const dummyTransaction = new Transaction({
    recentBlockhash: blockhash,
    feePayer: Keypair.generate().publicKey,
  });

  // Add a dummy instruction to the transaction
  dummyTransaction.add(
    SystemProgram.transfer({
      fromPubkey: Keypair.generate().publicKey,
      toPubkey: Keypair.generate().publicKey,
      lamports: 1,
    })
  );

  // Get the fee calculator for the recent blockhash
  const feeCalculator = (
    await connection.getFeeCalculatorForBlockhash(blockhash)
  ).value;

  if (feeCalculator) {
    // Calculate the fee for the transaction
    const transactionFee =
      feeCalculator.lamportsPerSignature * dummyTransaction.signatures.length;
    console.log(`Estimated transaction fee: ${transactionFee} lamports`);
  } else {
    console.error(
      "Could not retrieve fee calculator for the recent blockhash."
    );
  }
}

estimateTransactionFee();
```

### Explanation

1. **Install and Import Libraries**:

   - Import necessary modules from `@solana/web3.js`.

2. **Connect to the Solana Network**:

   - Use `new Connection(clusterApiUrl('testnet'), 'confirmed')` to connect to the Solana testnet. You can change `'testnet'` to `'mainnet-beta'` or `'devnet'` depending on your needs.

3. **Get Recent Blockhash**:

   - Use `connection.getRecentBlockhash('finalized')` to get a recent blockhash.

4. **Create a Dummy Transaction**:

   - Create a dummy transaction using the recent blockhash and a dummy fee payer.
   - Add a dummy instruction to the transaction to simulate a real transaction.

5. **Get Fee Calculator**:

   - Use `connection.getFeeCalculatorForBlockhash(blockhash)` to get the fee calculator for the recent blockhash.

6. **Calculate the Transaction Fee**:
   - The transaction fee is calculated by multiplying the fee per signature (`lamportsPerSignature`) by the number of signatures required for the transaction.
   - Print the estimated transaction fee to the console.

### Running the Script

Save the code to a file, for example `estimateTransactionFee.ts`, and run it using:

```sh
ts-node estimateTransactionFee.ts
```

This script will connect to the Solana testnet, create a dummy transaction, retrieve the fee calculator for a recent blockhash, and estimate the transaction fee. The estimated fee will be printed to the console in lamports.
