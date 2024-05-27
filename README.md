BBBB    AAAAA   OOOOO   BBBB    AAAAA   OOOOO
B   B  A     A O     O B   B  A     A O     O
BBBB   AAAAAAA O     O BBBB   AAAAAAA O     O
B   B  A     A O     O B   B  A     A O     O
BBBB   A     A  OOOOO  BBBB   A     A  OOOOO


# Baobao NFT Contract

## Overview

The `Baobao` NFT contract is a robust and feature-rich implementation of the ERC721 standard, with additional functionality for royalties, access control, pausing, and burning. It is designed to be flexible and secure, allowing for easy management and interaction with NFTs.

## Features

1. **ERC721 Compliant**
   - Implements the ERC721 standard for Non-Fungible Tokens (NFTs).

2. **ERC721 Metadata Extension**
   - Supports metadata through the `tokenURI` function.

3. **ERC2981 Royalty Standard**
   - Implements the ERC2981 standard for NFT royalties, allowing creators to earn royalties on secondary sales.

4. **Access Control**
   - Uses OpenZeppelin's `AccessControl` for role-based access control.
   - Two roles: `DEFAULT_ADMIN_ROLE` and `NEW_ROLE`.

5. **Pausable**
   - Allows the contract to be paused and unpaused by an admin.
   - Pausing the contract prevents transfers and minting.

6. **Burnable**
   - Supports burning of tokens, reducing the total supply.

7. **Custom Base URI**
   - Allows setting a base URI for token metadata.

8. **Batch Role Assignment**
   - Admin can assign roles to multiple addresses.

9. **Minting**
   - Supports minting new tokens to a specified address.
   - Uses `safeMint` to ensure the receiving address can handle ERC721 tokens.

10. **Transfer**
    - Supports both `transferFrom` and `safeTransferFrom` functions for token transfers.
    - Restricted transfer functions to specific roles.

11. **Approval and Authorization**
    - Allows setting approval for individual tokens.
    - Allows setting operator approvals for managing all tokens of an owner.

12. **External Interfaces**
    - Interacts with external contracts for additional functionalities (`IBlast` and `IBlastPoints`).

13. **Gas Optimization**
    - Uses `immutable` for fixed contract addresses to save gas.
    - Uses `unchecked` blocks for arithmetic operations where overflow is not a concern.

## Usage

### Deployment

1. Deploy the contract with the required addresses for external interfaces:
   ```solidity
   constructor() AccessControlDefaultAdminRules(0, msg.sender) {
       IBlast(0xYourBlastContractAddress).configureClaimableGas();  // Replace with the actual address
       IBlast(0xYourBlastContractAddress).configureGovernor(msg.sender);  // Replace with the actual address
       IBlastPoints(0xYourBlastPointsContractAddress).configurePointsOperator(msg.sender);  // Replace with the actual address

       _name = "baobao";
       _symbol = "guai";
       
       _setupRole(DEFAULT_ADMIN_ROLE, msg.sender);
       _setupRole(NEW_ROLE, msg.sender);
   }

2. Role Management
    Assign Roles:
     function assignRoles(address account) public onlyRole(DEFAULT_ADMIN_ROLE) {
    grantRole(NEW_ROLE, account);
}

    Restricted Function:
     function restrictedFunction() public onlyRole(NEW_ROLE) {
    // function logic
}

3. Mint a New Token:
    function safeMint(address to) public onlyRole(NEW_ROLE) {
    _safeMint(to, tokenCounter);
    tokenCounter++;
}

4. Pause the Contract:
  function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {
    _pause();
}

function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {
    _unpause();
}

5. Set Royalty Info:
  function setRoyaltyInfo(uint256 tokenId, address recipient, uint96 fraction) public onlyRole(DEFAULT_ADMIN_ROLE) {
    _setTokenRoyalty(tokenId, recipient, fraction);
}

6. Set Base URI:
  function setBaseURI(string memory _baseURI) public onlyRole(DEFAULT_ADMIN_ROLE) {
    baseURI = _baseURI;
}

7. External Interfaces Replace the placeholders with actual addresses of your contracts:
  IBlast(0xYourBlastContractAddress).configureClaimableGas();
  IBlast(0xYourBlastContractAddress).configureGovernor(msg.sender);
  IBlastPoints(0xYourBlastPointsContractAddress).configurePointsOperator(msg.sender);
  


      
