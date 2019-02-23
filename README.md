# 0xdeface.me Whitepaper - A standard to settle Ethereum smart contracts gracefully in case of vulnerabilities.

## Abstract

There's an estimated 3 million ETH (350 million USD) locked in vulnerable smart
contracts today. While, curiously, only 0.30% or 9,094 ETH (1 million USD) has
been claimed by attackers, smart contracts are continued to be attacked on a
daily basis [1]. In this publication, we present a novel scheme to prevent
contracts getting hacked by incentivizing attackers to confidentially submit
vulnerabilities directly to contract owners through 0xdeface's Negotiator
contract. 0xdeface's goal is to make contract hacking a worthwhile occupation
for attackers while providing a secure protocol for contract owners to settle
their contracts, returning users funds without losing their credibility.


## Motivation

Smart contracts are a novel paradigm in programming. With their dawn, it's
suddenly possible that machines can hold and manage monetary value. In Ethereum
they're easily written in Solidity or Vyper, both languages coming from the
traditional software world - adjusted for programming money. Solidity's
striking similarity to JavaScript, has often provoked criticism accusing it of
having to relaxed design decisions when it comes to handle money. To name a
few, Solidity doesn't have under- and overflow protection, interfaces are not
enforced and similar modifiers are frequently confused by its developers.  The
fact of the matter is, writing secure smart contracts is hard and even the best
teams in the world sometimes make mistakes.

As most smart contracts essentially expose an API to interact with them, it's
is often a technical necessity for developers to open source a smart contract's
code. While in theory, this might lead to communities forming and improving the
contracts, once deployed the contract cannot be changed. It's immutably stored
on Ethereum. This in turn causes several problems. 

Firstly, an attacker can trivially get access to a contract's source code by
searching the web. Compared to intruding traditional closed source systems,
where leaking source code is often a desired target for an attack, finding a
contract's source code makes it easy for an attacker to get an overview of the
system and its potential flaws.  Furthermore, as smart contracts are not only
limited in their size, they're also often conventionally limited in their
complexity. A positive for the attacker as they can trivially setup a local
system to tentatively attack the system without running the risk of getting
caught attacking a live system.  Finally, has an attacker found a vulnerability
on their local system, they can confidentially execute the attack as the
deterministic nature of Ethereum allows them to download all past changes and
apply them to their local system.

Secondly, the contract developer is not able to mitigate any vulnerabilities by
upgrading the contract in place. Once uploaded to the Ethereum main net, smart
contracts get assigned a hash built from the deployer's address and nounce
[find source]. This unique identifier is then used by clients to interact with
the contract. Today, Ethereum smart contracts cannot be changed in place,
meaning the source code powering them cannot be replaced without not also
changing the unique identifier used to interact with the contract (Note that
this will change with Ethereum's Constantinople update and the introduction of
CREATE2). A developers options are hence limited to the following: (A) Opt to
build an upgradable contract using proxy-patterns or (B) deploy a revised
version of the contract and inform users about the change. Both options come
with significant negatives: (A) An upgradable proxy contract is inherently not
permissionless as it likely features a contract owner that can change the
underlying logic and cause hence significant damages to the contract's users.
(B) Informing users and updating web apps to the newly generated contract
address is cumbersome and non-scalable.

Finally, instead of fairly disclosing the vulnerability to the contract
developer, an attacker might simply be incentivized to directly attack a
contract by draining its funds. Reasons for this are plentiful. With today's
privacy-technologies like TOR it's virtually trivial to be anonymous.
Privacy-preserving cryptocurrencies like Monero and Zcash allow an attacker to
"wash" their coins before and after an attack. Bug bounties for contracts might
not exist or their amounts might simply perish in comparison to the contract's
actual main net balance.

While all of these points play in favor of the attacker, executing the actual
hack still comes with significant negatives. Attacking contracts with a large
balance often comes with increased (social) media coverage and hence with an
increased community vigilance trying to expose the hacker. In that process, an
attacker's accounts might virtually get frozen.  Usually, by exchanges and
users blacklisting the attackers' accounts. This increased scrutiny in reality then often
means that an attacker cannot spend their stolen coins, potentially extinguish
an attacker's incentive to find more vulnerabilities. 

Additionally, we'd like to comically note that attacking smart contracts of
course is also a violation of the law in many juristictions, giving the
attacker again less motivation to attack as facing charges might deter many.
Speculatively, this might be why we only see 0.30% of vulnerable ETH being
stolen today.

- missing points: auditingj
    - Auditing is good, but doesn't accidents from happen. It's not enough
    - Often, there is no way to contact the contract owner simply. There is no
    canonical way of settling smart contracts. We would like to improve this

## Sources

1. [PEREZ, Daniel; LIVSHITS, Benjamin. Smart Contract Vulnerabilities: Does
   Anyone Care?. arXiv preprint arXiv:1902.06710,
   2019.](https://arxiv.org/abs/1902.06710)

