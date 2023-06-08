Certainly! Here's an example of setting up a middleware pattern in a Next.js project using TypeScript:

1. Create a middleware function to handle the database connection:

```typescript
import { NextApiRequest, NextApiResponse } from "next";
import { MongoClient } from "mongodb";

export const withDatabase =
  (handler: any) => async (req: NextApiRequest, res: NextApiResponse) => {
    const client = new MongoClient(process.env.MONGODB_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true,
    });

    try {
      await client.connect();
      req.dbClient = client; // Attach the database client to the request object
      return handler(req, res);
    } catch (error) {
      console.error("Failed to connect to the database:", error);
      res.status(500).json({ message: "Internal Server Error" });
    } finally {
      await client.close();
    }
  };
```

2. Use the middleware function in your API route or handler:

```typescript
import { NextApiRequest, NextApiResponse } from "next";
import { withDatabase } from "../../path/to/middleware";

const exampleHandler = async (req: NextApiRequest, res: NextApiResponse) => {
  const collection = req.dbClient.db().collection("users");
  // Perform database operations...

  res.status(200).json({ message: "Success" });
};

export default withDatabase(exampleHandler);
```

In this example, the `withDatabase` middleware function wraps around the handler function. Inside the middleware function, a database connection is established using the `MongoClient` from the MongoDB package. The database client is attached to the `req` object to make it accessible within the handler function. The handler function can then access the database client and perform the necessary database operations.

By using the `withDatabase` middleware, you can ensure that the database connection is established and closed for each request. This centralizes the database connection logic and allows for reusability across different routes or handlers.

Make sure to adjust the code to fit your specific database technology and connection requirements.

Note that this example assumes you are using MongoDB as your database. If you are using a different database technology, you would need to modify the code accordingly.

Remember to handle error cases appropriately and add any necessary error handling or error response logic based on your specific application requirements.
