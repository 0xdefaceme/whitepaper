# 0xdeface.me Whitepaper

> A standard to settle Ethereum smart contracts fairly in case of
> vulnerabilities.

## Abstract

There's an estimated 3 million ETH (350 million USD) locked in vulnerable smart
contracts today. While, curiously, only 0.30% or 9,094 ETH (1 million USD) have
been claimed by attackers, smart contracts continue to be attacked on a daily
basis [1]. In this paper, we present a novel scheme to prevent contracts from
getting their funds drained by attackers. We introduce a game (0xdeface.me)
that incentivizes attackers to confidentially submit vulnerabilities directly
to a contract's owner, transforming contract attacking from a zero-sum game
to a positive-sum game. 0xdeface's goal is to make contract attacking
a worthwhile occupation while providing a secure protocol for owners to settle
their contracts fairly by returning users' funds.

## Introduction

Smart contracts represent a fundamental paradigm shift in programming. Ever
since their practical implementation through Ethereum, they allow for
permissionless and transparent transfer of monetary value. As their advent is
still recent, many best-practices are under active development and will take
years to stabilize.

In Ethereum, smart contracts are written in Solidity or Vyper, with both being
respectively inspired by languages from the traditional software world:
JavaScript and Python. In both Solidity and Vyper's case, the base language was
slightly adjusted to feature new primitives making transfer of monetary value
possible.  Not always with great success, however.  Especially Solidity's
striking similarity to JavaScript has provoked criticism, the accusation of
having too relaxed design principles for transferring money securely being
thrown around on the Internet frequently. And indeed, Solidity features odd
design choices. To name a few: (1) It doesn't have under- and overflow
protection, (2) interfaces are not enforced and (3) similar modifiers are
easily confused as their naming is virtually indistinguishable.

Unsurprisingly, these choices paired with a lack of best practices,
inexperienced developers and lots of money at stake caused several million ETH
to be stolen. Most notably in June 2016, when an attacker drained 3.6 million
ETH from TheDAO [2], forcing the Ethereum community to hard fork the network to
regain access to the funds. Though much improvement has taken place over the
last years with (1) the community working together towards best practices, (2)
developers becoming more experienced and (3) professional audit firms
eliminating risks pre-launch, writing secure smart contracts remains difficult.
To this day, even the most experienced teams and their world-class auditors
make mistakes [3] and will, going forward.

To further understanding of this phenomenon and to give proper context, we'd
like to highlight the incentives at play.  Specifically, we'd like to elaborate
on the motivations to attack contracts and how we think 0xdeface.me can help to
improve the situation from an ecosystem-wide zero-sum game to a positive-sum
game.

## Motivation

With the release of automated auditing suites like Consensys's Mithril [4] and
TrailOfBits's ManiCore [5], documentation of common vulnerabilities and the
growing monetary value at stake, we'll see the frequency and ingenuity of smart
contract attacks increase in the future. 

In this section, we hence discuss the motives at play for (1) why it is
seemingly rational for an attacker to target smart contracts, (2) why it is
difficult to maintain smart contracts and (3) why this leads to a zero-sum game
for all participants involved. Furthermore, we motivate why there should be an
EIP standard and marketplace for consensually exchanging vulnerabilities
between attackers and contract owners. To begin with, we elaborate reasons for
why it is seemingly rational for an attacker to target smart contracts.

Ever since the dawn of Bitcoin and Ethereum, there has been a commonly shared
conviction of the community that transparency, decentralization and
permission-less innovation is the key to success of the respective ecosystems.
For several reasons, developing a smart contract as closed source is hence
considered a faux-pas. (1) Users should be able comprehend the full
functionality of a smart contract (transparency). (2) Nobody must be able to
exercise full control over a contract (decentralization). (3) Everybody must be
be allowed, without the incurrence of a cost, to innovate on any part of the
infrastructure (permission-less). It is therefore, and for other miscellaneous
reasons, a best practice in the ecosystem to open source smart contracts. For
an smart contract attackers, however, this yields a positive side effect.
While, open sourcing smart contracts can lead to communities and ecosystems
forming and best practices to develop, it also leads to source code being
easily accessible to attackers.

