If you want to generate a random and unique string for each minted NFT without relying on an external source like Chainlink VRF, you can use a combination of the user's address, a timestamp, and some additional data as input to a hash function. This approach provides randomness based on on-chain data. Here's an example of how to do this in Solidity:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Enumerable.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Strings.sol";

contract MyNFT is ERC721Enumerable, Ownable {
    using Strings for uint256;

    // Base URI for token metadata
    string private baseTokenURI;

    // Counter for minted tokens
    uint256 public tokenIdCounter;

    constructor(string memory _name, string memory _symbol, string memory _baseTokenURI) ERC721(_name, _symbol) {
        baseTokenURI = _baseTokenURI;
        tokenIdCounter = 0;
    }

    // Mint a new NFT with a random unique string
    function mintNFT() public onlyOwner {
        uint256 tokenId = tokenIdCounter + 1;
        _mint(msg.sender, tokenId);
        tokenIdCounter++;

        // Generate a unique random string based on on-chain data
        string memory uniqueString = generateUniqueString(msg.sender, tokenId);

        // Set the token URI to include the unique string
        string memory tokenURI = string(abi.encodePacked(baseTokenURI, uniqueString));
        _setTokenURI(tokenId, tokenURI);
    }

    // Generate a unique random string based on on-chain data
    function generateUniqueString(address user, uint256 tokenId) internal view returns (string memory) {
        bytes32 hash = keccak256(abi.encodePacked(block.timestamp, user, tokenId));
        return hash.toHexString();
    }
}
```

In this example:

1. We inherit from `ERC721Enumerable` and `Ownable` to create an ERC-721 NFT contract with enumeration support and ownership control.

2. The `mintNFT` function allows the owner of the contract to mint an NFT. It generates a unique random string by calling the `generateUniqueString` function and sets the token URI to include this unique string.

3. The `generateUniqueString` function takes the user's address, the current timestamp, and the tokenId as input and hashes them together to create a unique random string.

4. You can set the `baseTokenURI` to a base URI for your token metadata. The unique string generated in `generateUniqueString` is appended to this base URI to create the final token URI.

This approach provides a pseudo-random and unique string for each minted NFT based on on-chain data, without relying on external randomness sources.
