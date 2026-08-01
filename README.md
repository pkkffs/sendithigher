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

Getting familiar with the basic setup.

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
