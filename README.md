# Exploring the Base Ecosystem

Base is designed to make onchain development more accessible.  

This repository will contain notes, resources, and experiments as I learn.

### Key Benefits of Base

- Very low transaction costs  
- Fast block times  
- Growing ecosystem  
- Strong developer support  

These are the main reasons I’m focusing on Base.

### Tools I’m Starting With

- Coinbase Wallet or MetaMask  
- Base network configuration  
- Block explorer  
- Official documentation  

### Adding Increment Function

```solidity
function increment() public {
    count += 1;
}

### Decrement Function

```solidity
function decrement() public {
    count -= 1;
}

### CountChanged Event

```solidity
event CountChanged(address indexed user, uint256 newCount);

function increment() public {
    count += 1;
    emit CountChanged(msg.sender, count);
}

### Withdraw Function

```solidity
function withdraw(uint256 amount) public {
    require(balances[msg.sender] >= amount, "Insufficient balance");
    balances[msg.sender] -= amount;
    payable(msg.sender).transfer(amount);
}
### Constructor for Counter

```solidity
constructor(uint256 startingCount) {
    count = startingCount;
}

### Protecting Functions

```solidity
function resetCount() public onlyOwner {
    count = 0;
}
### Renounce Ownership

```solidity
function renounceOwnership() public onlyOwner {
    owner = address(0);
}

### Total Deposits Tracking

```solidity
uint256 public totalDeposits;

function deposit() public payable {
    balances[msg.sender] += msg.value;
    totalDeposits += msg.value;
}

### Custom Error Messages

Clear error messages make debugging much easier:

```solidity
require(msg.sender == owner, "Only owner can call this function");

### Valid Amount Modifier

```solidity
modifier validAmount(uint256 amount) {
    require(amount > 0, "Amount must be greater than zero");
    _;
}

### Using block.timestamp

```solidity
uint256 public lastUpdate;

function update() public {
    lastUpdate = block.timestamp;
}

### Receive Function

```solidity
receive() external payable {
    balances[msg.sender] += msg.value;
}

### Receive Function

```solidity
receive() external payable {
    balances[msg.sender] += msg.value;
}

### Logging in Fallback

```solidity
event FallbackCalled(address sender, uint256 value);

fallback() external payable {
    emit FallbackCalled(msg.sender, msg.value);
}

### Contract Address

```solidity
function getContractAddress() public view returns (address) {
    return address(this);
}

### Updating Struct Values

```solidity
function updateBalance(uint256 newBalance) public {
    users[msg.sender].balance = newBalance;
}

### Accessing Array Elements

```solidity
function getUser(uint256 index) public view returns (address) {
    require(index < userList.length, "Index out of bounds");
    return userList[index];
}

### While Loop Example

```solidity
function countDown(uint256 n) public pure returns (uint256) {
    while (n > 0) {
        n--;
    }
    return n;
}

### Multiple Conditions

```solidity
function checkAccess(uint256 amount) public view returns (bool) {
    if (msg.sender == owner && amount > 0) {
        return true;
    }
    return false;
}

### Reading Enum Values

```solidity
function isActive() public view returns (bool) {
    return currentStatus == Status.Active;
}

### Calling Another Contract

```solidity
function readValue(address target) public view returns (uint256) {
    return IExample(target).getValue();
}

### Simple ERC20 Structure

```solidity
mapping(address => uint256) public balanceOf;
mapping(address => mapping(address => uint256)) public allowance;
uint256 public totalSupply;

### Approve Function

```solidity
function approve(address spender, uint256 amount) public returns (bool) {
    allowance[msg.sender][spender] = amount;
    return true;
}

### Decimals

```solidity
uint8 public decimals = 18;

### Token Constructor

```solidity
constructor(uint256 initialSupply) {
    totalSupply = initialSupply;
    balanceOf[msg.sender] = initialSupply;
    owner = msg.sender;
}

### Simple NFT Mint

```solidity
function mint() public {
    ownerOf[nextTokenId] = msg.sender;
    balanceOf[msg.sender] += 1;
    nextTokenId++;
}

### tokenURI Idea

```solidity
mapping(uint256 => string) public tokenURI;

function setTokenURI(uint256 tokenId, string memory uri) public {
    require(ownerOf[tokenId] == msg.sender, "Not the owner");
    tokenURI[tokenId] = uri;
}

### transferFrom for NFT

```solidity
function transferFrom(address from, address to, uint256 tokenId) public {
    require(ownerOf[tokenId] == from, "Wrong owner");
    require(
        msg.sender == from || msg.sender == getApproved[tokenId],
        "Not authorized"
    );

    ownerOf[tokenId] = to;
    balanceOf[from] -= 1;
    balanceOf[to] += 1;
    delete getApproved[tokenId];
}


