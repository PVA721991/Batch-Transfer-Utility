# Batch-Transfer-Utility
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract BatchTransfer {
    error LengthMismatch();
    error TransferFailed();

    event BatchSent(uint256 count);

    function batchSend(address[] calldata recipients, uint256[] calldata amounts) public payable {
        if (recipients.length != amounts.length) revert LengthMismatch();

        for (uint256 i = 0; i < recipients.length; i++) {
            (bool success, ) = recipients[i].call{value: amounts[i]}("");
            if (!success) revert TransferFailed();
        }

        emit BatchSent(recipients.length);
    }
}
