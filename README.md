// SPDX-License-Identifier: MIT
pragma solidity 0.8.20;
contract Lottery{
    address public manager;
    address payable[] public participant;

    constructor(){
        manager = msg.sender;
    }
    receive() external payable {
        require(msg.value==1 ether);
        participant.push(payable(msg.sender));
    }

    function getBalance() public view returns(uint){
        require(msg.sender == manager);
        return address(this).balance;
    }
    function random() public view returns (uint) {
   return uint(keccak256(abi.encodePacked(blockhash(block.number - 1), block.timestamp, participant.length)));
}

function selectWinner() public
{
    require(msg.sender==manager);
    require(participant.length>=3);
    uint r = random();
    address payable winner;
    uint index = r%participant.length;
    winner=participant[index];
    winner.transfer(getBalance());
    participant = new address payable[](0);
}


}
