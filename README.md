# Simple Contract

The simple contract is designed to manage funds by allowing the owner to deposit and withdraw Ether. The contract also provides a way to view its balance.

## Description

The simple contract is a basic implementation of a fund management system. The contract ensures that only the owner (the deployer of the contract) can deposit and withdraw funds, while the balance can be viewed by anyone. This contract is built using Solidity and is intended to be deployed on the Ethereum blockchain. The primary functions include depositing funds, withdrawing funds, and checking the contract balance.

## Getting Started

### Installing

* Ensure you have Node.js and npm installed. Then, install Hardhat and other dependencies.

### Executing progra

1.Open In Remix

2.Compile

3.Deploy

## Code
```
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;
//import "hardhat/console.sol";

contract Simple_contract {
    address payable public owner;
    uint256 public balance;

    event Deposit(uint256 amount);
    event Withdraw(uint256 amount);

    constructor(uint initBalance) payable {
        owner = payable(msg.sender);
        balance = initBalance;
    }

    function getBalance() public view returns(uint256){
        return balance;
    }

    function deposit(uint256 _amount) public payable {
        uint _previousBalance = balance;

        // make sure this is the owner
        require(msg.sender == owner, "You are not the owner of this account");

        // perform transaction
        balance += _amount;

        // assert transaction completed successfully
        assert(balance == _previousBalance + _amount);

        // emit the event
        emit Deposit(_amount);
    }

    // custom error
    error InsufficientBalance(uint256 balance, uint256 withdrawAmount);

    function withdraw(uint256 _withdrawAmount) public {
        require(msg.sender == owner, "You are not the owner of this account");
        uint _previousBalance = balance;
        if (balance < _withdrawAmount) {
            revert InsufficientBalance({
                balance: balance,
                withdrawAmount: _withdrawAmount
            });
        }
         
         // transfer the amount to the owner
        (bool sent, ) = owner.call{value: _withdrawAmount}("");
        require(sent, "Failed to send Ether");

        // withdraw the given amount
        balance -= _withdrawAmount;

        // assert the balance is correct
        assert(balance == (_previousBalance - _withdrawAmount));

        // emit the event
        emit Withdraw(_withdrawAmount);
    }
}
```

## Help

For common issues:

* Ensure your Node.js and npm versions are up to date.

## Authors

VINAY

vinayseh244@gmail.com


## License

This project is licensed under the [Simple Contract] License - see the LICENSE.md file for details
