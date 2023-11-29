In ethers.js, you can fetch specific ERC-20 token transactions by querying the Ethereum blockchain using a provider and filtering transactions based on the ERC-20 token transfer events.

Here's an example of how you might fetch ERC-20 token transactions for a specific token contract address:

```javascript
const { ethers } = require("ethers");

// Initialize provider (replace with your own provider)
const provider = new ethers.providers.JsonRpcProvider(
  "YOUR_JSON_RPC_PROVIDER_URL"
);

// ERC-20 token contract address
const tokenContractAddress = "YOUR_ERC20_TOKEN_CONTRACT_ADDRESS";

// ERC-20 token Transfer event topics
const transferEventTopic = ethers.utils.id("Transfer(address,address,uint256)");
const topicFilter = {
  address: tokenContractAddress,
  topics: [transferEventTopic],
};

// Function to fetch ERC-20 token transactions
async function fetchTokenTransactions() {
  const blockNumber = await provider.getBlockNumber();

  // Get past events using provider's getLogs method
  const tokenTransfers = await provider.getLogs({
    ...topicFilter,
    fromBlock: 0,
    toBlock: blockNumber,
  });

  // Print the transactions
  tokenTransfers.forEach((log) => {
    console.log("Transaction hash:", log.transactionHash);
    console.log(
      "From:",
      ethers.utils.getAddress("0x" + log.topics[1].substring(26))
    );
    console.log(
      "To:",
      ethers.utils.getAddress("0x" + log.topics[2].substring(26))
    );
    console.log("Amount:", ethers.BigNumber.from(log.data).toString());
    console.log("Block number:", log.blockNumber);
    console.log("-------------------");
  });
}

fetchTokenTransactions();
```

Explanation:

1. The code initializes an ethers.js provider to interact with the Ethereum network.
2. It specifies the ERC-20 token contract address for which you want to fetch transactions.
3. The code uses `getLogs` function of the provider to fetch all logs (events) related to the `Transfer` event of the ERC-20 token.
4. It retrieves transaction details like transaction hash, sender, receiver, amount, and block number for each Transfer event.

Replace `'YOUR_JSON_RPC_PROVIDER_URL'` with your Ethereum JSON-RPC provider URL and `'YOUR_ERC20_TOKEN_CONTRACT_ADDRESS'` with the ERC-20 token contract address you want to monitor.

This code snippet fetches past Transfer events for the specified ERC-20 token contract. You can further refine the filtering based on specific block ranges or additional conditions as needed.
