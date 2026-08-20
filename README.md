# Experiment 7: AI-Powered Smart Contract for Decentralized Negotiation
# Name: Divya R V
# Reg no: 212223100005
# Aim:
To create a smart contract that integrates AI logic for automated negotiation in decentralized commerce. The contract adjusts price and conditions dynamically based on real-time market trends using an on-chain AI model.
# Algorithm:
Step 1: AI-Powered Dynamic Pricing
Seller lists an item with a minimum price and negotiation range.

Buyer submits an offer price.

AI logic (simulated using Solidity algorithms) evaluates the price based on:

Market demand (tracked using on-chain transactions).

Historical transaction data.

Time-based price fluctuations.

Step 2: Smart Contract Counteroffer
The contract automatically generates a counteroffer if the buyer’s price is within the negotiation range.

If the buyer accepts, the transaction is executed on-chain.

Step 3: Settlement and Price Learning
Every completed transaction updates the price learning algorithm to refine future pricing decisions.

# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract AIPoweredNegotiation {
    struct Item {
        address seller;
        uint256 minPrice;
        uint256 maxPrice;
        uint256 basePrice;
        bool sold;
    }

    mapping(uint256 => Item) public items;
    uint256 public itemCount;

    event ItemListed(uint256 itemId, uint256 basePrice);
    event OfferMade(uint256 itemId, address buyer, uint256 offer);
    event CounterOffer(uint256 itemId, uint256 counterOffer);
    event Sold(uint256 itemId, address buyer, uint256 finalPrice);

    function listItem(uint256 _basePrice, uint256 _minPrice, uint256 _maxPrice) public {
        require(_minPrice <= _basePrice && _basePrice <= _maxPrice, "Invalid price range");
        
        items[itemCount] = Item(msg.sender, _minPrice, _maxPrice, _basePrice, false);
        emit ItemListed(itemCount, _basePrice);
        itemCount++;
    }

    function makeOffer(uint256 _itemId, uint256 _offerPrice) public payable {
        Item storage item = items[_itemId];
        require(!item.sold, "Item already sold");
        require(msg.value == _offerPrice, "Incorrect offer amount");

        emit OfferMade(_itemId, msg.sender, _offerPrice);

        uint256 aiCounterOffer = dynamicPricing(item.basePrice, item.minPrice, item.maxPrice, _offerPrice);

        if (_offerPrice >= aiCounterOffer) {
            item.sold = true;
            payable(item.seller).transfer(_offerPrice);
            emit Sold(_itemId, msg.sender, _offerPrice);
        } else {
            payable(msg.sender).transfer(_offerPrice); // Refund buyer
            emit CounterOffer(_itemId, aiCounterOffer);
        }
    }

    function dynamicPricing(uint256 base, uint256 min, uint256 max, uint256 offer) private pure returns (uint256) {
        if (offer >= max) return max;
        if (offer >= base) return base;
        uint256 counter =(base+offer)/2;
        return counter < min ? min : counter; // Enforce a Minimum bound
        
    }
}
```
# Expected Output:
Buyers submit offers, and the contract auto-negotiates the price.

If the buyer’s offer is fair, the deal is executed.

If the offer is too low, the contract suggests a counteroffer.

High-Level Overview:
First-of-its-kind AI-powered pricing contract.

Mimics real-world price negotiations using dynamic on-chain pricing.

Can be extended to AI oracles for real-time market data.

Inspired by AI-enhanced commerce and eBay-like decentralized auctions.

# OUTPUT:
<img width="1920" height="1080" alt="Screenshot 2026-08-20 104058" src="https://github.com/user-attachments/assets/140604e0-6dde-4d39-b71a-d0102eeb3bf3" />

<img width="1920" height="1080" alt="Screenshot 2026-08-20 104122" src="https://github.com/user-attachments/assets/9edd857a-7d4d-4f62-b54f-beb9179ae10f" />

<img width="1920" height="1080" alt="Screenshot 2026-08-20 104342" src="https://github.com/user-attachments/assets/0e16216b-7746-42be-8b97-e26dcc9c7fd8" />

<img width="1920" height="1080" alt="Screenshot 2026-08-20 104353" src="https://github.com/user-attachments/assets/a136468d-495a-4056-b8a9-ab428aa48fa2" />

<img width="1920" height="1080" alt="Screenshot 2026-08-20 104427" src="https://github.com/user-attachments/assets/1263b74b-8288-4038-8de1-4a724a8cfe44" />

# RESULT:
