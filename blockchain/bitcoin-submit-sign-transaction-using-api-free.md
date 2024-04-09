To broadcast a signed Bitcoin transaction, you can use various free API services provided by blockchain explorers or Bitcoin node providers. Here's a simple example using a free API provided by Blockstream's Esplora:

1. **Construct and Sign Transaction:**
   First, construct and sign your Bitcoin transaction using `bitcoinjs-lib` as shown in the previous example.

2. **Choose a Block Explorer API:**
   Blockstream's Esplora provides a free-to-use API that allows you to broadcast Bitcoin transactions. You can use their testnet or mainnet API depending on your needs:

   - Testnet: https://blockstream.info/testnet/api
   - Mainnet: https://blockstream.info/api

3. **Broadcast Transaction:**
   Use `axios` or any other HTTP request library to send a POST request to the API endpoint with your signed transaction hex. Here's an example using Axios:

   ```javascript
   const axios = require("axios");

   async function broadcastTransaction(signedTransactionHex, network) {
     try {
       const apiUrl =
         network === "testnet"
           ? "https://blockstream.info/testnet/api"
           : "https://blockstream.info/api";
       const response = await axios.post(`${apiUrl}/tx`, signedTransactionHex);
       console.log("Transaction ID:", response.data);
     } catch (error) {
       console.error("Error broadcasting transaction:", error.response.data);
     }
   }

   // Example usage
   const signedTransactionHex = "SIGNED_TRANSACTION_HEX";
   const network = "testnet"; // or 'mainnet'

   broadcastTransaction(signedTransactionHex, network);
   ```

   Replace `'SIGNED_TRANSACTION_HEX'` with your signed transaction hexadecimal string and `'testnet'` or `'mainnet'` depending on the network you are using.

4. **Run the Script:**
   Run the Node.js script using Node.js:
   ```
   node broadcastBitcoinTransaction.js
   ```

This script will send the signed transaction to the Blockstream's Esplora API, which will then broadcast it to the Bitcoin network. After successful execution, the transaction ID will be logged to the console.

Make sure to replace the placeholder values with your actual signed transaction hex and network. Additionally, remember that broadcasting transactions involves interacting with a third-party service, so ensure that you're comfortable with the privacy and security implications.
