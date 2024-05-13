To add NFT metadata to PolygonScan (now known as BscScan for Binance Smart Chain), you'll need to host your metadata files on a server accessible via HTTP(S). Here's a step-by-step guide on how to do this:

1. **Host Metadata Files**: Host your NFT metadata files (JSON files) on a server. Each JSON file should contain metadata for a single NFT token, including attributes like name, description, image URL, etc.

2. **Set Base URI in Contract**: In your ERC-1155 smart contract, set a base URI that points to the location where your metadata files are hosted. For example:

   ```solidity
   string public baseURI = "https://your-metadata-server.com/metadata/";
   ```

   Ensure that the base URI ends with a trailing slash (`/`).

3. **Generate Metadata URLs**: When minting NFTs, construct the metadata URI for each token by concatenating the base URI with the token ID. For example:

   ```solidity
   function tokenURI(uint256 tokenId) public view returns (string memory) {
       return string(abi.encodePacked(baseURI, tokenId.toString()));
   }
   ```

4. **Verify Contract on BscScan**: After deploying your ERC-1155 contract on the Binance Smart Chain, verify the contract on BscScan. This step is necessary to access the contract's metadata URLs from external platforms.

5. **Submit Metadata URLs to BscScan**: Once your contract is verified, you can submit your metadata URLs to BscScan using their Metadata Management service. This service allows you to register metadata URLs for your contract, enabling platforms like BscScan to fetch and display NFT metadata.

   - Go to BscScan Metadata Management: https://bscscan.com/metamaskinfo
   - Connect your wallet and select the contract you want to manage.
   - Enter the token ID and corresponding metadata URL for each NFT token.
   - Submit the metadata URLs.

6. **Test Metadata Display**: After submitting the metadata URLs, you can test if BscScan is correctly displaying the metadata for your NFTs. Navigate to the contract's page on BscScan and view the token details. The metadata should include the name, description, image, and other attributes specified in your JSON files.

By following these steps, you can add NFT metadata to PolygonScan/BscScan and ensure that your NFTs are displayed with their images and descriptions on the platform.
