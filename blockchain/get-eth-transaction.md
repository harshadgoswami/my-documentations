In ethers.js, you can fetch Ethereum (ETH) transfer transactions by filtering logs for specific topics related to Ethereum transfer events. Here's an example of how you might fetch only ETH transfer transactions:

```javascript
const { ethers } = require("ethers");

// Initialize provider (replace with your own provider)
const provider = new ethers.providers.JsonRpcProvider(
  "YOUR_JSON_RPC_PROVIDER_URL"
);

// ETH Transfer event topics
const transferEventTopic = ethers.utils.id("Transfer(address,address,uint256)");
const topicFilter = {
  topics: [transferEventTopic],
};

// Function to fetch ETH transfer transactions
async function fetchEthTransactions() {
  const blockNumber = await provider.getBlockNumber();

  // Get past events using provider's getLogs method
  const ethTransfers = await provider.getLogs({
    ...topicFilter,
    fromBlock: 0,
    toBlock: blockNumber,
  });

  // Print the ETH transfer transactions
  ethTransfers.forEach((log) => {
    if (log.topics.length === 3 && log.topics[0] === transferEventTopic) {
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
    }
  });
}

fetchEthTransactions();
```

Explanation:

1. This code initializes an ethers.js provider to interact with the Ethereum network.
2. It specifies a filter for logs related to Ethereum transfer events (`Transfer` event for ETH).
3. The code uses `getLogs` function of the provider to fetch all logs (events) related to the `Transfer` event of Ethereum.
4. It retrieves transaction details like transaction hash, sender, receiver, amount, and block number for each ETH transfer event.

Replace `'YOUR_JSON_RPC_PROVIDER_URL'` with your Ethereum JSON-RPC provider URL.

This code snippet fetches past Ethereum transfer events that represent ETH transactions. It filters logs based on the `Transfer` event topic, which indicates an ETH transfer. You can further process or filter the data as needed for your use case.
