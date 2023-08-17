Sure, here's how you can use ImageMagick in a Node.js TypeScript application to resize images to a specific size and upload them to the server in multiple sizes. We'll use the `sharp` library, which is a fast and efficient Node.js image processing library that utilizes ImageMagick under the hood.

1. **Set Up the Project:**

Create a new directory for your project, navigate into it, and initialize a Node.js project:

```bash
mkdir image-resize-upload
cd image-resize-upload
npm init -y
```

Install the required dependencies:

```bash
npm install sharp express multer fs
```

2. **Create an Upload Route:**

Create an `uploads` directory at the root of your project where you'll store the uploaded images.

Create a `server.ts` file and add the following code to set up an Express server to handle image uploads and resizing:

```typescript
import express from "express";
import multer from "multer";
import sharp from "sharp";
import fs from "fs";

const app = express();
const port = 3000;

// Configure multer to handle file uploads
const upload = multer({ dest: "uploads/" });

app.post("/upload", upload.single("image"), async (req, res) => {
  try {
    if (!req.file) {
      res.status(400).send("No image uploaded.");
      return;
    }

    // Resize and upload the image
    const imagePath = req.file.path;
    const sizes = [300, 600, 900]; // Define the sizes you want to resize to

    for (const size of sizes) {
      const resizedImageBuffer = await sharp(imagePath).resize(size).toBuffer();
      const resizedImagePath = `uploads/${size}-${req.file.filename}`;
      fs.writeFileSync(resizedImagePath, resizedImageBuffer);
    }

    res.send("Images uploaded and resized successfully.");
  } catch (error) {
    console.error("Error uploading images:", error);
    res.status(500).send("An error occurred while uploading images.");
  }
});

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```

3. **Run the Server:**

Run the server:

```bash
node server.js
```

4. **Use the API to Upload and Resize Images:**

You can use tools like `curl` or Postman to make a `POST` request to `http://localhost:3000/upload` with a `multipart/form-data` body containing an `image` field.

This example demonstrates how to upload an image, resize it to multiple sizes (300px, 600px, and 900px), and save them to the `uploads` directory.

Please note that this is a basic example. In a production environment, you might want to handle errors more gracefully, validate image types, and possibly use a storage solution like Amazon S3 for storing the images.
