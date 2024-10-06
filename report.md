# **Accumulated finance Audit Competition on Hats.finance** 


## Introduction to Hats.finance


Hats.finance builds autonomous security infrastructure for integration with major DeFi protocols to secure users' assets. 
It aims to be the decentralized choice for Web3 security, offering proactive security mechanisms like decentralized audit competitions and bug bounties. 
The protocol facilitates audit competitions to quickly secure smart contracts by having auditors compete, thereby reducing auditing costs and accelerating submissions. 
This aligns with their mission of fostering a robust, secure, and scalable Web3 ecosystem through decentralized security solutions​.

## About Hats Audit Competition


Hats Audit Competitions offer a unique and decentralized approach to enhancing the security of web3 projects. Leveraging the large collective expertise of hundreds of skilled auditors, these competitions foster a proactive bug hunting environment to fortify projects before their launch. Unlike traditional security assessments, Hats Audit Competitions operate on a time-based and results-driven model, ensuring that only successful auditors are rewarded for their contributions. This pay-for-results ethos not only allocates budgets more efficiently by paying exclusively for identified vulnerabilities but also retains funds if no issues are discovered. With a streamlined evaluation process, Hats prioritizes quality over quantity by rewarding the first submitter of a vulnerability, thus eliminating duplicate efforts and attracting top talent in web3 auditing. The process embodies Hats Finance's commitment to reducing fees, maintaining project control, and promoting high-quality security assessments, setting a new standard for decentralized security in the web3 space​​.

## Accumulated finance Overview

Omnichain modular liquid staking and restaking. Boosted staking yields and extra rewards for stakers.

## Competition Details


- Type: A public audit competition hosted by Accumulated finance
- Duration: 14 days
- Maximum Reward: $35,416.73
- Submissions: 72
- Total Payout: $16,401.84 distributed among 12 participants.

## Scope of Audit

# Project Overview
Accumulated Finance is an omnichain liquid staking protocol.

# Attack goals
We are launching ROSE liquid staking on Oasis Sapphire with on-chain stake delegation from Oasis Sapphire to Oasis Consensus Layer using Oasis Subcall library.

- Be able to take deposited assets (ROSE) from stROSE Minter
- Be able to receive more stROSE than expected from stROSE Minter
- Be able to receive more ROSE using built-in stROSE Minter withdrawal system, which is based on ERC-721 withdrawal requests
- Be able to disrupt the functionality of stROSE Minter that may lead to loss of user funds
- Find bugs in Oasis Subcall library and its methods, used in stROSE Minter, that may lead to loss of user funds

# Audit competition scope

```
|-- contracts
     |-- Minter.sol
     |-- minters
          |-- stROSEMinter.sol
```



# Deployments

