To create a Tron address and its corresponding private key, you can use the TronWeb library in Node.js. TronWeb provides utilities for key generation and account management. Here's how you can do it:

1. **Install Required Packages:**
   Make sure you have the `tronweb` package installed. You can install it via npm:

   ```
   npm install tronweb
   ```

2. **Write Node.js Script:**
   Create a Node.js script (e.g., `generateTronAddress.js`) and write the following code:

   ```javascript
   const TronWeb = require("tronweb");

   async function generateTronAddress() {
     try {
       // Create a new instance of TronWeb
       const tronWeb = new TronWeb({
         fullHost: "https://api.trongrid.io",
       });

       // Generate a new address
       const account = await tronWeb.createAccount();

       console.log("Address:", account.address.base58);
       console.log("Private Key:", account.privateKey);
     } catch (error) {
       console.error("Error:", error);
     }
   }

   generateTronAddress();
   ```

3. **Run the Script:**
   Run the Node.js script using Node.js:
   ```
   node generateTronAddress.js
   ```

This script will generate a new Tron address and its corresponding private key. The address is represented in base58 format, and the private key is a hexadecimal string.

Make sure to securely store the private key as it grants access to the associated Tron address and funds. Never share your private key with anyone and consider using a secure key management solution.
