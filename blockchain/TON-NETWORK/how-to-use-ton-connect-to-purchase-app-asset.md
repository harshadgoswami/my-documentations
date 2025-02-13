You can integrate **TON Connect** into your **React-based Telegram bot** to allow users to purchase in-app assets using TON. Below is a step-by-step guide to achieve this:

---

### **1. Install TON Connect SDK**

TON Connect allows users to connect their TON wallet (e.g., Tonkeeper) to your dApp.

Run the following command to install TON Connect SDK:

```bash
npm install @tonconnect/sdk
```

---

### **2. Set Up TON Connect in Your React App**

Create a **TonConnectButton** component to handle the wallet connection.

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

### **3. Add the Component to Your React Telegram Bot**

In your main app file (`App.tsx` or `BotUI.tsx`):

```tsx
import React from "react";
import TonConnectButton from "./TonConnectButton";

const BotUI: React.FC = () => {
    return (
        <div>
            <h2>Welcome to our Telegram Bot</h2>
            <TonConnectButton />
        </div>
    );
};

export default BotUI;
```

---

### **4. Set Up a TON Smart Contract for Purchases**

To process TON payments, you need a **smart contract** that receives TON and stores asset ownership.

#### **Smart Contract (Fift)**

```fift
const asset_price 1000000000 ; 1 TON

;; Function to accept payment
accept_msg
dup asset_price eq if
  accept
  "Purchase successful" .print
else
  drop
  "Insufficient funds" .print
then
```

Deploy this contract on TON Testnet or Mainnet.

---

### **5. Handle Transactions in React App**

Once the user is connected, use TON Connect to send a payment transaction.

#### **Handle TON Payment**

```tsx
import { useTonConnect } from "@tonconnect/ui-react";

const PurchaseButton = () => {
    const { sendTransaction, account } = useTonConnect();

    const handlePurchase = async () => {
        if (!account) {
            alert("Please connect your wallet first!");
            return;
        }

        const txParams = {
            to: "YOUR_SMART_CONTRACT_ADDRESS",
            value: "1000000000", // 1 TON in nanoTON
            message: "Purchase asset",
        };

        await sendTransaction(txParams);
    };

    return <button onClick={handlePurchase}>Buy Asset</button>;
};
```

---

### **6. Add the Purchase Button to Your Telegram Bot UI**

```tsx
import PurchaseButton from "./PurchaseButton";

const BotUI: React.FC = () => {
    return (
        <div>
            <h2>Welcome to our Telegram Bot</h2>
            <TonConnectButton />
            <PurchaseButton />
        </div>
    );
};
```

---

### **7. Deploy and Test**

-   Run your React bot using:
    ```bash
    npm start
    ```
-   Deploy on **Vercel** or **Netlify**.
-   Ensure **TON Wallet** transactions are processed correctly.

---

### **Conclusion**

By integrating TON Connect, users can:

1. **Connect their TON wallet** via **Tonkeeper**.
2. **Purchase assets** by sending **TON to a smart contract**.
3. **Own digital assets** inside the Telegram bot.

This approach allows seamless **Web3 payments** inside Telegram apps! 🚀