0xdeface ran a several week long experiment where we, using an Ethereum full
node, audited and crawled the Ethereum blockchain and GitHub for newly uploaded
smart contracts. Through leaving their `/build` folder in their GitHub
repository, contract repositories could simply be found by their address using
the GitHub search API.  While the setup of this experiment was trivial, it
yielded many potential opportunities where contracts were vulnerable and easily
exploitable. In many cases, Mythril, the automatic auditing tool, even hinted
towards the specific function and vulnerability to exploit [9].  Even more
trivial than that is of course to occasionally scan Etherscan's "Verified
Contracts" page [5] for smart contract source code [footnote 1]. All this to
say that while open sourcing smart contracts might increase security
ecosystem-wide, it may not necessarily for the individual contract.

Once an attacker has access to a contract's source code, they can setup a local
environment using tools like Ganache, Truffle and Remix IDE. This local setup
is then used to simulate attacks against the contract. Is a serious
vulnerability found, the attacker can - given the deterministic nature of
Ethereum - confidentially assess whether it can be used to drain funds on the
main net contract. Using privacy-preserving technologies like TOR, Monero and
Zcash, this makes attacks efficient, cheap, largely anonymous and therefore
rational for an attacker.

Now that we've given reasons for why attacking smart contracts is rational,
we'd like to elaborate why it is difficult for owners to maintain their
contracts once deployed.

In essence there's a simple reason: Smart contracts are immutable. Every
contract uploaded to the Ethereum network gets assigned an address, that is
generated using the deployer's address and a nonce. This address is then used
by clients to interact with the contract. Today, Ethereum smart contracts
cannot be changed in place [footnote 2].  A contract owner's options are hence
limited to the following maintenance schemes: (1) Opt to build an upgradeable
contract using proxy-patterns or (2) deploy a revised version of the contract
and inform users about the change.

Unfortunately, both options come with significant negatives: (1) An upgradeable
proxy contract is inherently not permissionless and/or decentralized. Though
implementation-specific, it likely defines an owner that is able to change the
proxy routing such that potentially users' funds could get, in the worst case,
stolen. (2) Informing users and updating apps to the newly generated contract
address is cumbersome, trust-maximizing and non-scalable. It comes with
increased scrutiny by the community, potential loss of credibility and
significant cost.

While in the traditional software world vulnerabilities can simply be fixed in
minutes by identifying, fixing and redeploying the software with the same
state, Ethereum's immutability property makes mitigating vulnerabilities 
non-trivial. For many businesses in the ecosystem, smart contract
vulnerabilities might in fact be a worst-case scenario. We hence conclude that
maintaining deployed contracts is, and will be, difficult.

Which brings us to why attacking smart contracts is currently a zero-sum game.
Firstly, we argue why attackers may not be incentivized to attack contracts.
Secondly, we highlight the problems of contract owners suffering an attack.
Thirdly, we state the role of the user in the situation.

Attacking contracts, especially ones of widely-known projects, often comes with
several negatives for the attacker. (1) Increased (social) media coverage and
community vigilance can lead to the hacker getting exposed.  (2) Through
crowd-sourcing the attacker's addresses, the stolen founds can effectively be
frozen. This usually happens by exchanges blacklisting the attacker's accounts.
(3) Stolen funds can most likely not be exchanged into fiat, as most exchanges
require KYC for larger amounts. (4) An attacker runs the risk of being
prosecuted by the law as stealing users' funds is very likely a criminal
offense in many jurisdictions [footnote 3]. These, amongst others, may be the
reasons for why only 0.3% of vulnerable funds have been claimed by attackers
[1].

For a contract owner, the situation is equally tricky. (1) Bounty programs are
often run before a contract gets deployed. After deployment, the contract quite
literally _becomes_ the bounty [8]. (2) As seen recently, audits can only limit
decrease the risk of vulnerabilities but not totally eradicate them [footnote
4]. (3) And as stated before, successful attacks to deployed contracts are
often businesses' worst-case scenarios, going so far as to the total shutdown
of the operation (see: The DAO) [2].

In all of this, the contract users are probably off worst. They potentially use
all their funds and often have no way to participate in the governance
decisions taken by a contract owner under fire.  With regulation lagging
behind, there is also a significant lack in accountability. Who's to blame for
the loss of user funds? The attacker? The contract owner?

