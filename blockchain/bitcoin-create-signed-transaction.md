To initiate a Bitcoin withdrawal transaction programmatically, you typically need to use a Bitcoin wallet library like `bitcoinjs-lib` in Node.js. Below is a basic example of how you can create a Bitcoin withdrawal transaction using `bitcoinjs-lib`:

1. **Install Required Packages:**
   Make sure you have the `bitcoinjs-lib` package installed. You can install it via npm if you haven't already:

   ```
   npm install bitcoinjs-lib
   ```

2. **Write Withdrawal Code:**
   Below is a Node.js script (`withdrawBitcoin.js`) that demonstrates how to create and sign a Bitcoin withdrawal transaction:

   ```javascript
   const bitcoin = require("bitcoinjs-lib");

   // Define private key and destination address
   const privateKeyWIF = "YOUR_PRIVATE_KEY_WIF";
   const destinationAddress = "DESTINATION_BITCOIN_ADDRESS";

   // Create a Bitcoin network (mainnet or testnet)
   const network = bitcoin.networks.testnet; // Change to 'bitcoin.networks.mainnet' for mainnet

   // Create a Bitcoin key pair from the private key
   const keyPair = bitcoin.ECPair.fromWIF(privateKeyWIF, network);

   // Create a Bitcoin transaction builder
   const txb = new bitcoin.TransactionBuilder(network);

   // Add input (UTXO) to the transaction
   // Example UTXO (unspent transaction output)
   const txid = "INPUT_TXID";
   const vout = 0; // Index of the output in the transaction
   const amount = 10000; // Amount in satoshis (0.0001 BTC)
   txb.addInput(txid, vout);

   // Add output (destination address) to the transaction
   const outputAmount = amount - 1000; // Subtract fee (e.g., 1000 satoshis)
   txb.addOutput(destinationAddress, outputAmount);

   // Sign the transaction with the private key
   txb.sign(0, keyPair);

   // Build the transaction
   const tx = txb.build();

   // Serialize the transaction as hexadecimal string
   const hex = tx.toHex();

   console.log("Signed Transaction:", hex);
   ```

   Replace `'YOUR_PRIVATE_KEY_WIF'` with your private key in Wallet Import Format (WIF) and `'DESTINATION_BITCOIN_ADDRESS'` with the recipient's Bitcoin address. You also need to specify the transaction ID (`txid`) and the index of the output (`vout`) of the UTXO you want to spend.

3. **Run the Script:**
   Run the Node.js script using Node.js:
   ```
   node withdrawBitcoin.js
   ```

This script constructs a Bitcoin transaction, signs it with the specified private key, and outputs the signed transaction in hexadecimal format. Make sure to replace the placeholder values with your actual private key, destination address, transaction ID, output index, and amount.

After obtaining the signed transaction, you need to broadcast it to the Bitcoin network using a broadcasting service or a Bitcoin node.