Deployed contracts on Oasis Sapphire Testnet. You can interact with Accumulted Finance ROSE Liquid Staking with our frontend. Testing attacks on the deployed contracts is permissible before the end of the competition (if bugs are found out we'll redeploy).

## Frontend
https://testnet.accumulated.finance/stake/rosebeta

## Oasis Sapphire Testnet

- stROSEMinter: 0x3b3223ccf802e295202917330c59bd50f74d20ae
- stROSE (ERC-20): 0x9cd892c37258290bcdc59dc8e159785fb0045b18
- wstROSE (ERC-4626): 0x3be7a9e2526f6be015f3b2b13fe5fb9444ada20f

## Low severity issues


- **AccumulatedFi SafeTransferLib's Use May Cause Fund Miscalculation in Contracts**

  The `AccumulatedFi` contracts use the `SafeTransferLib` from Solmate, which does not verify if a token address has code. This oversight can lead to a false success response when transferring tokens, resulting in potential fund miscalculations. It is suggested to switch to OpenZeppelin's `safeERC20` or manually check for code existence before executing a transfer.


  **Link**: [Issue #51](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/51)


- **collectWithdrawalFees Function Emits Zero Due to TotalWithdrawalFees Reset**

  The function `collectWithdrawalFees` emits a zero value for collected fees due to the `totalWithdrawalFees` being set to zero before the event is emitted. This results in incorrect event data, making it unusable for frontends or indexing. It is recommended to store `totalWithdrawalFees` in a separate variable before emitting the event.


  **Link**: [Issue #20](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/20)


- **Add Minimum Deposit Check to Prevent Spam in Minter Contract**

  The `deposit()` function in `stROSEMinter.sol` lacks a minimum deposit amount check, allowing potential spammers to repeatedly deposit very low amounts. This could lead to excessive spamming, potentially affecting legitimate user deposits. While the withdrawal function includes a minimum amount check, a similar validation should be implemented for deposits to mitigate this risk.


  **Link**: [Issue #10](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/10)


- **Missing Minimum Delegation Amount Validation for ROSE Tokens in Contract**

  The `delegate()` function in `stROSEMinter.sol` allows any amount of ROSE tokens to be delegated, violating OASIS's minimum requirement of 100 ROSE tokens. This could cause delegations to fail and may allow malicious users to obstruct others by continuously sending small delegations. It is recommended to implement a validation check to ensure the minimum delegation amount is met.


  **Link**: [Issue #4](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/4)


- **Issue with Solmate's SafeTransferLib: Missing Code Existence Check for Tokens**

  The AccumulatedFi contracts use the `SafeTransferLib` library, which does not verify if a token address has associated contract code. This oversight could lead to miscalculations and potential fund losses, as unsuccessful transfers may appear successful. It's recommended to use OpenZeppelin's safeERC20 library or check code existence to ensure accuracy.


  **Link**: [Issue #51](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/51)


- **Bug in collectWithdrawalFees function results in incorrect event emission**

  The `collectWithdrawalFees` function always emits an event with zero fees because `totalWithdrawalFees` is reset to 0 before emitting. This makes the event unusable for frontend or indexing purposes. The suggested fix is to store `totalWithdrawalFees` in a local variable before setting it to zero, and then use this variable in the emit statement.


  **Link**: [Issue #20](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/20)


- **Minter.sol lacks minimum deposit check, risking spam and user issues**

  The `deposit()` function in `stROSEMinter.sol` lacks a minimum deposit check, allowing users to spam the contract with deposits as low as 0.1 tokens. This could disrupt legitimate transactions. While withdrawals have a minimum threshold to prevent such spam, implementing a similar check for deposits is recommended to enhance security and usability.


  **Link**: [Issue #10](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/10)


- **Delegate function in `stROSEMinter.sol` lacks minimum delegation amount check**

  The `delegate()` function in the `stROSEMinter.sol` contract fails to enforce a minimum delegation amount of 100 ROSE tokens, as required by OASIS documentation. This oversight allows users to attempt delegations of any amount, potentially causing failed transactions. It is recommended to implement a validation check for a minimum of 100 ROSE tokens to prevent misuse.


  **Link**: [Issue #4](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/4)



## Conclusion

The audit report for Accumulated Finance's audit competition on Hats.finance highlights several aspects of the innovative auditing approach. Hats.finance leverages decentralized audit competitions to optimize security in web3 projects, providing a cost-effective, results-oriented model that rewards auditors for identifying vulnerabilities. This approach reduces duplication of efforts, attracts top audit talent, and ensures high-quality security assessments. For Accumulated Finance, the audit competition focused on their omnichain liquid staking protocol, with a particular emphasis on identifying potential vulnerabilities in the ROSE liquid staking on Oasis Sapphire. The competition, spanning 14 days, saw the participation of numerous auditors, leading to the identification of several low-severity issues. Recommendations were made to enhance the security and robustness of the protocol by addressing bugs in contract methods, ensuring minimum checks for deposits and delegations, and improving event emission accuracy. Overall, this decentralized auditing process emphasizes Hats.finance's commitment to fostering a secure and scalable web3 ecosystem.

## Disclaimer


This report does not assert that the audited contracts are completely secure. Continuous review and comprehensive testing are advised before deploying critical smart contracts.


The Accumulated finance audit competition illustrates the collaborative effort in identifying and rectifying potential vulnerabilities, enhancing the overall security and functionality of the platform.


Hats.finance does not provide any guarantee or warranty regarding the security of this project. Smart contract software should be used at the sole risk and responsibility of users.

