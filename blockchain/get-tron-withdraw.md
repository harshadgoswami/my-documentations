To withdraw TRX (TRON's native cryptocurrency) from a Tron address to another address, you need to construct and sign a transaction using the TronWeb library in Node.js. Here's how you can do it:

1. **Install Required Packages:**
   Make sure you have the `tronweb` package installed. You can install it via npm if you haven't already:

   ```
   npm install tronweb
   ```

2. **Write Withdraw Code:**
   Below is a Node.js script (`withdrawTRX.js`) that demonstrates how to withdraw TRX from one Tron address to another:

   ```javascript
   const TronWeb = require("tronweb");

   async function withdrawTRX(fromAddress, toAddress, privateKey, amount) {
     try {
       // Create a new instance of TronWeb
       const tronWeb = new TronWeb({
         fullHost: "https://api.trongrid.io",
         privateKey: privateKey,
       });

       // Convert amount to sun (1 TRX = 1,000,000 sun)
       const amountSun = tronWeb.toSun(amount);

       // Build transaction
       const transaction = await tronWeb.transactionBuilder.sendTrx(
         toAddress,
         amountSun,
         fromAddress
       );

       // Sign transaction
       const signedTransaction = await tronWeb.trx.sign(transaction);

       // Broadcast transaction
       const result = await tronWeb.trx.sendRawTransaction(signedTransaction);
       console.log("Transaction ID:", result.txid);
     } catch (error) {
       console.error("Error:", error);
     }
   }

   // Example usage
   const fromAddress = "YOUR_FROM_ADDRESS";
   const toAddress = "YOUR_TO_ADDRESS";
   const privateKey = "YOUR_PRIVATE_KEY";
   const amount = 100; // Amount of TRX to withdraw (in TRX)

   withdrawTRX(fromAddress, toAddress, privateKey, amount);
   ```

   Replace `'YOUR_FROM_ADDRESS'`, `'YOUR_TO_ADDRESS'`, and `'YOUR_PRIVATE_KEY'` with the sender address, receiver address, and the corresponding private key respectively. Also, specify the amount of TRX to withdraw in the `amount` variable.

3. **Run the Script:**
   Run the Node.js script using Node.js:
   ```
   node withdrawTRX.js
   ```

This script will construct a transaction to transfer TRX from the specified sender address to the receiver address, sign the transaction using the sender's private key, and then broadcast the transaction to the Tron network. After successful execution, the transaction ID will be logged to the console.

Make sure to replace the placeholder values with your actual Tron addresses and private key. Also, ensure that your sender address has sufficient TRX balance to cover the withdrawal amount and transaction fees.
