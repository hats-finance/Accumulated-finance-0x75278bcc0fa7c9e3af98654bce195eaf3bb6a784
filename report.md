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


- **SafeTransferLib's Lack of Code Existence Check May Cause Fund Mismanagement**

  The AccumulatedFi contracts employ the `SafeTransferLib` from Solmate, which inadequately checks if a token address contains a contract. This flaw could lead to inaccurate fund transfers, as `safeTransfer()` and `safeTransferFrom()` may falsely return success. It is suggested to switch to OpenZeppelin's safeERC20 or implement code existence checks within the functions.


  **Link**: [Issue #51](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/51)


- **Incorrect event emission due to zeroing `totalWithdrawalFees` before event call**

  The `collectWithdrawalFees` function emits an event with a fee value that is always zero because `totalWithdrawalFees` is reset to zero before the event is emitted. This makes the event output unusable for front-end applications or indexing. The recommendation is to store the fee value in a local variable before resetting and using it in the event emission.


  **Link**: [Issue #20](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/20)


- **Deposit function lacks minimum deposit check, enabling potential spam transactions**

  The `Minter.sol` contract lacks a minimum deposit amount check, allowing users to spam the deposit functionality by depositing minimal amounts, such as 0.1 tokens. While withdrawals have a minimum limit to prevent such spam, implementing a similar check for deposits is recommended to mitigate potential spam issues and secure the process.


  **Link**: [Issue #10](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/10)


- **Add Validation for Minimum Delegation Amount in stROSEMinter.sol Delegate Function**

  The `delegate()` function in `stROSEMinter.sol` does not enforce the OASIS mandated minimum delegation amount of 100 ROSE tokens. This absent validation could allow malicious users to delegate smaller amounts, such as 1 ROSE, disrupting legitimate delegations. It is recommended to implement a validation check to ensure delegations meet the 100 ROSE minimum.


  **Link**: [Issue #4](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/4)


- **Solmate's SafeTransferLib Fails to Verify Contract Existence Causing Fund Miscalculations**

  The AccumulatedFi contracts use the `SafeTransferLib` library from Solmate, which fails to verify the existence of code at the token address, leading to potential fund miscalculations and loss. It's recommended to use OpenZeppelin's safeERC20 or implement code checks to ensure security against such issues.


  **Link**: [Issue #51](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/51)


- **collectWithdrawalFees event emits zero due to incorrect variable handling**

  The function `collectWithdrawalFees` emits an event with 0 fees collected because the `totalWithdrawalFees` is reset to 0 before the event is triggered. This issue affects the usability of the event for front-end and indexing purposes. It's recommended to save the fees in a local variable before emitting them.


  **Link**: [Issue #20](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/20)


- **Add Minimum Deposit Check to Prevent Spam in Minter.sol Deposit Function**

  The `stROSEMinter.sol` lacks a minimum deposit amount check in its `deposit()` function, potentially allowing malicious users to spam deposits with very low amounts like 0.1 tokens. This could cause issues for legitimate users. While withdrawals have a minimum check to prevent spam, it's recommended to implement a similar check for deposits.


  **Link**: [Issue #10](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/10)


- **Delegate function lacks minimum 100 ROSE token validation as required by OASIS**

  The `delegate()` function in `stROSEMinter.sol` does not enforce the OASIS minimum delegation requirement of 100 ROSE tokens. Without this constraint, users can delegate smaller amounts, which will fail, potentially disrupting the system and harming other users. It is recommended to implement a validation check to ensure delegations meet the minimum amount.


  **Link**: [Issue #4](https://github.com/hats-finance/Accumulated-finance-0x75278bcc0fa7c9e3af98654bce195eaf3bb6a784/issues/4)



## Conclusion

The Hats.finance audit competition for Accumulated Finance, an omnichain liquid staking protocol, showcases a novel, decentralized approach to improving Web3 security. By hosting a 14-day audit competition, Hats.finance leverages the collective expertise of numerous auditors to proactively identify vulnerabilities, rewarding only successful submissions, and efficiently managing costs. During this competition, 72 submissions were made with a total payout of $16,401.84 distributed among 12 participants, under a maximum reward cap of $35,416.73. Notable low-severity issues identified include inadequate checks in the SafeTransferLib library, incorrect event emissions due to variable mishandling, and the absence of minimum deposit and delegation checks, which could facilitate spam and disrupt operations. These findings underline the importance of implementing thorough validation checks and using secure libraries to mitigate such vulnerabilities. This audit process not only enhances Accumulated Finance's security posture but also sets a benchmark for decentralized security solutions in the Web3 space.

## Disclaimer


This report does not assert that the audited contracts are completely secure. Continuous review and comprehensive testing are advised before deploying critical smart contracts.


The Accumulated finance audit competition illustrates the collaborative effort in identifying and rectifying potential vulnerabilities, enhancing the overall security and functionality of the platform.


Hats.finance does not provide any guarantee or warranty regarding the security of this project. Smart contract software should be used at the sole risk and responsibility of users.

