
# Aloe contest details

- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the issue page in your private contest repo (label issues as med or high)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)

# Q&A

### Q: On what chains are the smart contracts going to be deployed?
mainnet, Arbitrum, Optimism, Base
___

### Q: Which ERC20 tokens do you expect will interact with the smart contracts? 
Any standard token, including USDT
___

### Q: Which ERC721 tokens do you expect will interact with the smart contracts? 
None
___

### Q: Which ERC777 tokens do you expect will interact with the smart contracts? 
None
___

### Q: Are there any FEE-ON-TRANSFER tokens interacting with the smart contracts?

No, fee-on-transfer tokens are not supported.
___

### Q: Are there any REBASING tokens interacting with the smart contracts?

No, rebasing tokens are not supported.
___

### Q: Are the admins of the protocols your contracts integrate with (if any) TRUSTED or RESTRICTED?
Restricted. The protocol should be able to handle any changes enacted by Uniswap Governance (i.e., new fee tiers or changes to the protocol fee).
___

### Q: Is the admin/owner of the protocol/contracts TRUSTED or RESTRICTED?
Restricted. The `governor` address should not be able to steal funds or prevent users from withdrawing. It does have access to the govern methods in `Factory`, and it could trigger liquidations by increasing `nSigma`. We consider this an acceptable risk, and the governor itself will have a timelock.
___

### Q: Are there any additional protocol roles? If yes, please explain in detail:
There is a `RESERVE` address, to which a portion of all interest accrues. It should be able to take any action and behave normally.

We also have a built-in referral program. If you enroll as a "courier" and attach the courier ID to your users' deposits, you will receive a portion of their interest. Couriers can take any action in the protocol *except* claiming rewards.

Also, if a courier credits *another courier* for their deposit, 100% of the first couriers earnings will be passed through to the other one. This edge case should not break anything or impact any other users.
___

### Q: Is the code/contract expected to comply with any EIPs? Are there specific assumptions around adhering to those EIPs that Watsons should be aware of?
The `Lender` complies with ERC4626 and EIP2612. Notes on our 4626 implementation are available here: https://coda.io/d/_dJtHvRlIBOx/Standard-Compliance_su6Wx
___

### Q: Please list any known issues/acceptable risks that should not result in a valid finding.
After a Borrower has been `warn`ed, but before it's liquidated, there's an opportunity to make it healthy and avoid liquidation. If this is done via a call to `Borrower.modify`, the warning will be cleared, and further interactions are normal. However, if it's done by a 3rd-party via Lender.repay (with the borrower set as the beneficiary), the warning will not be cleared.
___

### Q: Please provide links to previous audits (if any).
https://drive.google.com/file/d/1aWEkCTTcuEnupf6nbIsqWy38igsj9-Hx/view?usp=sharing
___

### Q: Are there any off-chain mechanisms or off-chain procedures for the protocol (keeper bots, input validation expectations, etc)?
We expect interested parties to watch Borrowers and call `warn`/`liquidate` when possible. Same goes for the emergency `pause` method in Factory, and the `update` function in the VolatilityOracle.
___

### Q: In case of external protocol integrations, are the risks of external contracts pausing or executing an emergency withdrawal acceptable? If not, Watsons will submit issues related to these situations that can harm your protocol's functionality.
Uniswap cannot be paused.
___

### Q: Do you expect to use any of the following tokens with non-standard behaviour with the smart contracts?
We use solmate's SafeTransferLib, so the protocol should work with tokens that are missing return values.
___

### Q: Add links to relevant protocol resources
https://docs.aloe.capital/aloe-ii/overview
https://docs.aloe.capital/aloe-ii/auditor-quick-start
https://aloelabs.github.io/aloe-ii/index.html
___



# Audit scope