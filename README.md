# Lottery-Contract
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Lottery {
    address[] public players;
    address public winner;
    uint256 public ticketPrice = 0.01 ether;

    function enter() public payable {
        require(msg.value == ticketPrice, "Incorrect ticket price");
        players.push(msg.sender);
    }

    function pickWinner() public {
        require(players.length > 0, "No players");
        uint256 index = uint256(keccak256(abi.encodePacked(block.timestamp, block.prevrandao))) % players.length;
        winner = players[index];
        payable(winner).transfer(address(this).balance);
    }

    function getPlayers() public view returns (address[] memory) {
        return players;
    }
}
