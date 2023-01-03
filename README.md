# Your-Life-Game
P2E Sport Game

# YLVAULT CONTRACT: 

**Substorage Fabric to create and asociate subvaults to wallets, where the users can deposit their ERC721(NFTs) & ERC1155(Boosters)**
    
    - Deposit function for storing ERC-721 in the subVault & and creating a new subvault if they user did not create one before.
    - Deposit function for storing ERC-1155 in the subVault & and creating a new subvault if they user did not create one before.
    - Setter for NFTWithdraw fee.
    - Setter for adding a new Sport and the players needed.
    - Setter for updating the amount of NFTs of each user has per Sport Category. HOW DO WE MAKE SURE THE USER IS NOT CHOOSING THE WRONG CATEGORY?
    - Getter for the subvault of the wallet address passed.
    - Getter for checking if the user is elegible for an specific category.
    - Getter for the NFTWithdraw fee.
     EVENTS: 
     - event RevertNftToWalletCommissionSetted(uint256 SettedFee, uint256 SettedTime);
     - event DepositedNftFromWalletToVaultERC721(address FromAddress, address GamerAddress, address VaultAddress, uint256 TokenId, uint256 DepositedTime);
     - event DepositedNftFromWalletToVaultERC1155(address FromAddress, address GamerAddress, address VaultAddress, uint256 TokenId, uint256 Amount, uint256 DepositedTime);


# VAULT CONTRACT:
**Substorage for keeping user´s ERC721(NFTs) & ERC1155(Boosters).**
    
    - Function to withdraw/revert the owner´s ERC721(NFTs) & ERC1155(Boosters) at any time, but they have to pay a fee.
    - Function to burn ERC1155. Option for Boosters.

     EVENTS:
        event RevertTransferNftFromVaultToWalletERC721(address VaultAddress, address GamerAddress, uint256 NFTID, uint256 FeeAmount, uint256 RevertedTime);
        event RevertTransferNftFromVaultToWalletERC1155(address VaultAddress, address GamerAddress, uint256 NFTID, uint256 Amount, uint256 FeeAmount, uint256 RevertedTime);
        event BoosterBurned(address VaultAddress, address GamerAddress, uint256 BoosterID, uint256 Amount, uint256 BurnedTime);
        event feePerNFTUpdated(uint NewFee, uint UpdatedTime);


# CONTEST CONTRACT:
**This contract tracks the game results and offerts the game logic, user can pay tournament fees and check if the player paid for the tournament fees.**
    
    - Function to play (ONLY OWNER). Requires the next parameters: players Address, each players final score, CategoryID, TournamentID (if needed). It will generate 2 random number to add to each player´s final score.
    - Funtion to pay the tournament fee in YLT. (It will set the msg.sender as elegible for the tournament selected)    
    - Function to WITHDRAW the funds in the contract (ONLY OWNER).
    - Setter for the tournament fee.
    - Setter for the minimum YLT staked to play.
    - Getter for checking if the address is elegible to play (Based on the YLT tokens staked). Useful for checking elegibility before matching players.
    - Getter for the minimum to stake to be elegible to play.
    - Getter for the tournament fee. (Requires the Tournament ID)
    - Getter for the Match details. (Requires the Category & Match ID)
    - Getter for the player record of victories, looses, drawns. (Requires user wallet)

     EVENTS:
        event MatchFinished(address Winner, uint8 Category, address Looser, uint MatchID, uint SettedTime);
        event TournamentCommissionSetted(uint SettedFee, uint SettedTime); 
        event TournamentFeePaid(address Player, uint TournamentID, uint SettedTime);
        event MinTokensStakedPlayUpdated(uint MinYLTStaked, uint SettedTime);
        
        
 # EXTRA ADDED TO OTHER CONTRACTS
 
 **YLproxy.sol**
 
 Getter for the total YLT staked by address & contract.
 ```
 function totalStakedAmount(address _user, address _contract) external view returns(uint){
        return stakedAmount[_user][_contract];
    }
```
 
 **YLNFT.sol**
 
 Getter for the NFT category
 ```
   mapping(uint => string) private categoryByID;
    
 ``` 
 ``` 
 // setter added to createToken():  

 categoryByID[newTokenId] = _sport;
    
 ```  
 ``` 
   function getCategory(uint _tokenId) external view returns(string memory){
        return categoryByID[_tokenId];
    }
 
 ``` 
 
 added Market place addres 2 and it was not compiling due bytes. Optimized.


# ProxyYLT 

Constructor modified to add YLT address.

# ERC1155 

Constructor modified to add Market 1155 contract by function.

# ERC721 

Constructor modified to add Market 721 contract by function.
added function to add the address of the second market place contract.

# Auction contract 

Constructor modified to add 2 marketplace for ERC721
functions adjusted to select the contract that includes the function.


# Market place NFT 1

Function fetchMyPausedItems adjusted, as it was pointing to the ERC1155 too.
Optimized, as it was passing the allowed gas! Warning: Contract code size is 25317 bytes and exceeds 24576 bytes (a limit introduced in Spurious Dragon). This contract may not be deployable on Mainnet. Consider enabling the optimizer (with a low "runs" value!), turning off revert strings, or using libraries.
  --> contracts/YLNFTMarketplace1.sol:21:1:
  
  I had to remove the event RevertDepositFromWalletToTeams(), TransferedNFTfromVaulttoMarket(), TransferedNFTfromMarkettoVault(), UserlistedNFTtoMarket() & RevertDepositFromWalletToMarket() from all the functiones to do not exceed the contract bytes.
and there were some unused events that I removed too MarketPerCommissionSet(), EscrowTransferFundsToSeller().

# Market place ERC1155

Optimized and toggle funcion created for: adminPauseUnpause(). Removed his events.

NOT DONE (I THINK): 
Fixed functions changePrice() & buyAdminListedNFT() & buyUserListedNFT(), adminUnlistedNFT(), finishAuction(), auctionInfo(), onERC1155Received(), onERC1155BatchReceived(), supportsInterface(). setVaultAddress() and setVaultAddress() without owner restriction.
