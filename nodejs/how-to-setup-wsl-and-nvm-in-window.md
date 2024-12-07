To install **NVM (Node Version Manager)** on Ubuntu, follow these steps:

---

### **1. Update Package List**

Open the Ubuntu terminal and update your system packages:

```bash
sudo apt update
sudo apt upgrade -y
```

---

### **2. Install Curl**

Ensure `curl` is installed:

```bash
sudo apt install curl -y
```

---

### **3. Download and Install NVM**

Run the following command to download and install NVM:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```

_This installs the latest stable version of NVM. If you want a specific version, replace `v0.39.5` with the desired version._

---

### **4. Activate NVM**

After installation, activate NVM by running:

```bash
source ~/.bashrc
```

or, if you use Zsh:

```bash
source ~/.zshrc
```

---

### **5. Verify Installation**

To confirm NVM is installed, run:

```bash
nvm --version
```

---

### **6. Install Node.js Using NVM**

- List available Node.js versions:
  ```bash
  nvm ls-remote
  ```
- Install a specific version (e.g., `18.17.1`):
  ```bash
  nvm install 18.17.1
  ```
- Use a specific version:
  ```bash
  nvm use 18.17.1
  ```
- Set a default Node.js version:
  ```bash
  nvm alias default 18.17.1
  ```

---

### **7. Verify Node.js Installation**

Check the installed Node.js and npm versions:

```bash
node -v
npm -v
```

You're all set! Let me know if you need further help.
