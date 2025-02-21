// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

contract Gw9Salary {
    address payable public agwOffice;
    
    event Withdraw(address indexed sender, uint amount);
    event Transfer(address indexed receiver, uint amount);

    modifier onlyOwner() {
        require(msg.sender == agwOffice, "Hanya owner yang bisa");
        _;
    }

    constructor() payable {
        agwOffice = payable(msg.sender);
    }

    function deposit() public payable {}

    function getAmount() public view returns (uint) {
        return address(this).balance;
    }

    function withdraw() public onlyOwner {
        uint amount = address(this).balance;
        (bool success,) = agwOffice.call{value: amount}("");
        require(success, "Gagal kirim ETH ke agwOffice");
        emit Withdraw(msg.sender, amount);
    }

    function transfer(address payable _to, uint _amount) public onlyOwner {
        require(_to != address(0), "Alamat tidak valid");
        require(_amount <= address(this).balance, "Saldo kontrak tidak cukup");
        
        (bool success,) = _to.call{value: _amount}("");
        require(success, "Gagal kirim ETH");
        emit Transfer(_to, _amount);
    }

    function getGw9data() external pure returns (string[5] memory) {
    return [
        "Capt = 2100", 
        "C/O = 1300", 
        "AB1 = 800", 
        "AB2 = 750", 
        "COOK = 750"
    ];
}
}
