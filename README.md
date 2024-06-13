# TVRechargeContract Smart Contract

## Overview

The `TVRechargeContract` smart contract is designed to manage watch time for a TV service. The contract allows the owner to consume and add watch time, ensuring that the total available watch time does not exceed the maximum limit specified at deployment. This contract includes basic access control to restrict these actions to the owner only.

## Features

- **Watch Time Management**: Allows the owner to consume and add watch time.
- **Events**: Emits events when watch time is consumed or added.
- **Access Control**: Only the contract owner can manage watch time.

## Prerequisites

Before deploying the `TVRechargeContract`, ensure that you have the following installed:

- Node.js
- npm (Node Package Manager)
- Truffle or Hardhat
- MetaMask (or any Ethereum wallet)

## Contract Details

### Constructor

```solidity
constructor(uint256 _maxWatchTime) {
    owner = msg.sender;
    availableWatchTime = _maxWatchTime;
}
```

- **Parameters**:
  - `_maxWatchTime`: The initial maximum watch time available.

### Modifiers

#### onlyOwner

```solidity
modifier onlyOwner() {
    require(msg.sender == owner, "Only the owner can perform this action");
    _;
}
```

- Restricts function access to the contract owner.

### Functions

#### consumeWatchTime

```solidity
function consumeWatchTime(uint256 _timeConsumed) external onlyOwner {
    require(_timeConsumed > 0, "Watch time consumed must be greater than zero");
    assert(_timeConsumed <= availableWatchTime);

    availableWatchTime -= _timeConsumed;

    emit WatchTimeConsumed(msg.sender, _timeConsumed);

    if (availableWatchTime == 0) {
        revert("Watch limit reached, please recharge");
    }
}
```

- Consumes a specified amount of watch time.
- Emits the `WatchTimeConsumed` event.
- Reverts if the available watch time is zero.

#### addWatchTime

```solidity
function addWatchTime(uint256 _timeAdded) external onlyOwner {
    assert(_timeAdded > 0 && _timeAdded < 300);
    availableWatchTime += _timeAdded;
    emit WatchTimeAdded(msg.sender, _timeAdded);
}
```

- Adds a specified amount of watch time.
- Emits the `WatchTimeAdded` event.
- Ensures that the added watch time is greater than zero and less than 300.

## Events

### WatchTimeConsumed

```solidity
event WatchTimeConsumed(address indexed user, uint256 timeConsumed);
```

- Emitted when watch time is consumed.

## Usage

1. **Consume Watch Time**: The owner can call `consumeWatchTime` to consume a specified amount of watch time.
2. **Add Watch Time**: The owner can call `addWatchTime` to add a specified amount of watch time, ensuring it is less than 300.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request on GitHub.