### Exists Function

```solidity
function exists(uint256 tokenId) public view returns (bool) {
    return ownerOf[tokenId] != address(0);
}

### Max Supply

```solidity
uint256 public maxSupply = 1000;

function mint() public payable {
    require(nextTokenId < maxSupply, "Max supply reached");
    require(msg.value >= mintPrice, "Insufficient payment");
    ownerOf[nextTokenId] = msg.sender;
    balanceOf[msg.sender] += 1;
    nextTokenId++;
}

### Pause and Unpause Functions

```solidity
function pause() public onlyOwner {
    paused = true;
}

function unpause() public onlyOwner {
    paused = false;
}

### Remaining Supply

```solidity
function remainingSupply() public view returns (uint256) {
    return maxSupply - nextTokenId;
}

### Set Max Per Transaction

```solidity
function setMaxPerTx(uint256 newMax) public onlyOwner {
    maxPerTx = newMax;
}

### Get Mint Info

```solidity
function getMintInfo(uint256 tokenId) public view returns (address owner, uint256 mintedTime) {
    return (ownerOf[tokenId], mintedAt[tokenId]);
}

### Only Minter Modifier Idea

```solidity
modifier onlyMinter(uint256 tokenId) {
    require(originalMinter[tokenId] == msg.sender, "Not the original minter");
    _;
}

### Max Level

```solidity
uint256 public maxLevel = 10;

function levelUp(uint256 tokenId) public {
    require(ownerOf[tokenId] == msg.sender, "Not the owner");
    require(tokenLevel[tokenId] < maxLevel, "Max level reached");
    tokenLevel[tokenId] += 1;
}

### Get Token Stats

```solidity
function getTokenStats(uint256 tokenId) public view returns (
    address owner,
    uint256 level,
    uint256 exp
) {
    return (
        ownerOf[tokenId],
        tokenLevel[tokenId],
        experience[tokenId]
    );
}

### Using Strings Library

To convert `uint256` to `string`, you can use OpenZeppelin’s `Strings` library:

```solidity
import "@openzeppelin/contracts/utils/Strings.sol";

### Common Interface IDs

- ERC165 → `0x01ffc9a7`  
- ERC721 → `0x80ac58cd`  
- ERC721Metadata → `0x5b5e139f`  
- ERC721Enumerable → `0x780e9d63`  

These are standard identifiers.

### Collection Info View

```solidity
function getCollectionInfo() public view returns (
    string memory collectionName,
    string memory collectionSymbol,
    uint256 minted,
    uint256 max
) {
    return (name, symbol, nextTokenId, maxSupply);
}

### Improving Transfer Checks

```solidity
function transferFrom(address from, address to, uint256 tokenId) public {
    require(to != address(0), "Cannot transfer to zero address");
    require(ownerOf[tokenId] == from, "Wrong owner");
    require(
        msg.sender == from || msg.sender == getApproved[tokenId],
        "Not authorized"
    );
    // rest of the logic
}

### Prevent Transfer When Locked

```solidity
function transferFrom(address from, address to, uint256 tokenId) public {
    require(tokenStatus[tokenId] != TokenStatus.Locked, "Token is locked");
    // rest of transfer logic
}

### Get Token Status

```solidity
function getTokenStatus(uint256 tokenId) public view returns (TokenStatus) {
    return tokenStatus[tokenId];
}

### Calculating Staking Time

```solidity
function getStakingTime(uint256 tokenId) public view returns (uint256) {
    require(isStaked[tokenId], "Not staked");
    return block.timestamp - stakedAt[tokenId];
}

### Reward Claimed Event

```solidity
event RewardClaimed(address indexed user, uint256 indexed tokenId, uint256 amount);

function claimReward(uint256 tokenId) public {
    // existing logic...
    emit RewardClaimed(msg.sender, tokenId, reward);
}

### Reward Configuration Event

```solidity
event RewardPerDayUpdated(uint256 newReward);

function setRewardPerDay(uint256 newReward) public onlyOwner {
    rewardPerDay = newReward;
    emit RewardPerDayUpdated(newReward);
}

### Can Unstake View

```solidity
function canUnstake(uint256 tokenId) public view returns (bool) {
    if (!isStaked[tokenId]) return false;
    return block.timestamp >= stakedAt[tokenId] + minStakeTime;
}

