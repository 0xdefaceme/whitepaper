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
to a contract's owner We argue why we think this will transform contract
attacking from a negative-sum game to a positive-sum game. 0xdeface's goal is
to make contract attacking a worthwhile occupation while providing a secure
protocol for owners to settle their contracts fairly by returning users' funds.

## Introduction

Smart contracts represent a paradigm shift in programming. Ever since their
practical implementation through Ethereum, they allow for permissionless and
transparent transfer of monetary value. As their advent is still recent, many
best-practices are under active development and will take years to stabilize.

In Ethereum, smart contracts are written in Solidity or Vyper. Both being
respectively inspired by languages from the traditional software world:
JavaScript and Python. In both Solidity's and Vyper's case, the base language
was slightly adjusted to feature new primitives making transfer of monetary
value possible.  Not always with great success, however.  Especially Solidity's
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
improve the situation from an ecosystem-wide negative-sum game to a
positive-sum game.

## Motivation

With the release of automated auditing suites like Consensys's Mithril [4] and
TrailOfBits's ManiCore [5], documentation of common vulnerabilities and the
growing monetary value at stake, we'll see the frequency and ingenuity of smart
contract attacks increase in the future. 

In this section, we hence discuss the motives at play for (1) why it is
seemingly rational for an attacker to target smart contracts, (2) why it is
difficult to maintain smart contracts and (3) why the situation leads to a
negative-sum game. Furthermore, we motivate why there should be an EIP standard
and marketplace for consensually exchanging vulnerabilities between attackers
and contract owners. To begin with, we elaborate reasons for why it is
seemingly rational for an attacker to target smart contracts.

Ever since the dawn of Bitcoin and Ethereum, there has been a commonly shared
conviction of the community that transparency, decentralization and
permission-less innovation is the key to success of the respective ecosystems.
For several reasons, developing a smart contract as closed source is hence
considered a faux-pas. (1) Users should be able comprehend the full
functionality of a smart contract (transparency). (2) Nobody must be able to
exercise full control over a contract (decentralization). (3) Everybody must be
be allowed, without the incurrence of a cost, to innovate on any part of the
infrastructure (permission-less). It is therefore, and for other miscellaneous
reasons, a best practice in the ecosystem to open source smart contracts. For a
smart contract attacker, however, this yields a positive side effect. Namely,
the source code being easily accessible.

0xdeface ran a several week long experiment where we, using an Ethereum full
node, audited and crawled the Ethereum blockchain and GitHub for newly uploaded
smart contracts. Through leaving their `/build` folder in their GitHub
repository, contract repositories could simply be found by their address using
the GitHub search API.  While the setup of this experiment was trivial, it
yielded many potential opportunities where contracts were vulnerable. In many
cases, Mythril, the automatic auditing tool, even hinted towards the specific
function and vulnerability to exploit [9].  Even more trivial than that is of
course to occasionally scan Etherscan's "Verified Contracts" page [5] for smart
contract source code [footnote 1]. All this to say that while open sourcing
smart contracts might increase security ecosystem-wide, it may not necessarily
for the individual contract.

Once an attacker has access to a contract's source code, they can setup a local
environment using tools like Ganache, Truffle and Remix IDE. This local setup
is then used to simulate attacks against the contract. Is a critical
vulnerability found, then the attacker can - given the deterministic nature of
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

While in the traditional software world vulnerabilities can be fixed in minutes
by identifying, fixing and redeploying the software with the same state,
Ethereum's immutability property makes mitigating vulnerabilities non-trivial.
For many businesses in the ecosystem, smart contract vulnerabilities might in
fact be a worst-case scenario. We hence conclude that maintaining deployed
contracts is, and will be, difficult.

Which brings us to why attacking smart contracts is currently a negative-sum
game.  Firstly, we argue why attackers may not be incentivized to attack
contracts.  Secondly, we highlight the problems of contract owners suffering an
attack.  Thirdly, we state the role of the user in the situation.

Attacking contracts, especially ones of widely-known projects, often comes with
several negatives for the attacker. (1) Increased (social) media coverage and
community vigilance can lead to the hacker getting exposed.  (2) Through
crowd-sourcing the attacker's addresses, the stolen founds can effectively be
frozen by exchanges, making contract attacking a negative-sum game.
Essentially, the stolen ETH is taken out of the circulating supply. (3) Stolen
funds can most likely not be exchanged into fiat, as most exchanges require KYC
for larger amounts. (4) An attacker runs the risk of being prosecuted by the
law as stealing users' funds is very likely a criminal offense in many
jurisdictions [footnote 3]. These, amongst others, may be the reasons for why
only 0.3% of vulnerable funds have been claimed by attackers [1].

