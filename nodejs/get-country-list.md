To create a country code map in Node.js that maps country names to their alpha-2 code and alpha-3 code, you can utilize the `countries-and-timezones` package to fetch the required data. Here's how you can do it:

1. Set up a new Node.js project and install the required package:

```bash
npm init -y
npm install countries-and-timezones
```

2. Create a JavaScript file (e.g., `countryCodes.js`) and add the code to create the country code map:

```js
// countryCodes.js

const countriesAndTimezones = require("countries-and-timezones");

// Function to create the country code map
const createCountryCodeMap = () => {
  const countryCodeMap = {};

  // Get all countries with their timezones from the package
  const allCountries = countriesAndTimezones.getAllCountries();

  // Loop through each country and extract the required data
  Object.keys(allCountries).forEach((countryCode) => {
    const countryData = allCountries[countryCode];
    countryCodeMap[countryData.name] = {
      alpha2: countryCode,
      alpha3: countryData.alpha3,
    };
  });

  return countryCodeMap;
};

// Call the function to create the country code map
const countryCodes = createCountryCodeMap();

console.log(countryCodes);
```

3. Run the script to generate the country code map:

```bash
node countryCodes.js
```

The script will generate a map that contains country names as keys and an object with `alpha2` and `alpha3` codes as values. For example:

```json
{
  "Afghanistan": {
    "alpha2": "AF",
    "alpha3": "AFG"
  },
  "Albania": {
    "alpha2": "AL",
    "alpha3": "ALB"
  },
  ...
}
```

With this map, you can easily look up the alpha-2 and alpha-3 codes of a country based on its name. For example, to get the alpha-2 code of "United States," you would access `countryCodes['United States'].alpha2`, which would return `"US"`. Similarly, to get the alpha-3 code, you would use `countryCodes['United States'].alpha3`, which would return `"USA"`.
