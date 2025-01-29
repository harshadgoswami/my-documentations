Yes, **Sumsub** provides a way to generate an **inquiry URL** that allows users to upload documents or capture photos using their webcam directly on Sumsub's hosted platform. This feature simplifies the KYC process by offloading UI and data handling to Sumsub.

---

### **Step 1: Generate an Inquiry URL**

To generate the URL, use the following API:

**Endpoint:**  
`POST /resources/accessTokens`

The response contains a URL (`url`) that you can redirect the user to.

---

### **Example Implementation in TypeScript**

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

    async generateInquiryUrl(applicantId: string): Promise<string> {
        const path = `/resources/accessTokens`;
        const ts = Math.floor(Date.now() / 1000);
        const body = JSON.stringify({
            applicantId: applicantId,
            ttlInSecs: 600, // URL expiration time in seconds
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

            console.log("Generated inquiry URL:", response.data);
            return response.data.url;
        } catch (error) {
            console.error("Error generating inquiry URL:", error);
            throw error;
        }
    }
}

(async () => {
    const kycService = new KYCService();
    try {
        const inquiryUrl = await kycService.generateInquiryUrl(
            "your-applicant-id-here"
        );
        console.log("Redirect the user to this URL:", inquiryUrl);
    } catch (error) {
        console.error("Failed to generate inquiry URL");
    }
})();
```

---

### **Response Example**

```json
{
    "url": "https://test-api.sumsub.com/sdk/web/tt7e-bb4ca3bb-c9ab-11eb-8529-0242ac130003"
}
```

---

### **Step 2: Redirect or Show the URL**

After generating the inquiry URL, you can:

1. **Redirect the User:**
    ```typescript
    res.redirect(inquiryUrl);
    ```
2. **Display as a Link:**
    ```html
    <a href="https://generated-url-here" target="_blank">Complete KYC</a>
    ```

---

### **Optional Parameters**

You can customize the inquiry by adding extra fields in the request body:

-   `ttlInSecs`: Time-to-live for the inquiry URL.
-   `externalUserId`: Track user sessions.

---

### **Benefits of Inquiry URLs**

-   Simplifies user KYC experience.
-   Sumsub handles UI, document upload, and validation.
-   Reduces development complexity on your side.

This approach is ideal for both mobile and desktop applications.