For a contract owner, the situation is equally tricky. (1) Bounty programs are
often run before a contract gets deployed. After deployment, the contract quite
literally _becomes_ the bounty [8]. (2) As seen recently, audits can only limit
the risk of vulnerabilities but not totally eradicate them [footnote 4]. (3)
And as stated before, successful attacks to deployed contracts are often
businesses' worst-case scenarios, going so far as to the total shutdown of the
operation (see: The DAO) [2].

In all of this, the contract users are probably off worst. They potentially use
all their funds and often have no way to participate in the governance
decisions taken by a contract owner under fire.  With regulation lagging
behind, there is also a significant lack in accountability. Who's to blame for
the loss of user funds? The attacker? The contract owner?

Hence as the stolen ETH is essentially taken out of circulation, we conclude
the situation to be a negative-sum game. It is at this point that we introduce
0xdeface. 0xdeface or EIP-XXXX is a standard to settle vulnerable smart
contracts fairly in favor of users and developers. Auditors confidentially
submit disclosures to 0xdeface.  Contract owners review disclosures. Do auditor
and contract owner agree that a critical vulnerability has been found, then a
contract can be settled fairly by returning its users' funds. Auditors get
rewarded with a bounty held in escrow by 0xdeface's Negotiator. 0xdeface's goal
is to make attacking Ethereum smart contracts a positive-sum game.

## Overview

0xdeface's incentive game consists of multiple components. In this section we
define what these components are and how they form an incentive game.

### Negotiator

The _Negotiator_ is 0xdeface's central component. It is an Ethereum smart
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

Another central component is 0xdeface's _Exploitable_ EIP standard. It is an
optional interface for developers to implement, potentially preventing their
contracts from getting drained. Exploitable currently consists of seven
functions. These being: 

1. `exploitableVersion()`: Returning the version of the incentive game
1. `implementsExploitable()`: Checking the contract's compatibility
1. `exploitableReward()`: Returning the attacker's potential reward in Wei
1. `pay(uint256 vulnId, string publicKey)`: For the owner to submit their
   public key along with the attacker's reward
1. `decide(uint256 vulnId, bool decision)`: For the owner to decide on the
   criticality of a vulnerability
1. `restore()`: Invoked when an owner decides to ignore a vulnerability; and 
1. `exit()`: Invoked when an owner decides to shut down the contract due to a
   vulnerability.

In one of the following sections, we discuss how developers will profit from
implementing the Exploitable EIP standard.

### 0xdeface.me Website

0xdeface.me serves as a hub of exchange for all stakeholders (owners, attackers
and contract users). We believe that there is a large untapped potential in
improving the security of Ethereum smart contracts by gamifying the experience.
0xdeface.me allows contract owners to confidentially negotiate with attackers,
attackers to be rewarded fairly and users to be able to participate in
governance decisions. In the process of building this infrastructure, it is
0xdeface's goal to nurture a community of security professionals by providing
tools for collaboration and automation.

Now that we highlighted the main components, we walk through the process of
committing and eventually exiting a vulnerability.

## Process

This section outlines the interactive process of _commiting_, _paying_,
_revealing_ and _deciding_ on a vulnerability. As it serves the purpose to
further understanding about *how* the system works, we're going to ignore
incentives for now. To simplify, we're instead going to make a few assumptions.
(1) The exploitable contract implements the Exploitable EIP standard. (2) The
attacker is willing to participate in 0xdeface's incentive game. Additionally,
we're only going to describe one happy path here. We'll discuss incentives and
attack vectors in one of the following sections.

As figure 1 illustrates, the process starts with the attacker finding a
vulnerability in the exploitable contract.

