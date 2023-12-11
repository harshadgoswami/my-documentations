Certainly! Below is a comprehensive example of using Memcached with Node.js and TypeScript.

### Step 1: Set up a Node.js TypeScript project

Create a new Node.js project with TypeScript:

1. Initialize the project:

   ```bash
   mkdir memcached-nodejs-typescript
   cd memcached-nodejs-typescript
   npm init -y
   ```

2. Install necessary dependencies:

   ```bash
   npm install memjs memjs-promisified @types/memjs @types/node typescript ts-node --save-dev
   ```

3. Create a `tsconfig.json` file:
   ```json
   {
     "compilerOptions": {
       "target": "es6",
       "module": "commonjs",
       "strict": true,
       "esModuleInterop": true,
       "skipLibCheck": true,
       "forceConsistentCasingInFileNames": true
     }
   }
   ```

### Step 2: Configure Memcached Connection

Create a `MemcachedService.ts` file to manage the Memcached connection:

```typescript
// MemcachedService.ts

import * as memjs from "memjs";

class MemcachedService {
  private client: memjs.Client;

  constructor() {
    const MEMCACHED_SERVER = "YOUR_MEMCACHED_SERVER_URL"; // Replace with your Memcached server URL
    const MEMCACHED_USERNAME = "YOUR_MEMCACHED_USERNAME"; // Replace with your Memcached username
    const MEMCACHED_PASSWORD = "YOUR_MEMCACHED_PASSWORD"; // Replace with your Memcached password

    this.client = memjs.Client.create(`${MEMCACHED_SERVER}`, {
      username: `${MEMCACHED_USERNAME}`,
      password: `${MEMCACHED_PASSWORD}`,
    });
  }

  async get(key: string): Promise<any | null> {
    try {
      const result = await this.client.get(key);
      if (result && result.value) {
        return JSON.parse(result.value.toString());
      }
      return null;
    } catch (error) {
      console.error("Error getting data from Memcached:", error);
      throw error;
    }
  }

  async set(
    key: string,
    data: any,
    expiresInSeconds: number = 60
  ): Promise<void> {
    try {
      await this.client.set(key, JSON.stringify(data), {
        expires: expiresInSeconds,
      });
    } catch (error) {
      console.error("Error setting data in Memcached:", error);
      throw error;
    }
  }
}

export default MemcachedService;
```

### Step 3: Use Memcached in a TypeScript file

Now, create a TypeScript file where you want to use the Memcached service, for example, `app.ts`:

```typescript
// app.ts

import MemcachedService from "./MemcachedService";

async function run() {
  const memcachedService = new MemcachedService();

  const key = "example_key";

  // Example: Fetch data from Memcached
  const cachedData = await memcachedService.get(key);
  if (cachedData) {
    console.log("Data fetched from Memcached:", cachedData);
  } else {
    // Example: If data not found in Memcached, set it and retrieve
    const newData = { message: "Hello, Memcached!" };
    await memcachedService.set(key, newData, 60); // Cache for 60 seconds
    console.log("Data set in Memcached:", newData);

    const fetchedData = await memcachedService.get(key);
    console.log("Data fetched from Memcached after setting:", fetchedData);
  }
}

run().catch(console.error);
```

Replace `'YOUR_MEMCACHED_SERVER_URL'`, `'YOUR_MEMCACHED_USERNAME'`, and `'YOUR_MEMCACHED_PASSWORD'` with your actual Memcached server details.

### Step 4: Compile and Run

Run the TypeScript code:

```bash
npx ts-node app.ts
```

This example demonstrates how to create a `MemcachedService` class to manage the Memcached connection and how to use it to get and set data within a TypeScript file.

Adjust the code according to your project's needs and error handling requirements. This example provides a basic setup for using Memcached with Node.js and TypeScript.
