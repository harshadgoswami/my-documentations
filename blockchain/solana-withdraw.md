To withdraw SOL (Solana's native cryptocurrency) from a Solana address to another address, you need to construct and sign a transaction using the `@solana/web3.js` library in Node.js. Here's how you can do it:

1. **Install Required Packages:**
   Make sure you have the `@solana/web3.js` package installed. You can install it via npm if you haven't already:

   ```
   npm install @solana/web3.js
   ```

2. **Write Withdraw Code:**
   Below is a Node.js script (`withdrawSOL.js`) that demonstrates how to withdraw SOL from one Solana address to another:

   ```javascript
   const {
     Connection,
     PublicKey,
     Transaction,
     SystemProgram,
     TransactionInstruction,
     sendAndConfirmTransaction,
   } = require("@solana/web3.js");

   async function withdrawSOL(fromPrivateKey, toPublicKey, amount) {
     try {
       // Connect to Solana cluster
       const connection = new Connection("https://api.mainnet-beta.solana.com");

       // Decode private key
       const fromAccount = new Account(Buffer.from(fromPrivateKey, "hex"));

       // Get the public key for the destination address
       const toAccount = new PublicKey(toPublicKey);

       // Amount to withdraw (in SOL)
       const lamports = amount * Math.pow(10, 9); // Convert SOL to lamports

       // Build transaction
       const transaction = new Transaction().add(
         SystemProgram.transfer({
           fromPubkey: fromAccount.publicKey,
           toPubkey: toAccount,
           lamports: lamports,
         })
       );

       // Sign transaction
       const signature = await sendAndConfirmTransaction(
         connection,
         transaction,
         [fromAccount],
         { commitment: "confirmed" }
       );

       console.log("Transaction Signature:", signature);
     } catch (error) {
       console.error("Error:", error);
     }
   }

   // Example usage
   const fromPrivateKey = "YOUR_PRIVATE_KEY"; // Private key of the sender account
   const toPublicKey = "DESTINATION_PUBLIC_KEY"; // Public key of the receiver account
   const amount = 1; // Amount of SOL to withdraw (in SOL)

   withdrawSOL(fromPrivateKey, toPublicKey, amount);
   ```

   Replace `'YOUR_PRIVATE_KEY'` with the sender's private key and `'DESTINATION_PUBLIC_KEY'` with the receiver's public key. Also, specify the amount of SOL to withdraw in the `amount` variable.

3. **Run the Script:**
   Run the Node.js script using Node.js:
   ```
   node withdrawSOL.js
   ```

This script will construct a transaction to transfer SOL from the sender's address to the receiver's address, sign the transaction using the sender's private key, and then submit the transaction to the Solana network. After successful execution, the transaction signature will be logged to the console.

Make sure to replace the placeholder values with your actual Solana addresses and private key. Also, ensure that your sender address has sufficient SOL balance to cover the withdrawal amount and transaction fees.
