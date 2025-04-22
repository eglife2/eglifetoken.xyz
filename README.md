// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Capped.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/security/Pausable.sol";

contract EGLIFE is ERC20Capped, ERC20Burnable, Ownable, Pausable {
    uint256 private constant MAX_SUPPLY = 1_000_000_000 * 10 ** 18; // 1 Billion EGL

    constructor() ERC20("EGLIFE Token", "EGL") ERC20Capped(MAX_SUPPLY) {
        _mint(msg.sender, 100_000_000 * 10 ** 18); // 100 million EGL to owner initially
    }

    function pause() public onlyOwner {
        _pause();
    }

    function unpause() public onlyOwner {
        _unpause();
    }

    function _beforeTokenTransfer(address from, address to, uint256 amount)
        internal override(ERC20) {
        require(!paused(), "Token transfers are paused");
        super._beforeTokenTransfer(from, to, amount);
    }

    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount); // only allowed within MAX_SUPPLY cap
    }
}