Hence as all participants lose when contracts are attacked, we conclude this to
be a zero-sum game. It is at this point that we introduce 0xdeface. 0xdeface or
EIP-XXXX is a standard to settle deployed smart contracts gracefully in favor
of users and developers. Auditors confidentially submit disclosures to
0xdeface. Contract owners review disclosures. Do auditor and contract owner
agree that a serious vulnerability has been found, then a contract can be
settled fairly by returning its users' funds. Auditors get rewarded with a
bounty held in escrow by 0xdeface's Negotiator. 0xdeface makes attacking
Ethereum smart contracts a positive-sum game.

## Overview

0xdeface's incentive game exists of multiple components. In this section we
define what these components are and how they form an incentive game, making
smart contract attacking a positive-sum game.

### Negotiator

The _negotiator_ is 0xdeface's central component. It is an Ethereum smart
contract consisting of functions for (1) an attacker to _commit_ a
vulnerability, (2) a contract owner to _pay_ for a vulnerability, (3) an
attacker to _reveal_ a vulnerability and (4) a contract owner to _decide_ on a
vulnerability. In essence, the negotiator allows an attacker to confidentially
transmit a vulnerability to a contract owner using public-key cryptography
(commit and reveal). In order for a contract owner to view a vulnerability,
however, they'll have to send a stake to the negotiator (pay). Has a potential
vulnerability been found, then it is in the contract owner's decision to either
(decide):

1. Shut down the contract, return its users' funds and send the stake to the
   attacker; or
1. Ignore the vulnerability, sending the stake back to the contract owner.

In the following sections, we argue for why we believe the negotiator's
scheme is in fact incentivizing fair settlements of contracts.

### Exploitable EIP standard

Another central component is 0xdeface's _exploitable_ EIP standard. It is an
optional interface for developers to implement, potentially preventing their
contracts from getting drained. Exploitable currently consists of seven
functions. These being: 

1. `exploitableVersion()`: Returning the version of the incentive game
1. `implementsExploitable()`: Returning the attacker's potential reward in Wei,
1. `pay(uint256 bountyId, string publicKey)`: For the owner to submit their
   public key along with the attacker's reward.
1. `decide(uint256 bountyId, bool decision)`: For the owner to decide on the
   seriousness of a vulnerability
1. `restore()`: Invoked when an owner decides to ignore a vulnerability; and 
1. `exit()`: Invoked when an owner decides to shut down the contract due to a
   vulnerability.

Going forward, we discuss how developers will profit from implementing the
Exploitable EIP standard.

### 0xdeface.me Website

0xdeface.me serves as a hub of exchange for all stakeholders (owners, attackers
and contract users). As already mentioned, we believe that there is a large
untapped potential in improving the security of Ethereum smart contracts by
gamifying the experience. 0xdeface.me allows contract owners to confidentially
negotiate with attackers, attackers to be rewarded fairly and users to be able
to participate in governance decisions. In the process of building this
infrastructure, it is 0xdeface's goal to nurture a community of security
professionals by providing tools for collaboration and automation.

Now that we highlighted the main components, we walk through the process of
commiting and eventually exiting a vulnerability.

## Process

This section outlines the interactive process of _commiting_, _paying_,
_revealing_ and _deciding_ on a vulnerability. As it serves the purpose to
further understanding about *how* the system works, we're going to ignore
incentives for now. To simplify, we're instead going to make a few assumptions.
(1) The exploitable contract implements the Exploitable EIP standard. (2) The
attacker is willing to participate in 0xdeface's incentive game.  To not blow
the scope of this paper, we're only going to describe the happy path here.
We'll discuss attack vectors in one of the following sections.

As figure 1 illustrates, the process starts with the attacker finding a
vulnerability in the exploitable contract.