### Penalty Applied Event

```solidity
event EarlyUnstakePenaltyApplied(uint256 indexed tokenId, uint256 penaltyAmount);

// Emit this event when a penalty is applied during unstake

### Set Max Staked Per User

```solidity
function setMaxStakedPerUser(uint256 newMax) public onlyOwner {
    maxStakedPerUser = newMax;
}

### Get Staked Tokens Count

```solidity
function getStakedTokensCount(address user) public view returns (uint256) {
    return userStakedTokens[user].length;
}

### Get Global Stats

```solidity
function getGlobalStats() public view returns (
    uint256 minted,
    uint256 max,
    uint256 currentlyStaked
) {
    return (nextTokenId, maxSupply, totalStaked);
}

### Rewards Stats Event

```solidity
event RewardsDistributed(uint256 totalDistributed);

// You can emit this occasionally or after important claims

### Staking Pause Event

```solidity
event StakingPauseChanged(bool isPaused);

function toggleStakingPause() public onlyOwner {
    stakingPaused = !stakingPaused;
    emit StakingPauseChanged(stakingPaused);
}

### Checks-Effects-Interactions Pattern

Recommended order inside functions:

1. **Checks** (require statements)  
2. **Effects** (update state variables)  
3. **Interactions** (external calls / transfers)  

This greatly reduces reentrancy risks.

### Possible Next Features

Ideas to continue improving the contract:

- Breeding system  
- Merging NFTs  
- Seasonal rewards  
- On-chain traits / randomness  
- Marketplace listing functions  
- Governance power based on NFTs  

### Breeding Event

```solidity
event Bred(address indexed owner, uint256 parent1, uint256 parent2, uint256 childId);

// Emit after successfully creating the new NFT

### Breeding Cost Event

```solidity
event BreedingCostUpdated(uint256 newCost);

function setBreedingCost(uint256 newCost) public onlyOwner {
    breedingCost = newCost;
    emit BreedingCostUpdated(newCost);
}

### Get Attributes

```solidity
function getAttributes(uint256 tokenId) public view returns (uint8 strength, uint8 agility, uint8 intelligence) {
    Attributes memory attr = tokenAttributes[tokenId];
    return (attr.strength, attr.agility, attr.intelligence);
}

### Get Power Score

```solidity
function getPowerScore(uint256 tokenId) public view returns (uint256) {
    Attributes memory attr = tokenAttributes[tokenId];
    return uint256(attr.strength) + attr.agility + attr.intelligence + tokenLevel[tokenId];
}

### Get Token Rarity Info

```solidity
function getTokenRarityInfo(uint256 tokenId) public view returns (
    string memory rarity,
    uint256 powerScore
) {
    return (getRarity(tokenId), getPowerScore(tokenId));
}

### Get Player Score

```solidity
function getPlayerScore(address player) public view returns (uint256) {
    return playerScore[player];
}

### Buy Token Function

```solidity
function buyToken(uint256 tokenId) public payable {
    Listing memory item = listings[tokenId];
    require(item.active, "Listing not active");
    require(msg.value >= item.price, "Insufficient payment");
    
    listings[tokenId].active = false;
    
    // Transfer ownership
    address seller = item.seller;
    ownerOf[tokenId] = msg.sender;
    balanceOf[seller] -= 1;
    balanceOf[msg.sender] += 1;
    
    payable(seller).transfer(item.price);
}

### Sale Event

```solidity
event TokenSold(
    uint256 indexed tokenId,
    address indexed seller,
    address indexed buyer,
    uint256 price
);

// Emit this event after a successful purchase

### Get Listing Info

```solidity
function getListingInfo(uint256 tokenId) public view returns (
    address seller,
    uint256 price,
    bool active
) {
    Listing memory item = listings[tokenId];
    return (item.seller, item.price, item.active);
}

### Cancel Offer

```solidity
function cancelOffer(uint256 tokenId) public {
    Offer memory offer = offers[tokenId];
    require(offer.buyer == msg.sender, "Not the buyer");
    require(offer.active, "Offer not active");

    offers[tokenId].active = false;
    payable(msg.sender).transfer(offer.price);
}

### Handling Multiple Offers (Idea)

The current structure only stores one offer per token.  

A more advanced version could use:

