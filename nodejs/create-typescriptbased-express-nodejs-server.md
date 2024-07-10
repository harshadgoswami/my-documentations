Creating a TypeScript-based Node.js REST API server with Express involves several steps. Here’s a comprehensive guide:

### Step 1: Set Up the Project

1. **Initialize a new Node.js project:**

   ```bash
   mkdir express-typescript-api
   cd express-typescript-api
   npm init -y
   ```

2. **Install necessary packages:**

   ```bash
   npm install express
   npm install --save-dev typescript ts-node @types/node @types/express
   ```

3. **Initialize TypeScript:**
   ```bash
   npx tsc --init
   ```

### Step 2: Configure TypeScript

Edit the `tsconfig.json` file to set up the basic TypeScript configuration. Here’s a simple setup:

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*.ts"],
  "exclude": ["node_modules"]
}
```

### Step 3: Create the Server

1. **Create the project structure:**

   ```bash
   mkdir src
   touch src/index.ts
   mkdir src/routes
   touch src/routes/index.ts
   ```

2. **Setup the Express server in `src/index.ts`:**

```typescript
import express, { Request, Response, NextFunction } from "express";
import routes from "./routes";

const app = express();
const PORT = process.env.PORT || 3000;

// Middleware to parse JSON
app.use(express.json());

// API Routes
app.use("/api", routes);

// Error handling middleware
app.use((err: Error, req: Request, res: Response, next: NextFunction) => {
  console.error(err.stack);
  res.status(500).send({ error: err.message });
});

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

3. **Define routes in `src/routes/index.ts`:**

```typescript
import { Router, Request, Response } from "express";

const router = Router();

// Example route
router.get("/", (req: Request, res: Response) => {
  res.send("Hello, world!");
});

router.get("/data", (req: Request, res: Response) => {
  res.json({ message: "Hello, from the data endpoint!" });
});

export default router;
```

### Step 4: Add Scripts

Update the `package.json` to add scripts for running the TypeScript code:

```json
{
  "name": "express-typescript-api",
  "version": "1.0.0",
  "main": "dist/index.js",
  "scripts": {
    "build": "tsc",
    "start": "node dist/index.js",
    "dev": "ts-node src/index.ts"
  },
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "@types/express": "^4.17.12",
    "@types/node": "^14.14.31",
    "ts-node": "^9.1.1",
    "typescript": "^4.1.3"
  }
}
```

### Step 5: Run the Server

To run the server in development mode (using `ts-node`):

```bash
npm run dev
```

To build and run the server in production mode:

```bash
npm run build
npm start
```

### Final Project Structure

Your project structure should look like this:

```
express-typescript-api/
│
├── node_modules/
├── src/
│   ├── index.ts
│   └── routes/
│       └── index.ts
├── package.json
├── tsconfig.json
└── package-lock.json
```

### Conclusion

You now have a basic TypeScript-based Node.js REST API server using Express. You can expand upon this by adding more routes, controllers, middleware, and error handling as needed. This setup provides a solid foundation for building scalable and maintainable REST APIs with TypeScript and Express.
