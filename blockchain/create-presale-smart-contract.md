Creating a complete presale smart contract for an ERC-20 token called "Kongcoin" is a complex task that involves multiple components. Below, I'll provide you with a simplified version of the presale contract that you can use as a starting point. Please note that this code is meant for educational purposes, and you should conduct a thorough security audit before deploying it to the Ethereum mainnet or any other network.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Import the ERC-20 token interface
import "./IERC20.sol";

contract KongcoinPresale {
    // Token contract address
    address public tokenAddress;

    // Presale parameters
    uint256 public presaleStartTime;
    uint256 public presaleEndTime;
    uint256 public presaleTokenPrice;
    uint256 public presaleTokenSupply;
    uint256 public tokensSold;

    // Participant contributions
    mapping(address => uint256) public contributions;

    // Presale status
    enum PresaleStatus { Active, Completed }
    PresaleStatus public status;

    // Events
    event TokensPurchased(address indexed buyer, uint256 amount);

    // Constructor
    constructor(
        address _tokenAddress,
        uint256 _presaleStartTime,
        uint256 _presaleEndTime,
        uint256 _presaleTokenPrice,
        uint256 _presaleTokenSupply
    ) {
        tokenAddress = _tokenAddress;
        presaleStartTime = _presaleStartTime;
        presaleEndTime = _presaleEndTime;
        presaleTokenPrice = _presaleTokenPrice;
        presaleTokenSupply = _presaleTokenSupply;
        status = PresaleStatus.Active;
    }

    // Modifier to check if the presale is active
    modifier presaleActive() {
        require(
            block.timestamp >= presaleStartTime && block.timestamp <= presaleEndTime,
            "Presale is not active"
        );
        require(status == PresaleStatus.Active, "Presale has been completed");
        _;
    }

    // Function to participate in the presale
    function buyTokens() external payable presaleActive {
        require(msg.value > 0, "You need to send some ether");

        uint256 tokensToBuy = (msg.value * 1e18) / presaleTokenPrice; // Assuming ERC-20 token has 18 decimals

        require(tokensSold + tokensToBuy <= presaleTokenSupply, "Not enough tokens left for purchase");

        // Transfer tokens to the buyer
        IERC20 tokenContract = IERC20(tokenAddress);
        tokenContract.transfer(msg.sender, tokensToBuy);

        // Update the tokensSold and contributions
        tokensSold += tokensToBuy;
        contributions[msg.sender] += msg.value;

        emit TokensPurchased(msg.sender, tokensToBuy);
    }

    // Function to withdraw the raised funds by the contract owner after the presale ends
    function withdrawFunds() external {
        require(block.timestamp > presaleEndTime, "Presale is still active");
        require(status == PresaleStatus.Active, "Presale has already been completed");

        status = PresaleStatus.Completed;

        // Transfer the raised funds to the contract owner
        payable(msg.sender).transfer(address(this).balance);
    }
}
```

Before deploying this contract, make sure to follow these steps:

1. Prepare the ERC-20 Token Contract: Deploy an ERC-20 token contract (called Kongcoin) separately if you haven't done so already. You'll need the address of this contract to use it in the presale contract.

2. Set the Presale Parameters: Decide on the presale start time, end time, token price, and the total number of tokens available for the presale. Pass these parameters to the constructor when deploying the presale contract.

3. Deploy the KongcoinPresale Contract: Deploy the presale contract by providing the necessary parameters, including the address of the Kongcoin ERC-20 token contract.

4. Participants Purchase Tokens: Participants can participate in the presale by calling the `buyTokens()` function and sending ether to the contract. The contract will automatically calculate the number of tokens they'll receive based on the presale token price.

5. Withdraw Funds: After the presale ends, the contract owner can withdraw the raised funds by calling the `withdrawFunds()` function.

Please exercise caution when dealing with smart contracts and real funds. It is essential to perform thorough testing and security audits to ensure the contract's correctness and robustness. Additionally, consider using a multi-signature wallet or other secure means to manage the contract's funds and ownership.