```solidity
mapping(uint256 => Offer[]) public tokenOffers;

### Royalty Info View

```solidity
function getRoyaltyInfo(uint256 salePrice) public view returns (address receiver, uint256 royaltyAmount) {
    royaltyAmount = (salePrice * royaltyPercentage) / 100;
    return (royaltyReceiver, royaltyAmount);
}
### Royalty Best Practices

- Keep royalties between 2.5% and 10%  
- Use ERC2981 for better compatibility  
- Allow the owner to update the receiver  
- Clearly document the royalty policy  

### Next Learning Steps

Possible next topics to explore:

- Using OpenZeppelin contracts  
- Upgradeable contracts (Proxies)  
- Chainlink VRF for randomness  
- ERC20 + NFT interaction  
- Governance with NFTs  
- Frontend integration with wagmi / viem / ethers  

### OpenZeppelin Minting Best Practice

Prefer using `_safeMint` instead of `_mint`.

`_safeMint` checks if the receiver is a contract and can handle ERC721 tokens correctly.

### Protecting Functions

```solidity
function safeMint(address to, string memory uri) public onlyOwner whenNotPaused {
    uint256 tokenId = nextTokenId;
    nextTokenId++;
    _safeMint(to, tokenId);
    _setTokenURI(tokenId, uri);
}

### supportsInterface with Royalties

```solidity
function supportsInterface(bytes4 interfaceId)
    public
    view
    override(ERC721URIStorage, ERC2981)
    returns (bool)
{
    return super.supportsInterface(interfaceId);
}
### Admin Functions

```solidity
function setMintPrice(uint256 newPrice) public onlyOwner {
    mintPrice = newPrice;
}

function setMaxSupply(uint256 newMax) public onlyOwner {
    require(newMax >= nextTokenId, "Cannot reduce below current supply");
    maxSupply = newMax;
}

function pause() public onlyOwner {
    _pause();
}

function unpause() public onlyOwner {
    _unpause();
}

### Set Max Per Wallet

```solidity
function setMaxPerWallet(uint256 newMax) public onlyOwner {
    maxPerWallet = newMax;
}

### Allowlist Event

```solidity
event AllowlistUpdated(address indexed user, bool status);

function setAllowlist(address user, bool status) public onlyOwner {
    allowlist[user] = status;
    emit AllowlistUpdated(user, status);
}

### Advantages of Merkle Allowlist

- Much cheaper gas costs  
- Can support thousands of addresses  
- Only the root is stored on-chain  
- Widely used in real NFT projects  

### Set Hidden URI

```solidity
function setHiddenURI(string memory _hiddenURI) public onlyOwner {
    hiddenURI = _hiddenURI;
}

### Pre-Reveal Experience

Before the reveal, all NFTs usually show the same placeholder image and description.

After the reveal, each token gets its unique traits and artwork.



### Gas Considerations of Enumerable

`ERC721Enumerable` is very convenient but more expensive in gas.

For large collections, some projects prefer not to use it and instead index events off-chain.

### Batch Mint Gas Warning

Even with a limit, batch minting can be expensive.

Always test gas usage before using large batches on mainnet.

### Phase Change Event

