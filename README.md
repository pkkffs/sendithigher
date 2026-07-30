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
