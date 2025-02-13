If you want to send **TON payments directly** to a specific wallet address without a smart contract, you can use **TON Connect** in your React-based Telegram bot. Below is how you can integrate it:

---

### **1. Install TON Connect SDK**

Run the following command to install TON Connect:

```bash
npm install @tonconnect/sdk
```

---

### **2. Set Up TON Connect in Your React App**

Create a **TonConnectButton.tsx** to handle wallet connection.

#### **TonConnectButton.tsx**

```tsx
import React, { useEffect, useState } from "react";
import { TonConnectUI, useTonConnect } from "@tonconnect/ui-react";

const TonConnectButton: React.FC = () => {
    const [tonConnectUI] = useState(
        () =>
            new TonConnectUI({
                manifestUrl: "https://your-domain.com/tonconnect-manifest.json",
            })
    );
    const { connected, account } = useTonConnect();

    return (
        <div>
            {!connected ? (
                <button onClick={() => tonConnectUI.connect()}>
                    Connect Wallet
                </button>
            ) : (
                <div>
                    <p>Connected to: {account?.address}</p>
                    <button onClick={() => tonConnectUI.disconnect()}>
                        Disconnect
                    </button>
                </div>
            )}
        </div>
    );
};

export default TonConnectButton;
```

---

### **3. Send TON to a Specific Address**

Now, create a **SendTON.tsx** component where users can **send TON to a fixed address**.

#### **SendTON.tsx**

```tsx
import { useTonConnect } from "@tonconnect/ui-react";

const SendTON = () => {
    const { sendTransaction, account } = useTonConnect();

    const handleSendTON = async () => {
        if (!account) {
            alert("Please connect your wallet first!");
            return;
        }

        const recipientAddress = "YOUR_TON_WALLET_ADDRESS"; // Replace with your TON address
        const amount = "1000000000"; // 1 TON in nanoTON

        const txParams = {
            to: recipientAddress,
            value: amount,
            message: "Payment for asset",
        };

        await sendTransaction(txParams);
        alert("Payment Sent!");
    };

    return <button onClick={handleSendTON}>Pay with TON</button>;
};

export default SendTON;
```

---

### **4. Add Components to Your Telegram Bot UI**

In your **main bot UI file (`App.tsx`)**, import the TON connection button and send button.

```tsx
import React from "react";
import TonConnectButton from "./TonConnectButton";
import SendTON from "./SendTON";

const BotUI: React.FC = () => {
    return (
        <div>
            <h2>Welcome to our Telegram Bot</h2>
            <TonConnectButton />
            <SendTON />
        </div>
    );
};

export default BotUI;
```

---

### **5. Deploy and Test**

1. **Run your React app**:
    ```bash
    npm start
    ```
2. **Deploy on Vercel or Netlify** for production.
3. **Test TON payments** in a real wallet.

---

### **What This Does**

✅ Users **connect their TON wallet** (like Tonkeeper).  
✅ Users **send TON directly** to a fixed **wallet address** (no smart contract required).  
✅ **Fast and easy integration** in a React-based Telegram bot.

Let me know if you need more features! 🚀
