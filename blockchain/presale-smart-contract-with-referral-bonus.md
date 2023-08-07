To add a referral bonus logic to the presale smart contract, we can modify the existing contract to track referrals and reward participants who bring in new buyers. Below is the updated contract with the referral bonus logic:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

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

    // Referral bonus parameters
    uint256 public referralBonusPercentage; // Referral bonus percentage (e.g., 5%)
    mapping(address => address) public referrals; // Referral tracking

    // Participant contributions
    mapping(address => uint256) public contributions;

    // Presale status
    enum PresaleStatus { Active, Completed }
    PresaleStatus public status;

    // Events
    event TokensPurchased(address indexed buyer, uint256 amount);
    event ReferralBonus(address indexed referrer, address indexed referee, uint256 amount);

    // Constructor
    constructor(
        address _tokenAddress,
        uint256 _presaleStartTime,
        uint256 _presaleEndTime,
        uint256 _presaleTokenPrice,
        uint256 _presaleTokenSupply,
        uint256 _referralBonusPercentage
    ) {
        tokenAddress = _tokenAddress;
        presaleStartTime = _presaleStartTime;
        presaleEndTime = _presaleEndTime;
        presaleTokenPrice = _presaleTokenPrice;
        presaleTokenSupply = _presaleTokenSupply;
        referralBonusPercentage = _referralBonusPercentage;
        status = PresaleStatus.Active;
    }

    modifier presaleActive() {
        require(
            block.timestamp >= presaleStartTime && block.timestamp <= presaleEndTime,
            "Presale is not active"
        );
        require(status == PresaleStatus.Active, "Presale has been completed");
        _;
    }

    function buyTokens(address referrer) external payable presaleActive {
        require(msg.value > 0, "You need to send some ether");

        uint256 tokensToBuy = (msg.value * 1e18) / presaleTokenPrice;

        require(tokensSold + tokensToBuy <= presaleTokenSupply, "Not enough tokens left for purchase");

        // Transfer tokens to the buyer
        IERC20 tokenContract = IERC20(tokenAddress);
        tokenContract.transfer(msg.sender, tokensToBuy);

        // Update the tokensSold and contributions
        tokensSold += tokensToBuy;
        contributions[msg.sender] += msg.value;

        if (referrer != address(0) && referrer != msg.sender) {
            // Calculate the referral bonus amount
            uint256 referralBonusAmount = (msg.value * referralBonusPercentage) / 100;
            if (referralBonusAmount > 0) {
                // Transfer the referral bonus to the referrer
                payable(referrer).transfer(referralBonusAmount);
                emit ReferralBonus(referrer, msg.sender, referralBonusAmount);
            }
            // Record the referral
            referrals[msg.sender] = referrer;
        }

        emit TokensPurchased(msg.sender, tokensToBuy);
    }

    function withdrawFunds() external {
        require(block.timestamp > presaleEndTime, "Presale is still active");
        require(status == PresaleStatus.Active, "Presale has already been completed");

        status = PresaleStatus.Completed;

        // Transfer the raised funds to the contract owner
        payable(msg.sender).transfer(address(this).balance);
    }
}
```

In this updated contract, we've added a `referralBonusPercentage` variable to define the percentage of the purchase amount that will be given as a referral bonus. When a participant buys tokens and provides a referrer address (different from their own address), they become eligible for a referral bonus.

If the participant is referred by someone, the contract calculates the referral bonus amount and transfers it to the referrer's address. The referral is recorded in the `referrals` mapping, associating the buyer's address with the referrer's address.

Keep in mind that the `referralBonusPercentage` is an integer value representing the percentage, e.g., 5 for 5%, 10 for 10%, etc. It should be carefully set to avoid excessive rewards that could harm the presale's token distribution. Additionally, consider adding proper validation and security measures to handle edge cases and potential exploits.
