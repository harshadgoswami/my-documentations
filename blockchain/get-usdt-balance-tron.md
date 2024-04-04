To get the USDT balance on the Tron network using Node.js and TypeScript, you'll need to interact with the Tron blockchain through its HTTP API and the USDT token contract deployed on the Tron network. You can achieve this using libraries such as `axios` for making HTTP requests. Here's how you can do it:

1. **Install Required Packages:**
   Install the `axios` package via npm:

   ```
   npm install axios dotenv
   ```

2. **Set Up Your Environment:**
   Create a `.env` file in your project directory to store your Tron HTTP API endpoint and the USDT token address. For example:

   ```
   TRON_HTTP_ENDPOINT=https://api.trongrid.io
   USDT_CONTRACT_ADDRESS=USDT_CONTRACT_ADDRESS_HERE
   ```

3. **Write TypeScript Code:**
   Create a TypeScript file (e.g., `getUSDTBalance.ts`) and write the following code:

   ```typescript
   import axios from "axios";
   import dotenv from "dotenv";

   // Load environment variables
   dotenv.config();

   async function getUSDTBalance(address: string) {
     try {
       // Make HTTP request to get account information
       const response = await axios.post(
         `${process.env.TRON_HTTP_ENDPOINT}/wallet/getaccount`,
         { address: address }
       );

       const accountData = response.data;
       if (!accountData || !accountData.assetV2) {
         throw new Error("Account data or assetV2 not found");
       }

       // Find USDT balance from assetV2 array
       const usdtBalance = accountData.assetV2.find(
         (asset: any) => asset.key === process.env.USDT_CONTRACT_ADDRESS
       );
       if (!usdtBalance) {
         console.log("USDT balance: 0");
       } else {
         console.log(`USDT balance: ${usdtBalance.value}`);
       }
     } catch (error) {
       console.error("Error:", error);
     }
   }

   // Example usage
   const tronAddress = "TRON_ADDRESS_HERE";
   getUSDTBalance(tronAddress);
   ```

4. **Run the Script:**
   Compile the TypeScript file using a TypeScript compiler (e.g., `tsc`) and then run the generated JavaScript file using Node.js:
   ```
   tsc getUSDTBalance.ts
   node getUSDTBalance.js
   ```

This script will connect to the Tron network using the provided HTTP endpoint, fetch the account information for the specified Tron address, and then extract and log the USDT balance from the account data.

Make sure to replace the placeholder USDT token address with the actual address of the USDT token on the Tron network, and replace the placeholder Tron address with the actual address for which you want to get the USDT balance. Also, ensure that your Tron HTTP endpoint is correct and accessible.