![figure 1](https://raw.githubusercontent.com/0xdefaceme/whitepaper/master/assets/figure1.png)

The attacker hence calls `commit(IExploitable exploitable, uint256 damage)` on
the negotiator contract. `exploitable` here being the vulnerable contract and
`damage` the potential damage the vulnerability could cause (denoted in "Wei").
Note that in this first iteration, we allow the attacker to single-handedly set
the estimation [footnote 5].

Calling `commit`, the negotiator contract will make sure that `IExploitable
exploitable` implements `implementsExploitable()` and that `damage` doesn't
exceed the exploitable's balance. Is this the case, then a new vulnerability
will be added and a `Commit(uint256 id, address exploitable, uint256 damage,
address attacker)` event will be emitted to inform the contract owner [footnote
6].

Note that at this point no information about the vulnerability has been put
online yet. As the vulnerability report will be encrypted using eth-ecies
(public-key cryptography), the contract owner first has to share a public key
with the attacker [10]. This is done by the contract owner calling `pay(uint256
vulnId, string publicKey) public payable`. `pay` checks (1) if the
vulnerability exists (2) if an appropriate bounty was sent and (3) if the
sender is the exploitable contract. Is this the case, then the vulnerability is
marked paid, the public key is stored and a `Pay(uint256 id, address
exploitable, uint256 bounty)` event is emitted to inform the attacker.

As the contract owner's public key is now available to the attacker, the
vulnerability can be encrypted using a public-key encryption scheme.
Subsequently, the attacker uploads it to IPFS and shares the hash by calling
`reveal(uint256 vulnId, string hash)`. In case (1) the vulnerability exists,
(2) the attacker has sent the vulnerability from the account that committed it
and (3) the vulnerability was previously paid for, the hash is revealed to the
contract owner through an event `Reveal(uint256 id, string hash)`.

Using the revealed hash, the contract owner can now download the report from
IPFS, decrypt it using their private key and study it. They now have two
options: (1) Decide to ignore the vulnerability and therefore send the bounty
back to themselves. (2) Decide to give away under the vulnerability and hence
shut down their contract. Do they decide to give in, then the negotiator sends
the bounty to the attacker's account. Nonetheless, the Negotiator emits a
`Decide(uint256 vulnId, bool decision)` event and closes the vulnerability.

In this section, we discussed a possible happy path of disclosing a
vulnerability. Of course, there are many other branches. To highlight
these, we discuss the incentives at play in the next section.

## Incentives

Earlier in this paper, we made the claim that 0xdeface transforms attacking
smart contracts from a negative-sum game to a positive-sum game. As there are
too many considerable human factors involved in the decision to maliciously
drain a contract's funds, it would not make sense to outline a mathematical
model of the problem here. Instead, this section makes an argument for why
attackers and contract owners should use the 0xdeface protocol and why this
transforms attacking contracts to a positive-sum game. We'll start by making
the case for why contract owners should implement the exploitable EIP standard.

By implementing the exploitable EIP standard, contract owners now have the
option to dynamically set bounties on a main net contract. While there is of
course a cost involved in offering this bounty to attackers, it comes with
several benefits: (1) It can be grown dynamically with the size of the
contract's balance. (2) A vulnerability may not be the worst-case scenario for
the contract owner's business anymore. (3) Shutting down and redeploying
contracts becomes a canonical process. (4) Vulnerabilities are disclosed
directly to the contract owner and do not end up on secondary markets instead.
(5) And investors and community are reassured that they're funds are insured.
In case of a shut down, they'll likely not become angry or hesitant to invest.

Secondly, attacking smart contracts becomes an attractive occupation. (1) An
attacker doesn't have to commit a potential crime anymore [footnote 7].  (2)
The attacker doesn't have to worry about public exposure, as they're not
stealing the money of investors anymore. In fact, the protocol allows for a
"Proof of Attacking". It gives the attacker the possibility to increase their
credibility within the community by being able to prove that they're the
attacker. We believe, this will foster an agile hacker community with quickly
evolving best practices. (3) Most importantly, however, the attacker's
scavenged funds are not in danger of being blacklisted anymore. As they were
acquired legally, they can be e.g. exchanged for fiat to pay for living
expenses.

Lastly, users' funds are secured through the 0xdeface protocol. As an attacker
will be more inclined to scavenge the bounty than draining the contract, user
funds will not be affected by the disclosures of vulnerabilities. In addition,
the exploitable EIP standard is generic enough to allow users to vote on how to
handle a vulnerability, e.g. via Aragon Agent [11]. This gives the option to
make users part of the governance process in case of a imminent settlement.

We lined out several arguments how 0xdeface protocol can provide utility to
users, attackers and contract owners. As we believe the gained utility to be of
higher value than the cost of paying bounties to attackers, we argue that
0deface protocol will transform contract attacking to a positive-sum game.  In
the next section, we'll elaborate on the potential attack vectors of the
protocol and how we plan to mitigate them.

### Attack Vectors

This section will list all known attack vectors of the protocol and answer how
we plan to mitigate them. If, in the future, more attack vectors become known,
we'll update this list.

#### 1. What if an attacker doesn't want to participate in the 0xdeface protocol?

We believe we've made a compelling case for why an attacker should participate
in the 0xdeface protocol in the previous sections. If an attacker wants to
drain a contract implementing the Exploitable EIP standard anyways, they can of
course do so. Ultimately, 0deface protocol doesn't have any mechanism to
prevent irrational actors from doing damage.

We believe that there's a significant necessity of experimentation needed for
0xdeface protocol to find the right parameters. At this point we can only
speculate about the optimal ratio between a contract's balance and the 0xdeface
bounty. Is 10%, e.g. 100€ bounty for a contract with a balance of 1000€ enough?

0xdeface is committed to experiment with those parameters before launching to
give contract owners reliable heuristics.

#### 2. What if an attacker submits a vulnerability but never reveals it?

All vulnerability structs have a timeout property. In case an attacker submits
a vulnerability, it's being paid by the contract owner but the hacker never
calls `reveal()`, a timeout expires that allows the contract owner to reclaim
the paid bounty.

#### 3. What if a contract owner decides to ignore a legit vulnerability?

A contract owner can of course decide to do so. In that case, however, their
contract is at risk of getting drained by the attacker illegally.

#### 4. What if a contract owner is able to adjust the value of `exploitableReward` mid-process?

The Exploitable EIP standard is not opinionated about the implementation of
`exploitReward`. A contract owner could hence implement an adjustable
`exploitReward` function that when called on `pay` could yield a lower than
expected bounty. Attackers should hence always audit the Exploitable EIP
standard functions carefully to make sure, they're not tricked in giving away a
vulnerability for free.

#### 5. What if an attacker maliciously sets the damage estimate to a higher than legit value?

It's of course in the contract owner's responsibility to make sure the damage
is correctly estimated by the attacker. As the `damage` will ultimately dictate
the bounty payout the attacker receives, the attacker might be tempted to set
the `damage` estimate to a higher value. If this is the case, we recommend the
contract owner to decline the vulnerability with e.g. `string reason = "Damage
value too high/low"`. This will allow the attacker to submit the vulnerability
again but with a correct `damage` estimate.

#### 6. What if two critical vulnerabilities are found at the same time?

It's in the contract owner's discretion to sort this out. If there are more
than two vulnerabilities found at the same time, overall having a greater
damage estimate than the exploitable's balance then, we recommend the contract
owner to simply process them sequentially. There might additionally be the case
where a vulnerability cannot be submitted as there is no bounty-ETH stocked in
the exploitable contract. In that case, the attacker should wait for the ETH to
be restocked by the contract owner.

#### 7. What if a legit vulnerability is committed but the contract owner choses to audit the contract code themselves instead of paying the bounty?

Assume an attacker finds a vulnerability and commits it. A contract owner could
now choose to not `pay` for the vulnerability and audit the contract code
themselves. If they have enough time, they can find the vulnerability and shut
down the contract in a controlled manner.

There's a couple of reasons why we believe that it is not in the interest of
the contract owner to engage in this behavior: (1) While taking their time to
audit the contract, the attacker could drain it illegally. (2) In order to
execute a controlled shut down, the attacker would have to a implement a
non-compliant shut down procedure before deploying the contract. As attackers
would immediately spot this, they'd be suspicious and hesitant to commit a
vulnerability in the first place.  In fact, the contract owner runs the risk of
having their contract drained illegally as they're considered to not playing
the game fairly. (3) Assume a contract owner implements a non-compliant shut
down procedure and an attacker submits a vulnerability anyway. If now the owner
decides to shut down the contract in a non-compliant way, this will attract the
attention of users and other attackers. They'd be able to see that the contract
owner is not playing the game fairly. We believe that in this case, the
contract owner's reputation would suffer significantly. To the point, in fact,
where their future contracts would become untrusted by attackers.  Instead of
committing vulnerabilities they'd simply drain future contract's illegally.

Even though we believe the arguments above to be sufficient to stop this
behavior from happening, there is a couple of things the protocol can help the
attackers with.

In the 0xdeface protocol an attacker is able to submit a hash of their
unencrypted vulnerability report along with their `commit`. While this doesn't
expose the report, it allows the attacker to prove publicly that they submitted
a legit vulnerability at a certain point in time (time-stamping).  Does the
contract owner in fact shut down the contract using a non-compliant procedure,
then the attacker can reveal their time-stamped report and prove to the world
that the owner didn't play the game fairly. We believe this would yield a
backlash in the community.

In cases where contract owners decide to audit the code themselves on reception
of a vulnerability report, the attacker could also choose to announce the
release of the time-stamped and unencrypted report publicly after e.g. 24
hours. While we're unsure about the legality of this (sounds a lot like
blackmailing), it would pressure the contract owner into paying for the
vulnerability and fairly exiting the contract.