```solidity
event PhaseChanged(MintPhase newPhase);

function setPhase(MintPhase newPhase) public onlyOwner {
    currentPhase = newPhase;
    emit PhaseChanged(newPhase);
}
### Set Reserved Supply

```solidity
function setReservedSupply(uint256 newReserved) public onlyOwner {
    require(newReserved >= reservedMinted, "Cannot be below already minted");
    reservedSupply = newReserved;
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/ReentrancyGuard.sol";

contract MarketplaceNFT is ERC721URIStorage, Ownable, ReentrancyGuard {
    uint256 public nextTokenId;
    uint256 public mintPrice = 0.02 ether;
    uint256 public maxSupply = 300;
    uint256 public marketplaceFee = 2; // 2%

    struct Listing {
        address seller;
        uint256 price;
        bool active;
    }

    mapping(uint256 => Listing) public listings;

    constructor() ERC721("Marketplace NFT", "MNFT") Ownable(msg.sender) {}

    function mint(string memory uri) external payable nonReentrant {
        require(nextTokenId < maxSupply, "Max supply reached");
        require(msg.value >= mintPrice, "Insufficient payment");

        uint256 tokenId = nextTokenId++;
        _safeMint(msg.sender, tokenId);
        _setTokenURI(tokenId, uri);
    }

    function listToken(uint256 tokenId, uint256 price) external {
        require(ownerOf(tokenId) == msg.sender, "Not owner");
        require(price > 0, "Invalid price");
        listings[tokenId] = Listing(msg.sender, price, true);
    }

    function buyToken(uint256 tokenId) external payable nonReentrant {
        Listing memory item = listings[tokenId];
        require(item.active, "Not listed");
        require(msg.value >= item.price, "Insufficient payment");

        listings[tokenId].active = false;

        uint256 fee = (item.price * marketplaceFee) / 100;
        uint256 sellerAmount = item.price - fee;

        _transfer(item.seller, msg.sender, tokenId);
        payable(item.seller).transfer(sellerAmount);
    }

    function withdraw() external onlyOwner {
        payable(owner()).transfer(address(this).balance);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract LockedNFT is ERC721URIStorage, Ownable {
    uint256 public nextTokenId;
    uint256 public maxSupply = 300;
    uint256 public lockDuration = 30 days;

    mapping(uint256 => uint256) public unlockTime;

    constructor() ERC721("Locked NFT", "LOCK") Ownable(msg.sender) {}

    function mint(address to, string memory uri) external onlyOwner {
        require(nextTokenId < maxSupply, "Max supply reached");
        uint256 tokenId = nextTokenId++;
        unlockTime[tokenId] = block.timestamp + lockDuration;

        _safeMint(to, tokenId);
        _setTokenURI(tokenId, uri);
    }

    function _update(address to, uint256 tokenId, address auth)
        internal
        override
        returns (address)
    {
        address from = _ownerOf(tokenId);

        // Allow minting and burning, but block transfers while locked
        if (from != address(0) && to != address(0)) {
            require(block.timestamp >= unlockTime[tokenId], "Token is still locked");
        }

        return super._update(to, tokenId, auth);
    }

    function getUnlockTime(uint256 tokenId) external view returns (uint256) {
        return unlockTime[tokenId];
    }

    function isUnlocked(uint256 tokenId) external view returns (bool) {
        return block.timestamp >= unlockTime[tokenId];
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/ReentrancyGuard.sol";

contract RaffleNFT is ERC721URIStorage, Ownable, ReentrancyGuard {
    uint256 public nextTokenId;
    uint256 public maxSupply = 100;
    uint256 public ticketPrice = 0.01 ether;

    address[] public participants;
    bool public raffleOpen = true;
    bool public winnerSelected;
    address public winner;

    constructor() ERC721("Raffle NFT", "RAFFLE") Ownable(msg.sender) {}

    function buyTicket() external payable nonReentrant {
        require(raffleOpen, "Raffle closed");
        require(msg.value >= ticketPrice, "Insufficient payment");
        require(nextTokenId < maxSupply, "All tickets sold");

        uint256 tokenId = nextTokenId++;
        participants.push(msg.sender);

        _safeMint(msg.sender, tokenId);
        _setTokenURI(tokenId, "");
    }

    function selectWinner() external onlyOwner {
        require(!winnerSelected, "Winner already selected");
        require(participants.length > 0, "No participants");

        uint256 randomIndex = uint256(
            keccak256(abi.encodePacked(block.timestamp, block.prevrandao, participants.length))
        ) % participants.length;

        winner = participants[randomIndex];
        winnerSelected = true;
        raffleOpen = false;
    }

    function claimPrize() external nonReentrant {
        require(winnerSelected, "Winner not selected yet");
        require(msg.sender == winner, "Not the winner");

        uint256 prize = address(this).balance;
        payable(winner).transfer(prize);
    }

    function getParticipantsCount() external view returns (uint256) {
        return participants.length;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/ReentrancyGuard.sol";

contract EnglishAuctionNFT is ERC721URIStorage, Ownable, ReentrancyGuard {
    uint256 public nextTokenId;
    uint256 public maxSupply = 50;

    struct Auction {
        address highestBidder;
        uint256 highestBid;
        uint256 endTime;
        bool settled;
        bool active;
    }

    mapping(uint256 => Auction) public auctions;
    mapping(uint256 => mapping(address => uint256)) public bids;

    constructor() ERC721("English Auction NFT", "AUCTION") Ownable(msg.sender) {}

    function mintAndStartAuction(string memory uri, uint256 duration) external onlyOwner {
        require(nextTokenId < maxSupply, "Max supply reached");

        uint256 tokenId = nextTokenId++;
        _safeMint(address(this), tokenId); // Contract holds the NFT during auction
        _setTokenURI(tokenId, uri);

        auctions[tokenId] = Auction({
            highestBidder: address(0),
            highestBid: 0,
            endTime: block.timestamp + duration,
            settled: false,
            active: true
        });
    }

    function bid(uint256 tokenId) external payable nonReentrant {
        Auction storage auction = auctions[tokenId];
        require(auction.active, "Auction not active");
        require(block.timestamp < auction.endTime, "Auction ended");
        require(msg.value > auction.highestBid, "Bid too low");

        // Refund previous highest bidder
        if (auction.highestBidder != address(0)) {
            payable(auction.highestBidder).transfer(auction.highestBid);
        }

        auction.highestBidder = msg.sender;
        auction.highestBid = msg.value;
        bids[tokenId][msg.sender] = msg.value;
    }

    function settleAuction(uint256 tokenId) external nonReentrant {
        Auction storage auction = auctions[tokenId];
        require(auction.active, "Auction not active");
        require(block.timestamp >= auction.endTime, "Auction not ended");
        require(!auction.settled, "Already settled");

        auction.settled = true;
        auction.active = false;

        if (auction.highestBidder != address(0)) {
            _transfer(address(this), auction.highestBidder, tokenId);
        }
    }

    function withdraw() external onlyOwner {
        payable(owner()).transfer(address(this).balance);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MessagingNFT is ERC721URIStorage, Ownable {
    uint256 public nextTokenId;
    uint256 public maxSupply = 300;
    uint256 public mintPrice = 0.015 ether;

    struct Message {
        address sender;
        string content;
        uint256 timestamp;
    }

    mapping(uint256 => Message[]) public tokenMessages;

    event NewMessage(uint256 indexed tokenId, address indexed sender, string content);

    constructor() ERC721("Messaging NFT", "MSGNFT") Ownable(msg.sender) {}

    function mint(string memory uri) external payable {
        require(nextTokenId < maxSupply, "Max supply reached");
        require(msg.value >= mintPrice, "Insufficient payment");

        uint256 tokenId = nextTokenId++;
        _safeMint(msg.sender, tokenId);
        _setTokenURI(tokenId, uri);
    }

    function sendMessage(uint256 tokenId, string memory content) external {
        require(_ownerOf(tokenId) != address(0), "Token does not exist");
        require(bytes(content).length > 0, "Empty message");
        require(bytes(content).length <= 280, "Message too long");

        tokenMessages[tokenId].push(Message(msg.sender, content, block.timestamp));
        emit NewMessage(tokenId, msg.sender, content);
    }

    function getMessagesCount(uint256 tokenId) external view returns (uint256) {
        return tokenMessages[tokenId].length;
    }

    function getMessage(uint256 tokenId, uint256 index) external view returns (
        address sender,
        string memory content,
        uint256 timestamp
    ) {
        Message memory msgData = tokenMessages[tokenId][index];
        return (msgData.sender, msgData.content, msgData.timestamp);
    }

    function withdraw() external onlyOwner {
        payable(owner()).transfer(address(this).balance);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract GuildMembership is ERC721, Ownable {
    uint256 private _nextTokenId;

    constructor(address initialOwner) 
        ERC721("Guild Membership", "GMEMBER") 
        Ownable(initialOwner) 
    {}

    function mint(address to) external onlyOwner {
        uint256 tokenId = _nextTokenId++;
        _safeMint(to, tokenId);
    }

    function burn(uint256 tokenId) external {
        require(ownerOf(tokenId) == msg.sender || msg.sender == owner(), "Not authorized");
        _burn(tokenId);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

contract GuildVesting {
    using SafeERC20 for IERC20;

    IERC20 public immutable token;
    address public immutable beneficiary;
    uint256 public immutable start;
    uint256 public immutable duration;
    uint256 public immutable totalAmount;
    uint256 public released;

    event TokensReleased(uint256 amount);

    constructor(
        address _token,
        address _beneficiary,
        uint256 _start,
        uint256 _duration,
        uint256 _totalAmount
    ) {
        token = IERC20(_token);
        beneficiary = _beneficiary;
        start = _start;
        duration = _duration;
        totalAmount = _totalAmount;
    }

    function releasable() public view returns (uint256) {
        if (block.timestamp < start) return 0;
        if (block.timestamp >= start + duration) {
            return totalAmount - released;
        }
        uint256 vested = (totalAmount * (block.timestamp - start)) / duration;
        return vested - released;
    }

    function release() external {
        uint256 amount = releasable();
        require(amount > 0, "Nothing to release");
        released += amount;
        token.safeTransfer(beneficiary, amount);
        emit TokensReleased(amount);
    }
}
