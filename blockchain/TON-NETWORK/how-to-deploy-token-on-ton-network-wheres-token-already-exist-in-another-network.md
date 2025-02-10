To launch your own token called **FINA** on the **TON Blockchain**, follow these steps:

---

### **1. Install Required Tools**

#### Install TON CLI

```bash
sudo apt update
sudo apt install build-essential clang libssl-dev pkg-config
curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt install nodejs
npm install -g tondev
```

#### Install `toncli`

```bash
pip3 install toncli
```

---

### **2. Set Up TON Wallet**

Create a wallet to deploy and manage your tokens:

```bash
toncli create wallet
toncli wallet deploy
```

Secure your wallet address and private keys.

---

### **3. Write the Smart Contract for FINA Token**

Use the **Fungible Token Standard (FT)** on the TON blockchain.

1. Create a directory for your project:

    ```bash
    mkdir FINA && cd FINA
    ```

2. Create `FinaToken.fc` (a smart contract template for fungible tokens):

```c
#include "stdlib.fc";

int constant FINA_TOTAL_SUPPLY = 1000000000; // 1 Billion tokens

contract FinaToken {
  int totalSupply;
  map(address, int) balances;

  constructor() public {
    totalSupply = FINA_TOTAL_SUPPLY;
    balances[msg.pubkey()] = totalSupply;
  }

  function getTotalSupply() public method_id(0x01) returns (int) {
    return totalSupply;
  }

  function getBalance(address owner) public method_id(0x02) returns (int) {
    return balances[owner];
  }

  function transfer(address to, int amount) public method_id(0x03) {
    require(balances[msg.pubkey()] >= amount, 101);
    balances[msg.pubkey()] -= amount;
    balances[to] += amount;
  }
}
```

---

### **4. Compile the Smart Contract**

```bash
toncli build FinaToken.fc
```

---

### **5. Deploy the Smart Contract**

```bash
toncli deploy FinaToken.fc --sign
```

---

### **6. Verify and Test the Token**

To interact with your token:

-   Check Total Supply
    ```bash
    toncli call FinaToken.fc getTotalSupply
    ```
-   Check Wallet Balance
    ```bash
    toncli call FinaToken.fc getBalance <wallet_address>
    ```
-   Transfer Tokens
    ```bash
    toncli call FinaToken.fc transfer <to_address> <amount>
    ```

---

### **7. Minting and Burning (Optional)**

Add functions to mint and burn FINA tokens:

```c
function mint(int amount) public method_id(0x04) {
  require(msg.pubkey() == owner_pubkey, 101);
  totalSupply += amount;
  balances[msg.pubkey()] += amount;
}

function burn(int amount) public method_id(0x05) {
  require(balances[msg.pubkey()] >= amount, 101);
  totalSupply -= amount;
  balances[msg.pubkey()] -= amount;
}
```

---

### **8. Deploy and List on TON Blockchain**

-   Announce your token by listing it on TON marketplaces like **TONScan**.
-   Interact with wallets and exchanges using compatible standards.

---

### **9. Security Tips**

-   Use audited contracts or hire a security audit firm.
-   Maintain private key safety and test thoroughly on the **TON Testnet** before launching on the **Mainnet**.
