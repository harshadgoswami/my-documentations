Certainly! Here's an example of transferring Ether from one account to another using ethers.js in a TypeScript Node.js environment:

Ensure you've installed the `ethers` package in your project:

```bash
npm install ethers
```

Then, use the following TypeScript code:

```typescript
import { ethers } from "ethers";

async function transferEther() {
  // Ethereum provider URL
  const provider = new ethers.providers.JsonRpcProvider(
    "YOUR_JSON_RPC_PROVIDER_URL"
  );

  // Sender's private key
  const senderPrivateKey = "SENDER_PRIVATE_KEY";
  const senderWallet = new ethers.Wallet(senderPrivateKey, provider);

  // Recipient's address
  const recipientAddress = "RECIPIENT_ADDRESS";

  // Amount in Ether to transfer
  const amountToSend = ethers.utils.parseEther("1"); // Example: Sending 1 Ether

  // Create transaction object
  const tx = {
    to: recipientAddress,
    value: amountToSend,
  };

  try {
    // Send transaction
    const sentTx = await senderWallet.sendTransaction(tx);
    console.log("Transaction hash:", sentTx.hash);
  } catch (error) {
    console.error("Error:", error);
  }
}

transferEther();
```

Replace `'YOUR_JSON_RPC_PROVIDER_URL'` with the Ethereum JSON-RPC provider URL, `'SENDER_PRIVATE_KEY'` with the private key of the sender account, and `'RECIPIENT_ADDRESS'` with the Ethereum address of the recipient account.

Ensure that the sender's account has enough Ether to cover the transaction fees and the amount to be sent. Also, handle the private keys securely and avoid exposing them in your code or repository.
