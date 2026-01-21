Composer is a **PHP dependency manager**. Here’s how to install it step-by-step on **Windows**, **Linux**, and **macOS**.
(I’ll keep it simple 👍)

---

## ✅ Install Composer on **Windows** (Recommended way)

### 🔹 Step 1: Download Installer

1. Go to **[https://getcomposer.org/download/](https://getcomposer.org/download/)**
2. Download **Composer-Setup.exe**

### 🔹 Step 2: Run Installer

- Double-click `Composer-Setup.exe`
- When asked for PHP path:
    - Usually auto-detected (e.g. `C:\xampp\php\php.exe`)

- Click **Next → Install**

### 🔹 Step 3: Verify Installation

Open **Command Prompt** and run:

```bash
composer --version
```

✔ If you see version info, Composer is installed successfully.

---

## ✅ Install Composer on **Linux / macOS**

### 🔹 Step 1: Download Composer

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
```

### 🔹 Step 2: Install Globally

```bash
php composer-setup.php
sudo mv composer.phar /usr/local/bin/composer
```

### 🔹 Step 3: Verify

```bash
composer --version
```

---

## ✅ Install Composer **Locally (Project only)**

If you don’t want global install:

```bash
php composer-setup.php
```

Run it using:

```bash
php composer.phar install
```

---

## 🔧 Common Issues & Fixes

### ❌ `composer is not recognized`

- Restart terminal
- Ensure PHP is added to **PATH**

### ❌ PHP not found

Check:

```bash
php -v
```

If not found, install PHP first.

---

## 📌 Using Composer (Basic Example)

```bash
composer init
composer require monolog/monolog
```

---

If you want:

- ✅ Composer for **XAMPP / WAMP**
- ✅ Offline installation
- ✅ Fix common Windows errors

Tell me your **OS** and **PHP setup** 😊
