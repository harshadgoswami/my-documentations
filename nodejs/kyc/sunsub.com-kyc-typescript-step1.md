To implement **Sumsub.com KYC (Know Your Customer)** in a **Node.js** + **TypeScript** application, follow these steps:

---

### **Step 1: Set Up Sumsub**

1. **Register for Sumsub** at [Sumsub.com](https://sumsub.com/).
2. Create a project and obtain:
    - **App Token** (for API authorization).
    - **Secret Key** (for signing API requests).

---

### **Step 2: Install Dependencies**

```bash
npm init -y
npm install axios crypto dotenv
npm install @types/node --save-dev
```

---

### **Step 3: Project Structure**

```
.
├── src
│   ├── kycService.ts
│   └── index.ts
└── .env
```

---

### **Step 4: Configure Environment Variables (`.env`)**

```env
SUMSUB_APP_TOKEN=your_app_token
SUMSUB_SECRET_KEY=your_secret_key
SUMSUB_BASE_URL=https://api.sumsub.com
```

---

### **Step 5: KYC Service (`kycService.ts`)**

```typescript
import axios from "axios";
import crypto from "crypto";
import dotenv from "dotenv";

dotenv.config();

class KYCService {
    private baseUrl: string;
    private appToken: string;
    private secretKey: string;

    constructor() {
        this.baseUrl = process.env.SUMSUB_BASE_URL || "";
        this.appToken = process.env.SUMSUB_APP_TOKEN || "";
        this.secretKey = process.env.SUMSUB_SECRET_KEY || "";
    }

    private createSignature(path: string, body: string, ts: number): string {
        const data = ts + path + body;
        return crypto
            .createHmac("sha256", this.secretKey)
            .update(data)
            .digest("hex");
    }

    async createApplicant(userId: string, externalUserId: string) {
        const path = `/resources/applicants?levelName=basic-kyc-level`;
        const ts = Math.floor(Date.now() / 1000);
        const body = JSON.stringify({
            externalUserId: externalUserId,
            info: {
                firstName: "John",
                lastName: "Doe",
                dob: "1990-01-01",
                country: "US",
            },
        });

        const signature = this.createSignature(path, body, ts);

        try {
            const response = await axios.post(this.baseUrl + path, body, {
                headers: {
                    "X-App-Token": this.appToken,
                    "X-App-Access-Sig": signature,
                    "X-App-Access-Ts": ts,
                    "Content-Type": "application/json",
                },
            });

            console.log("Applicant created:", response.data);
            return response.data;
        } catch (error) {
            console.error("Error creating applicant:", error);
            throw error;
        }
    }
}

export default KYCService;
```

---

### **Step 6: Application Entry (`index.ts`)**

```typescript
import KYCService from "./kycService";

const kycService = new KYCService();

(async () => {
    try {
        const applicant = await kycService.createApplicant(
            "user123",
            "external-user-123"
        );
        console.log("KYC Applicant:", applicant);
    } catch (error) {
        console.error("KYC process failed:", error);
    }
})();
```

---

### **Step 7: Run the Application**

1. **Compile TypeScript**:

    ```bash
    npx tsc
    ```

2. **Run the Service**:
    ```bash
    node dist/index.js
    ```

---

### **Key Notes**

-   **Signature Calculation**: Ensure correct signing with timestamp + path + body.
-   **API Request Headers**:
    -   `X-App-Token`: Sumsub App Token.
    -   `X-App-Access-Sig`: Generated HMAC signature.
    -   `X-App-Access-Ts`: Current timestamp.

---

### **Sample API Response**

```json
{
    "id": "12345",
    "externalUserId": "external-user-123",
    "inspectionId": "abcdef12345"
}
```

This setup provides a solid starting point for integrating Sumsub KYC in a Node.js + TypeScript application. You can extend it further for other endpoints, such as document uploads and verification status checks.
