### **Setup Next.js with Express – Complete Steps**

This guide will show you how to set up **Next.js** with **Express** as a custom server.

---

## **1️⃣ Install Next.js & Express**

Run the following command to create a new **Next.js** project and install Express:

```sh
npx create-next-app@latest nextjs-express-app
cd nextjs-express-app
npm install express
```

---

## **2️⃣ Modify `package.json` Scripts**

Change the `start` script to run your custom server.

**Edit `package.json`**

```json
"scripts": {
  "dev": "node server.js",
  "build": "next build",
  "start": "NODE_ENV=production node server.js"
}
```

---

## **3️⃣ Create `server.js` (Custom Express Server)**

Create a **`server.js`** file in the root of your project.

```javascript
const express = require("express");
const next = require("next");

const dev = process.env.NODE_ENV !== "production";
const app = next({ dev });
const handle = app.getRequestHandler();

app.prepare().then(() => {
    const server = express();

    // Custom API Route
    server.get("/api/hello", (req, res) => {
        res.json({ message: "Hello from Express!" });
    });

    // Custom Page Route (Optional)
    server.get("/custom", (req, res) => {
        return app.render(req, res, "/custom-page");
    });

    // Handle all Next.js pages
    server.all("*", (req, res) => {
        return handle(req, res);
    });

    const PORT = process.env.PORT || 3000;
    server.listen(PORT, () => {
        console.log(`🚀 Server running on http://localhost:${PORT}`);
    });
});
```

---

## **4️⃣ Create a Test API Route in Next.js**

Create a Next.js API route for comparison.

**Create `pages/api/test.js`**

```javascript
export default function handler(req, res) {
    res.json({ message: "Hello from Next.js API!" });
}
```

---

## **5️⃣ Start the Server**

Run the development server:

```sh
npm run dev
```

🚀 Now visit:

-   `http://localhost:3000/api/hello` → **Express API**
-   `http://localhost:3000/api/test` → **Next.js API**
-   `http://localhost:3000/custom` → **Custom Express Page (if created)**
-   `http://localhost:3000` → **Next.js pages work normally**

---

## **6️⃣ Deploying to Production**

Run:

```sh
npm run build
npm start
```

---

### **🔹 When to Use Express with Next.js?**

✅ If you need **custom middleware** (auth, logging, rate limiting).  
✅ If you want to use **Express API routes instead of Next.js API routes**.  
✅ If you need **proxying** or **custom backend logic**.

📌 **Next.js does NOT need Express by default** unless you have specific backend requirements.

---

### **🎯 Next Steps**

-   Add **CORS, JWT auth, or middleware** in `server.js`
-   Use **Fastify instead of Express** for better performance
-   Deploy on **Vercel, DigitalOcean, AWS, or Heroku**

Would you like help with **deploying the setup**? 🚀