![figure 1](https://raw.githubusercontent.com/0xdefaceme/whitepaper/master/assets/figure1.png)

The attacker hence calls `commit(IExploitable exploitable, uint256 damage)` on
the negotiator contract. `exploitable` here being the vulnerable contract and
`damage` the potential damage the vulnerability could cause. Note that in this
first iteration, we allow the attacker to single-handedly set the estimated
damage [footnote 5].

Calling `commit`, the negotiator contract will make sure that `IExploitable
exploitable` implements `implementsExploitable()` and that `damage` doesn't
exceed the exploitable's balance. Is this the case, then a new vulnerability
will be added and a `Commit(uint256 id, address exploitable, uint256 damage,
address attacker)` event will be emitted to inform the contract owner [footnote
6].

Note that at this point no information about the vulnerability has been put
online yet. As the vulnerability report will be encrypted using eth-ecies
(public-key cryptography), the contract owner first has to share a public key
with the attacker [10]. This is done by the contract owner calling `pay(uint
vulnId, string publicKey) public payable`. `pay` checks (1) if the
vulnerability exists (2) if an appropriate bounty was sent and (3) if the
sender is the exploitable contract. Is this the case, then the vulnerability is
marked paid, the public key is stored and a `Pay(uint256 id, address
exploitable, uint256 bounty)` event is emitted to inform the attacker.

As the contract owner's public key is now available to the attacker, the
vulnerability can be encrypted using a public-key encryption scheme.
Subsequently, the attacker uploads it to IPFS and shares the hash by calling
`reveal(uint256 vulnId, string hash)`. In case (1) the vulnerability exists,
(2) the attacker sends the vulnerability from the account that committed it and
(3) the vulnerability was previously paid for, the hash is revealed to the
contract owner through an event `Reveal(uint256 id, string hash)`.

Using the revealed hash, the contract owner can now download the report from
IPFS, decrypt it using their private key and study it. They now have two
options: (1) Decide to ignore the vulnerability and therefore send the
bounty back to themselves. (2) Decide to give away under the vulnerability and
hence shut down their contract. Is this the case, the negotiator sends the
bounty to the attacker's account. Nonetheless, the Negotiator emits a
`Decide(uint256 vulnId, bool decision)` event and closes the vulnerability.

In this section, we discussed a possible happy path of disclosing a
vulnerability. Of course, there are many other branches to it. To highlight
these, we discuss the incentives at play in the next section.

## Incentives

Attack vectors
- Attacker submits a wrong damage estimate
- Contract owner has dynamic bountyamount function
- Attacker never submits vulnerability in reveal (we need a time out)

Validation of incentives
Ultimately, 0xdeface's goal is to save as much ETH from getting stolen as
possible. While we proposed a concrete solution to this problem, we'll most
certainly have to experiment before actually launching.

## Business model and funding

## Conclusion

## Footnotes

1. In fact, we assume there to be a whole hidden community of diverse smart
   contract experts waiting to be spawned into existence on Etherscan's
   "Verified Contracts" page. In the future, 0xdeface's goal is to offer this
   community a proper home at 0xdeface.me.
2. Note that Ethereum smart contracts will be changeable in-place with the
   Constantinople update in February and the introduction of CREATE2.
3. Though currently we aren't aware of any on-going cases against smart
   contract attackers, we strongly assume it will be against the law in many
   jurisdictions. We assume this as there have been cases where stealing
   virtual goods has been deemed a crime before [7].
4. Even though Gnosis's DAOStack contracts were audited before deploying them
   to the Ethereum main net, they got drained as part of their extended bounty
   program within hours [3].
5. We acknowledge that setting `damage` to an incorrect value could potentially
   lead to an attack vector, where the attacker overstates the seriousness of a
   vulnerability. We see it, however, in the responsibility for the contract
   owner to do the math on the vulnerability. Is their decision to decline a
   legit vulnerability because of an incorrectly submitted `damage` estimate,
   so can the `decide`'s `string reason` be used to state so.
6. 0xdeface plans to deploy a Ethereum blockchain listener that contract owners
   can subscribe to via email to get the latest events emitted.

## References

1. [PEREZ, Daniel; LIVSHITS, Benjamin. Smart Contract Vulnerabilities: Does
   Anyone Care?. arXiv preprint arXiv:1902.06710,
   2019.](https://arxiv.org/abs/1902.06710)
1. https://medium.com/@ogucluturk/the-dao-hack-explained-unfortunate-take-off-of-smart-contracts-2bd8c8db3562
1. https://blog.gnosis.pm/security-update-on-the-dxdao-bug-bounty-52cec0f02cde
1. https://github.com/ConsenSys/mythril-classic
1. https://github.com/trailofbits
1. https://etherscan.io/contractsVerified
1. https://www.telegraph.co.uk/technology/video-games/9053870/Online-game-theft-earns-real-world-conviction.html
1. https://hackerone.com/augurproject
1. https://twitter.com/mythril_watch
1. https://github.com/libertylocked/eth-ecies
