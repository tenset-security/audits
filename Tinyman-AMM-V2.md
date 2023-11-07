# Summary
Tinyman AMM V2 is a Constant Product Market Maker, mathematically similar to Tinyman V1, which was modeled after Uniswap V2. V2's contract and protocol design significantly differ from V1, utilizing more recent Algorand Virtual Machine features.

In an AMM-based decentralized exchange, pools represent asset pairs, and users can swap assets by sending one and receiving the equivalent value of the other. Liquidity Providers own and contribute to these pools, receiving Pool Tokens in return. These tokens represent their share of the pool, and they can reclaim assets at any time.

Each pool functions as an Algorand Account, managing assets and pool-related state. A single stateful Algorand Application governs asset transfers and state management for all pools.

Pools have local state variables updated with every operation. Asset reserves are tracked locally and aren't dynamically determined from account balances. This means that "extra" assets or Algo donations don't affect pool reserves but can only be transferred to the fee collector if not part of a documented group transaction, ensuring they won't impact pool liquidity or liquidity provider shares.

| Project Name | Tinyman AMM V2 |
|---|---|
| Language | Tealish |
| Platform | Algorand |
| Codebase | https://github.com/tinymanorg/tinyman-amm-contracts-v2 |
| Commit | 3de3a35e1ce0ca96576d18c2728f6042d1a07f72 |
| Mainnet Validator App ID | [1002541853](https://algoexplorer.io/application/1002541853) |
| Testnet Validator App ID | [148607000](https://testnet.algoexplorer.io/application/148607000) |

## Audit Scope

```
contracts
├── amm_approval.tl
├── amm_clear_state.tl
└── pool_template.tl
```

| ID | File | SHA256 |
|---|---|---|
| AMA | amm_approval.tl | e0b6b7fae74977f98ca204d874dd8f0ba9eb782f9302326c20d6e76f9a66f67c |
| ACS | amm_clear_state.tl | f94dde358995af5e48bfb817c942b7d8167cad33a42fc6d94bdab74d5d1ab865 |
| PT | pool_template.tl | 065e1743607627230ecf97e7628b0e55c5749743be0baf2d44d39408a4fbfc62 |

# General security assumptions
In conducting our security audit of the Tinyman AMM V2, we have made a number of important security assumptions. These assumptions are fundamental to our analysis and should be considered when evaluating the overall security and potential vulnerabilities of the system:

- **Security of the Underlying Blockchain**: Our security analysis is predicated on the assumption that the underlying blockchain (for example, Algorand) is secure and functions as intended. Potential vulnerabilities or flaws in the blockchain protocol are outside the scope of this audit.

- **Trusted Environment**: We assume that users of the smart contract will interact with it in a secure and trusted environment. This encompasses the use of secure, up-to-date software and hardware, which is free from malware or any potential interference from malicious third parties.

- **Private Key Security**: Users of the smart contract are assumed to safeguard their private keys effectively. This involves not sharing private keys, securely storing key backups, and using hardware wallets or other secure means for key management.

These security assumptions form the basis of our audit. Deviations from these assumptions could potentially lead to risks or vulnerabilities not covered in this analysis. Therefore, the effective management of these factors is critical to ensure the ongoing security of the Tinyman AMM V2.

# Actors in the system

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/tenset-security/audits/assets/65830545/963225d8-0103-4cfa-afaa-aa3dc81debae">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/tenset-security/audits/assets/65830545/7f0eef8c-137a-4586-b1f0-07f2ed762f0d">
  <img alt="Tinyman AMM V2 system diagram" src="https://user-images.githubusercontent.com/71297412/178180441-59f1644e-2ab6-4bf0-866f-2c77b2a63433.png">
</picture>

In our examination of the Tinyman AMM V2, we've identified several key actors, each with their distinct roles, responsibilities, and potential influence within the protocol. These actors are as follows:
### User
User in the Tinyman Automated Market Maker Version 2 (AMM V2) system are key participants who fulfill essential roles in this decentralized exchange (DEX) and automated market maker protocol. They encompass liquidity providers and asset swappers, engaging with the system to provide liquidity, swap assets, and actively participate in trading activities.

### Fee Setter
Fee Setter in the  system holds a privileged role in determining the fee structure for asset swaps. This role requires careful consideration of the economic model, competitive factors, and the interests of various stakeholders to ensure the sustainability and fairness of the protocol. Effective communication and transparency in fee adjustments are essential to maintain the trust and support of users and liquidity providers.

### Fee Collector
Fee Collector in the system is responsible for receiving protocol fees and, potentially, additional assets in the form of donations. This role encompasses asset management, transparency, responsible utilization of funds, and accountability to the platform's community. Effective financial stewardship is crucial to ensure the sustainability and growth of the ecosystem.
 
### Fee Manager
Fee Manager in the system holds the highest authority, with the power to set and update the roles of the Fee Setter and Fee Collector.

# Thread model for each actor

### User
**Correct usage:** Users should be aware of the potential risks associated with swapping. If funds are transferred to the protocol without any transaction arguments, they will be marked as 'donations' and will be irreversibly lost. Users must exercise caution and avoid such unintentional transfers.  
**Liquidity risk Assessment:** Users who provide liquidity to the pools should assess the risks involved, including impermanent loss and exposure to changes in asset prices. They should be prepared for fluctuations in pool balances and potential losses.

### Fee Setter
**Loss of the private key**: The primary concern for `Fee Setter` is the security of their private key to prevent operational disruptions.

### Fee Collector
**Loss of the private key**: The primary concern for `Fee Collector` is the security of their private key to prevent operational disruptions.

### Fee Manager
**Loss of the private key**: The primary concern for `Fee Manager` is the security of their private key to prevent operational disruptions. Key focus areas for `Fee Manager` are the protection and secure storage of their private key.  
**Wrong Configuration:** A potential threat includes the wrong configuration of actors. Fee Managers must ensure that the roles of Fee_Setter and Fee_Collector are correctly configured to prevent unauthorized changes or misconfigurations.
# Audit Overview

| Total issues | 2 |
|---|---|
| Critical | 0 |
| High | 0 |
| Medium | 0 |
| Low | 0 |
| Centralization Risk | 1 |
| Informational | 1 |
# Findings

| ID | Title | Severity |
|---|---|---|
| AMA-01 | Centralization Issue in the Roles of `Fee_Setter`, `Fee_Collector`, and `Fee_Manager` | Centralization Risk |


## AMA-01 | Centralization Issue in the Roles of `Fee_Setter`, `Fee_Collector`, and `Fee_Manager`
### Description
The Tinyman AMM V2 protocol currently assigns the roles of `Fee_Setter`, `Fee_Collector`, and `Fee_Manager` to the same account. This centralization of authority presents significant concerns regarding governance, transparency, and the potential for misuse. In this configuration, a single entity can set max fees and easily withdraw them to their own wallet, which could be contrary to the best interests of the broader ecosystem. This centralization issue poses a clear risk of fee manipulation and conflicts of interest, which can undermine the integrity of the protocol.

### Recommendation
We recommend using distinct accounts for the roles of `Fee_Setter`, `Fee_Collector`, and `Fee_Manager`. Additionally, considerimplementing a timelock for `Fee_Collector` and `Fee_Setter` to restrict permissions for a specified time after role updates. This approach can significantly reduce the risk of centralization.

# QA
| ID | Title | Severity |
|---|---|---|
| AMA-02 | Inconsistent Usage of `pool_address` Variable | Informational |

## AMA-02 | Inconsistent Usage of `pool_address` Variable
### Description
In the Tinyman AMM V2 protocol, on line `L273`, the variable `pool_address` is declared and initialized as `Txn.Accounts[1]`. However, on line `L290`, the direct use of `Txn.Accounts[1]` is preferred over the `pool_address` variable. This inconsistency in variable usage can lead to confusion and reduced code readability.

### Recommendation
It is recommended to enhance code clarity and maintain consistent variable usage. Consider moving the declaration of `pool_address` to line `L263` for its initialization and use `pool_address` on line `L290` instead of `Txn.Accounts[1]`. This modification will improve code readability and streamline variable usage within the protocol.

# Disclaimer
The smart contracts given for audit have been analyzed by the best industry practices at the date of thisreport, with cybersecurity vulnerabilities and issues in smart contract source code, the details of which aredisclosed in this report (Source Code); the Source Code compilation, deployment, and functionality(performing the intended functions).

The report contains no statements or warranties on the identification of all vulnerabilities and security of the code. The report covers the code submitted to and reviewed, so it may not be relevant after any modifications. Do not consider this report as a final and sufficient assessment regarding the utility and safety of the code, bug-free status, or any other contract statements.

While we have done our best in conducting the analysis and producing this report, it is important to note that you should not rely on this report only — we recommend proceeding with several independent audits and a public bug bounty program to ensure the security of smart contracts. English is the original language of the report. The Consultant is not responsible for the correctness of the translated versions.

Smart contracts are deployed and executed on a blockchain platform. The platform, its programming language, and other software related to the smart contract can have vulnerabilities that can lead to hacks. Thus, Consultant cannot guarantee the explicit security of the audited smart contracts
