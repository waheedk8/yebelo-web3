// SPDX-License-Identifier: MIT
pragma solidity >=0.6.4;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";


contract Yebelo is ERC20, Ownable {
    /* Tried for an additional feature to restrict deposits capacity is full
    // uint[] Slab=[100,200,300,400,500];                  // Slab along with index

    // uint totalCapRemaining=Slab[4]+Slab[3]+Slab[2]+Slab[1]+Slab[0];                                 // Addition feature to check remaining capacity 
    // uint currentSlabcap=Slab[Slab.length-1];                            // if slab array is empty no more insertion is allowed
    */
    uint public deposited=0;                    //Keep track of capacity
    //uint totalDeposited;  // For addition feature 
    uint lastTransfer;

    address payable own; // Name conflicts with Ownable contract's owner 


    struct Stack{
        bool hasDeposited;
        uint depositedAmount;
        uint slab;

    }
    mapping(address=>Stack)public slabDetails; // Mapping an address to a struct containing necessary details

    constructor() ERC20("Yebelo", "YBL") {
        mint(address(this),5000); // Minting 5000 tokens for contract address 
        mint(msg.sender,3000);  // Minting 3000 tokens for contract caller so the address could transfer tokens
        own=payable(msg.sender);

    }

    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }

    /**
     * Query for slab of a given address
     */
    function returnSlab(address _checker)public view returns (uint){
        require(slabDetails[_checker].hasDeposited,"Deposit tokens before checking for slab."); // cannot check for slab of an address if the address has not deposited yet
        return slabDetails[_checker].slab;
    }

    /**
     * Core function to transfer tokens to contract address
     */

    function transferToContract(address _to,uint _amount)public payable {
        
        transfer(_to, _amount);
        slabDetails[msg.sender].hasDeposited=true;
        slabDetails[msg.sender].depositedAmount=slabDetails[msg.sender].depositedAmount+_amount;
        deposited+=_amount;
    }
    /**
     * Query for token balanceOf contract's address
     */
    function getBalanceOfToken(address _address) public view returns (uint) {
        
        return ERC20(_address).balanceOf(address(this));
    }

    function assignSlab()public  {
        uint amountDeposited=slabDetails[msg.sender].depositedAmount;
        if (amountDeposited<=100 ){
            slabDetails[msg.sender].slab=4;
        }
        else if (amountDeposited > 100 && amountDeposited <= 200){
            slabDetails[msg.sender].slab=3;
        }
        else if (amountDeposited > 200 && amountDeposited <= 300){
            slabDetails[msg.sender].slab=2;
        }
        else if (amountDeposited > 300 && amountDeposited <= 400){
            slabDetails[msg.sender].slab=1;
        }
        else if (amountDeposited > 400 && amountDeposited <= 500){
            slabDetails[msg.sender].slab=0;
        }

                    //Invalid value


    }
}