Lastly, assume an attacker could encrypt a file such that it takes e.g. 24
hours on any CPU to decrypt (non-serializable computation, VDFs). An attacker
could commit a time-locked vulnerability report to the negotiator and hence
pressure the contract owner into making a decision within 24 hours. To our
knowledge, such encryption is unfortunately not practically possible at this
point. It is, however, a part of on-going research [11].

In this section, we gave an overview of the incentives built into the 0xdeface
protocol. As we now covered all technical aspects of it, we move on to
explaining how we intent to build the protocol.

## Limitations

We acknowledge that fact that the economic incentives laid out in this document
might not work as intended. We're especially concerned about attackers simply
not being interested in playing 0xdeface's game as there's more monetary value
to gain from simply draining a contract illegally. It is our strong conviction,
however, that there's generally more attackers out there willing to make a
legal buck than an illegal.

At last, only experimentation and utilization will tell if this is the case. To
make this happen, 0xdeface is looking for funding.

## Funding

The 0xdeface protocol is currently being build by Tim Daubenschütz. Tim is
actively looking for grants to sponsor his work. Here are some reasons on why
you should fund Tim's work:

> Hi, I'm Tim. Ever since discovering Bitcoin, I've been enthusiastic about
> distributed systems. That's why in 2014 I joined the blockchain space as a
> front end developer for ascribe.io (now BigchainDB / Ocean Protocol).  There
> I built infrastructure for digital artists on Bitcoin. With the company
> shifting focus towards BigchainDB, my role changed to product manager.  Very
> quickly I became the central interface for external and internal developers
> building on BigchainDB, managing a team of ten. During that time also got the
> opportunity to generalize our efforts with ascribe.io. In 2016, I proudly
> co-authored COALA IP, a blockchain-ready protocol for intellectual property
> licensing.
> When BigchainDB shifted focus again in 2017, I helped prepare the company to
> launch their token, shadowed much of my CTO's work and helped to write the
> early drafts of what would later become the Ocean Protocol technical
> whitepaper.
> After this crazy but fun ride, I took half a year off to recalibrate myself
> and to recharge my batteries. As I had a lot of free time at hand I finally
> got to work on all the projects I had postponed over the years. In 2018, I
> hence built my own blockchain in Golang, ipfs-converter.com,
> mycollectibles.io, gave workshops to refugees, backpacked China and learned
> to play the piano. I deeply care about the future of the web, security,
> decentralization, open source and permission-less innovation.  Working on
> 0xdeface protocol would be a dream come true.

