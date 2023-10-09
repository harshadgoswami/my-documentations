In ethers.js, you can use the `JsonRpcProvider` to interact with Ethereum accounts and execute contract methods. Here's a step-by-step guide on how to load an account and execute a contract method using this provider:

1. Import the necessary modules:

```javascript
const ethers = require("ethers");
```

2. Create a `JsonRpcProvider` and connect it to your Ethereum node or RPC endpoint:

```javascript
const provider = new ethers.JsonRpcProvider("YOUR_RPC_ENDPOINT");
```

Replace `'YOUR_RPC_ENDPOINT'` with the URL of your Ethereum node or RPC endpoint.

3. Load your Ethereum account using a private key or JSON wallet file. You can use either method based on your use case.

   a. Load an account from a private key:

   ```javascript
   const privateKey = "YOUR_PRIVATE_KEY";
   const wallet = new ethers.Wallet(privateKey, provider);
   ```

   Replace `'YOUR_PRIVATE_KEY'` with the private key of the account you want to load.

   b. Load an account from a JSON wallet file:

   ```javascript
   const walletFilePath = "PATH_TO_YOUR_JSON_WALLET_FILE.json";
   const password = "YOUR_WALLET_PASSWORD";

   const wallet = await ethers.Wallet.fromEncryptedJsonFile(
     walletFilePath,
     password,
     provider
   );
   ```

   Replace `'PATH_TO_YOUR_JSON_WALLET_FILE.json'` with the path to your JSON wallet file and `'YOUR_WALLET_PASSWORD'` with the password to decrypt the wallet.

4. Load the contract ABI (Application Binary Interface) and address of the contract you want to interact with:

```javascript
const contractAddress = "CONTRACT_ADDRESS";
const contractAbi = [
  // Define your contract ABI here
  // Example:
  {
    constant: true,
    inputs: [],
    name: "getSomeValue",
    outputs: [{ name: "", type: "uint256" }],
    type: "function",
  },
  // Add more functions and events as needed
];

const contract = new ethers.Contract(contractAddress, contractAbi, wallet);
```

Replace `'CONTRACT_ADDRESS'` with the address of the Ethereum smart contract you want to interact with and define the contract ABI based on the methods and events of your contract.

5. Execute a contract method:

You can now call methods of the `contract` object to interact with the contract. For example, to call the `getSomeValue` function:

```javascript
async function callContractMethod() {
  try {
    const result = await contract.getSomeValue();
    console.log("Result:", result.toString());
  } catch (error) {
    console.error("Error:", error);
  }
}

callContractMethod();
```

This code snippet calls the `getSomeValue` function of the contract and logs the result.

Remember to handle errors appropriately when interacting with contracts on the Ethereum network, as transactions may fail for various reasons.
