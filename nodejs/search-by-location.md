To implement a search by location radius using Google Maps API with Node.js and TypeScript, you can follow these steps:

1. **Set Up Project**:
   Create a new directory for your project and initialize it with Node.js and TypeScript.

   ```bash
   mkdir location-search
   cd location-search
   npm init -y
   npm install typescript @types/node axios
   ```

2. **Create TypeScript Configuration**:
   Create a `tsconfig.json` file in your project directory to configure TypeScript.

   ```json
   {
     "compilerOptions": {
       "target": "ES6",
       "module": "commonjs",
       "outDir": "./dist",
       "rootDir": "./src",
       "strict": true
     }
   }
   ```

3. **Create Source Files**:
   Inside the `src` directory, create the necessary TypeScript files.

   - `src/main.ts`: Main entry point to run the application.
   - `src/googleMaps.ts`: Google Maps API integration for location search.

4. **Google Maps API Integration** (`src/googleMaps.ts`):

```typescript
import axios from "axios";

const GOOGLE_MAPS_API_KEY = "YOUR_GOOGLE_MAPS_API_KEY";

export async function searchLocationsByRadius(
  latitude: number,
  longitude: number,
  radiusMeters: number
): Promise<any> {
  const response = await axios.get(
    `https://maps.googleapis.com/maps/api/place/nearbysearch/json?location=${latitude},${longitude}&radius=${radiusMeters}&key=${GOOGLE_MAPS_API_KEY}`
  );
  return response.data;
}
```

5. **Main Application Logic** (`src/main.ts`):

```typescript
import { searchLocationsByRadius } from "./googleMaps";

async function main() {
  const latitude = 37.7749; // Example latitude
  const longitude = -122.4194; // Example longitude
  const radiusMeters = 1000; // Example radius in meters

  try {
    const locations = await searchLocationsByRadius(
      latitude,
      longitude,
      radiusMeters
    );
    console.log("Locations within the radius:", locations.results);
  } catch (error) {
    console.error("Error:", error.message);
  }
}

main();
```

6. **Run the Application**:
   Compile the TypeScript code and run the application.

   ```bash
   npx tsc
   node dist/main.js
   ```

Replace `'YOUR_GOOGLE_MAPS_API_KEY'` with your actual Google Maps API key in the `src/googleMaps.ts` file.

This example demonstrates a basic search by location radius using the Google Places API. You can further enhance this by adding error handling, additional query parameters, and parsing the response data as needed. Make sure to refer to the Google Places API documentation for more details on available parameters and responses.