For inquiries email: tim@0xdeface.me or meet me over a delicious Sterni
anywhere in Berlin.

## Monetization

We're big fans of projects like Uniswap [12]. Uniswap is an Ethereum Foundation
grant recipient. It doesn't require a token and comes with no artificially
added cost for users. That's awesome! We worry though that without at least a
minimum viable business model Uniswap will eventually face funding problems.

We hence propose the following to maintain and develop 0xdeface protocol: A
small fee (approx. 5%) of every bounty paid out from 0xdeface's negotiator. As
this fee would be trivially forked out of the protocol, we make it optional.

## Conclusion

We propose 0xdeface, a fair and secure protocol to settle Ethereum smart
contracts in case of vulnerabilities and a path towards making contract
attacking a worthwhile occupation and a positive-sum game.

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
   lead to an attack vector, where the attacker overstates the criticality of a
   vulnerability's damage. We see it, however, in the responsibility for the
   contract owner to do the math on the vulnerability. Is their decision to
   decline a legit vulnerability because of an incorrectly submitted `damage`
   estimate, so can `decide`'s `string reason` be used to state so.
6. 0xdeface plans to deploy an Ethereum blockchain listener that contract
   owners can subscribe to via email for updates on the latest emitted events.
7. Note that the 0xdeface protocol is consensual as only contracts implementing
   the exploitable EIP standard can be attacked. 

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
1. https://uniswap.io/
1. https://www.gwern.net/Self-decrypting-files
