# Summary
CupStakes is a decentralized application (DApp) built on the Algorand blockchain. It offers users the opportunity to participate in a lottery-style game by purchasing tickets using Algo coins. The game randomly selects a winning NFT ID, and the user who holds that NFT can either burn it or collect it as a prize. 

The project ensures transparency and fairness through the utilization of smart contracts and random number generation mechanisms. Users place trust in CupStakes' partners who possess 5/8 multisig access to the admin role. CupStakes aims to provide an engaging and secure gaming experience for participants in the Algorand ecosystem.

  | Project Name | CupStakes |
  |---|---|
  | Language | Python |
  | Platform | Algorand |
  | Codebase | https://github.com/CupStakes/cupstakes-smart-contracts |
  | Commit | 2155155985eb525f111458e0c237b0aaed642c39 |
## Audit Scope

  | ID | File | SHA256 |
  |---|---|---|
  | DS | draw/sc.py | 7821e7b51dd0cbf6d57fb416d7b5e74c08a7b6da0c69ad4437de8958116cfc81 |
  | CS | storage/sc.py | 0aa70668b760deeca8ae0418a40bb0a07306fc1e5e13e467ecdbbb3dff2c45e9 |
  | DA | draw/assets.py | 1214396d08f15e94f591bd356344afdb845ab9bbf627414a08d11d636b0cf367 |
# Audit Overview

  | Total issues | 1 |
  |---|---|
  | Critical | 0 |
  | High | 0 |
  | Medium | 0 |
  | Low | 0 |
  | Informational | 1 |
# General security assumptions
In conducting our security audit of the CupStakes Protocol, we have made a number of important security assumptions. These assumptions are fundamental to our analysis and should be considered when evaluating the overall security and potential vulnerabilities of the system:  

- **Security of the Underlying Blockchain**: Our security analysis is predicated on the assumption that the underlying blockchain (for example, Algorand) is secure and functions as intended. Potential vulnerabilities or flaws in the blockchain protocol are outside the scope of this audit.

- **Trusted Environment**: We assume that users of the smart contract will interact with it in a secure and trusted environment. This encompasses the use of secure, up-to-date software and hardware, which is free from malware or any potential interference from malicious third parties.

- **Private Key Security**: Users of the smart contract are assumed to safeguard their private keys effectively. This involves not sharing private keys, securely storing key backups, and using hardware wallets or other secure means for key management.  

These security assumptions form the basis of our audit. Deviations from these assumptions could potentially lead to risks or vulnerabilities not covered in this analysis. Therefore, the effective management of these factors is critical to ensure the ongoing security of the CupStakes Protocol.
# Actors in the system
We've identified the following actors in the CupStakes ecosystem: 
- **user** - anyone interacting with a protocol in a permissionless manner
- **admin** - can delete application, withdraw all funds from contract, get free nfts,  update state variables, change and close out all held NFTs to creator address
- **super admin** - can update application code and has the highest level of authority

# Thread model for each actor
### User
The user interacts with the smart contract and participates in the CupStakes application on the Algorand blockchain. The user's primary role is to purchase tickets and potentially win or collect NFTs based on the outcome of the draw. From a security standpoint, there are no major identified security threats directly impacting the user. However, it is important for the user to place trust in CupStakes' partners who possess 5/8 multisig access to the admin role. These partners have a certain level of control and authority over the smart contract, which may introduce security concerns if they misuse their privileges. It is essential for the user to have confidence in the integrity and trustworthiness of these partners to ensure fair operation of the application.
### Admin
The admin role in the smart contract on Algorand does not have any detected vulnerabilities or explicit threats. However, there is a potential risk associated with the misconfiguration of the system during the update of state variables using the 'update_state_int' method. One possible scenario is when the admin provides incorrect or invalid values during the update process, which could lead to misconfigurations within the system. These misconfigurations might result in unintended consequences, system instability, or potential vulnerabilities within the smart contract. Therefore, it is essential for the protocol's owner, who holds the admin role, to exercise extreme caution when updating the configuration.
### Super admin
The super admin role in the smart contract on the Algorand blockchain has the highest level of authority and is protected using a 2/2 multisig mechanism. Only the super admin has the opportunity to update the application, which helps ensure the integrity and security of the system. However, there is one identified vulnerability that can exist in this scenario: 
* Incorrect Application Updating: Despite the protection provided by the 2/2 multisig mechanism, there is still a potential risk of incorrect application updating by the super admin. This vulnerability can arise if the super admin mistakenly or maliciously performs an incorrect update to the application. Such incorrect updates can introduce bugs, vulnerabilities, or unintended consequences that compromise the functionality, security, or stability of the smart contract.

# Findings
  We have not identified any critical vulnerabilities during our review. All findings below are informational.  
* It is recommended to utilize the Beaker framework instead of plain PyTeal when developing smart contracts, as per the guidelines.

# Disclaimer
The smart contracts given for audit have been analyzed by the best industry practices at the date of thisreport, with cybersecurity vulnerabilities and issues in smart contract source code, the details of which aredisclosed in this report (Source Code); the Source Code compilation, deployment, and functionality(performing the intended functions). 

The report contains no statements or warranties on the identification of all vulnerabilities and security of the code. The report covers the code submitted to and reviewed, so it may not be relevant after any modifications. Do not consider this report as a final and sufficient assessment regarding the utility and safety of the code, bug-free status, or any other contract statements.

While we have done our best in conducting the analysis and producing this report, it is important to note that you should not rely on this report only — we recommend proceeding with several independent audits and a public bug bounty program to ensure the security of smart contracts. English is the original language of the report. The Consultant is not responsible for the correctness of the translated versions.

Smart contracts are deployed and executed on a blockchain platform. The platform, its programming language, and other software related to the smart contract can have vulnerabilities that can lead to hacks. Thus, Consultant cannot guarantee the explicit security of the audited smart contracts