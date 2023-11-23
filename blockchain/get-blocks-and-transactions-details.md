Absolutely, using `ethers.providers.JsonRpcProvider`, you can get a specific block and then retrieve transactions within that block. Here's an example in TypeScript:

```typescript
import { ethers } from "ethers";

async function getBlockAndTransactions(blockNumber: number) {
  const provider = new ethers.providers.JsonRpcProvider(
    "YOUR_ETHEREUM_NODE_URL"
  );

  try {
    // Get block information
    const block = await provider.getBlock(blockNumber);
    console.log("Block Number:", block.number);
    console.log("Timestamp:", new Date(block.timestamp * 1000)); // Convert UNIX timestamp to JavaScript Date object
    console.log("Hash:", block.hash);
    console.log("---------------------------");

    // Get transactions in the block
    if (block.transactions.length > 0) {
      for (const txHash of block.transactions) {
        const tx = await provider.getTransaction(txHash);
        console.log("Transaction Hash:", tx.hash);
        console.log("From:", tx.from);
        console.log("To:", tx.to);
        console.log("Value:", ethers.utils.formatEther(tx.value));
        console.log("---------------------------");
      }
    } else {
      console.log("No transactions in this block.");
    }
  } catch (error) {
    console.error("Error:", error);
  }
}

// Replace 'YOUR_ETHEREUM_NODE_URL' with your Ethereum node URL
// Replace 'BLOCK_NUMBER' with the block number you want to query
const blockNumber = BLOCK_NUMBER;
getBlockAndTransactions(blockNumber);
```

Replace `'YOUR_ETHEREUM_NODE_URL'` with the URL of your Ethereum node, and replace `'BLOCK_NUMBER'` with the specific block number you want to retrieve.

This code snippet demonstrates fetching block details using `provider.getBlock(blockNumber)` and then retrieving transaction details for each transaction hash in that block using `provider.getTransaction(txHash)`.

Keep in mind that some Ethereum nodes may limit the number of transactions returned per block. Additionally, querying historical blocks with a large number might take more time and resources, so consider this while retrieving block and transaction details.
