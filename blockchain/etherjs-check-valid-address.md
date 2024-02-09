In ethers.js, you can check whether a given string is a valid Ethereum address using the `isAddress()` function from the `utils` module. Here's how you can use it:

```javascript
const { ethers } = require("ethers");

function isValidAddress(address) {
  return ethers.utils.isAddress(address);
}

// Example usage
const address1 = "0x123abc..."; // Valid address
const address2 = "0x123"; // Invalid address

console.log(isValidAddress(address1)); // true
console.log(isValidAddress(address2)); // false
```

The `isAddress()` function returns `true` if the provided string is a valid Ethereum address and `false` otherwise.

Make sure to always validate addresses before interacting with them to avoid errors and potential vulnerabilities in your application.
