# Outline｜Ethereum's Consensus Protocol

### What is Consensus

#### Background
+ The Byzantine Generals Problem
    + Background
        + In a 1982 [paper](https://lamport.azurewebsites.net/pubs/byz.pdf), **Leslie Lamport** 
        + Basic idea: abstract
    + The problem
        ![](https://i.imgur.com/ZguVyxt.png)
        + The army are **divided** into several generals
        + Generals communicate **only** through **messenger**
        + Generals must decide **a common plan** of action
        + Howevery, some of generals are **traitors**, trying to **prevent** the loyal generals from **reaching agreement**
        + The generals must have an **algorithm** to guarantee👇
            1. All loyal generals decide upon the **same plan** of action (attack or retreat)
            2. A small number of traitors **cannot** cause the loyal generals to **adopt a bad plan**
    + The definitions
        + BGP
            + as above👆
        + BFT
            + Byzantine faults
                + traitor generals
                + malicious nodes
            + Byzantine fault tolerant
                + Any algorithms that can tolerate Byzantine faults
            + Some BFT algorithms
                + \> 51% general votes algorithm
                + pBFT
                    ![](https://i.imgur.com/igJbQ0f.png)
                    + in a 1999 [paper](https://www.scs.stanford.edu/nyu/03sp/sched/bfs.pdf)
                    + consists of one leader node (sending requests) and backup nodes (voting)
                    + Communication is fast. All nodes reach the result at the same time (but the number of nodes cannot be too many)
                    + High barriers to entry (participant list)
                    + Can tolerate Byzantine faults, allowing the 33% malicious nodes
                + PoW
                + Casper FFG
                + Gasper
    + Reference
        + [Eth2 Book](https://eth2book.info/bellatrix/part2/consensus/preliminaries/#byzantine-generals)
        + [Byzantine Generals Problem Paper](https://lamport.azurewebsites.net/pubs/byz.pdf) (introduction part)
        + Bitcoin Forum - [BFT](https://bitcoin.stackexchange.com/a/97102/136952)
+ Sybil attack
    + The definition
        + An attack by creating vast numbers of **duplicate identities** at a low (or zero) cost
    + Example
        + Background
            + \> 51% algorithm
            + If you run a software (open sourced) you can be a general and **vote**
        + Execute attack
            + You can make **many virtual machines** and run this "general software"
            + And control voting of all machines
            + Now you create many duplicate identities at a very low cost
            + Vote, take over the system
            ![](https://i.imgur.com/3o4egDW.png)
    + Reference
        + [Wikipedia](https://en.wikipedia.org/wiki/Sybil_attack)
#### The definitions
+ What is "consensus"
    + The definition
        + Consensus is a computer science problem of achieving system-wide **agreement** in the presence of **crash failure**/**Byzantine faults**
            + computer process crash
            + traitor generals
    + Reference
        + [Wikipedia](https://en.wikipedia.org/wiki/Consensus_(computer_science))
+ What is PoW and PoS
    + The definition
        + Proof-of-Work and Proof-of-Stake are essentially **Sybil resistance mechanisms**
        + Prevent duplicate identities with **limited resources** (hash power, network's coin)
    + Other Sybil resistance mechanism
        + [One-human-one-vote](https://archive.devcon.org/archive/watch/6/1-human-1-vote-money-legos-more-democratic-daos/?playlist=Devcon%206&tab=YouTube)
    + Reference
        + Eth2 Book ([PoW/PoS](https://eth2book.info/altair/part2/consensus/preliminaries#proof-of-stake-and-proof-of-work), [Sybil](https://eth2book.info/altair/part2/incentives/staking#introduction))
+ What is consensus protocol
    + The definition
        + The consensus protocol is **an agreed suite of algorithms** for a set of entities (nodes, people, etc) to follow
            + a set of algorithms
            + defined into protocol
    + Reference
        + Gasper [Paper](https://arxiv.org/pdf/1710.09437.pdf) (2.1 part)
          ![](https://i.imgur.com/6IlZE8Z.png)
+ What is consensus mechanism
    + The definition
        + The consensus mechanism is the **combination** of all components
            + Contains protocol-level (code) and non-protocol level (social layer)
            + PoS, consensus protocols, economic incentives, and community legitimacy are **all part of** the consensus mechanism
    + Reference
        + [Ethereum org](https://ethereum.org/en/developers/docs/consensus-mechanisms/#what-is-a-consensus-mechanism)

### What is the consensus protocol of Ethereum

#### Background
+ Safety and Liveness
    + Background
        + Derived from CAP Theorem
            + client-servers model
            + Consistency, Availability, Partition-tolerance
        + CAP Theorem beacomes a more general framework
            + The impossibility of guaranteeing both safety and liveness in an unreliable distributed system
            + safety, liveness, unreliable
    + Safety
        + The definition
            + Informally, an algorithm is safe if nothing bad ever happens
        + In blockchain context
            + Consistency
                + If we were to ask different (honest) nodes about **the state of the chain** at some point, then we should always get **the same answer**, no matter which node we ask
                + The blockchain transaction history will not contain two **conflicting** blocks
    + Liveness
        + The definition
            + By contrast, an algorithm is live if eventually something good happens
        + In blockchain context
            + The chain can always add a new block
    + In practice
        + Ethereum prioritises liveness
        + Bitcoin only has Liveness guarantee, no Safety guarantee
+ Reference
    + [Eth2 Book](https://eth2book.info/bellatrix/part2/consensus/preliminaries/#safety-and-liveness)
    + Perspectives on the CAP Theorem [Paper](https://groups.csail.mit.edu/tds/papers/Gilbert/Brewer2.pdf)
    + Gasper Paper (2.5 part)
      ![](https://i.imgur.com/erEpMlT.png)
      
#### General intro
+ General intro
    + The Proof-of-Stake (PoS) Ethereum consensus protocol is constructed by applying the finality gadget **Casper FFG** on top of the fork choice rule **LMD-GHOST**
+ Reference
    + Three Attacks on Proof-of-Stake Ethereum [Paper](https://arxiv.org/pdf/2110.10086.pdf) (introduction part)
    + Gasper [Paper](https://arxiv.org/pdf/1710.09437.pdf)

#### Fork-choice rule
+ The defination
    + The fork-choice rule is the rule for **choosing a branch** when a blockchain **fork** ocuurs
    + Subsequent blocks will be added to this chain (branch)
      ![](https://i.imgur.com/bQfPoNw.png)
+ Bitcoin and PoW Ethereum's fork-choice rule
    + They use **the longest chain rule**
      ![](https://i.imgur.com/OoBH5N9.png)
    + The longest chain represents **the most cumulative "work" done** under proof of work
    + New blocks continue to be **added** to the longest chain (highest block), which is "liveness" too
+ GHOST
    + The background
        + GHOST was proposed in a 2015 [paper](https://eprint.iacr.org/2013/881.pdf) to improve Bitcoin's fork choice rules
        + Many people say that GHOST is also used in PoW Ethereum, but Ethereum **never** actually implemented it
    + The definition
        + The **heaviest chain rules**, the chain with the most votes becomes the **canonical chain**
+ LMD-GHOST
    + The background
        + LMD-GHOST is a variant of GHOST that only considers the latest vote (LMD = latest message driven)
        + Proposed in a 2017 [paper](https://github.com/vladzamfir/research/blob/master/papers/CasperTFG/CasperTFG.pdf), V.zamfir tries to combine GHOST with Casper
    + The definition
        ![](https://i.imgur.com/5rwqLht.png)
        + Calculate the block with **the most votes**, return it as **the head of the chain**, and add subsequent blocks to this block
        + Number of votes = block + it's descendants
    + The workflow
        ![](https://i.imgur.com/BDuvAhZ.png)
        + Start at the genesis block
        + Each time there is a fork, **choose** the block’s subtree where more of LMs (latest messages) are supported
        + Repeat, until you research a block **with no children** then return this block as the head of the chain
+ Reference
    + [Eth2 Book](https://eth2book.info/bellatrix/part2/consensus/preliminaries/#fork-choice-rules)
    + [A CBC Casper Tutorial](https://vitalik.ca/general/2018/12/05/cbc_casper.html)
    + Casper FFG and Gasper papers (LMD-GHOST part)
#### Finality gadget
+ Safety
    + Bitcoin and PoW Ethereum have no "finality gadget" and “security guarantees”
        + Whoever constructs the new longest chain can change the previous blocks
    + Under the Casper FFG, the finalized block of PoS Ethereum cannot be modified
        + Requires huge attack cost, $\frac{2}{3}$ of the total staking amount
+ Casper FFG
    + The background
        + Based on the pBFT algorithm, but with many new features
            + Punishment mechanism
                + slashing conditions
            + Permissionless system
                + dynamic validators
            + Modular overlay
                + clean and have a modular design
    + Main components
        + Checkpoint tree
            + Casper is designed to work with a wide class of **blockchain protocols** with **tree-like structures**
            + Checkpoint tree
                + Checkpoint block
                    + The blocks whose height is a multiple of a constant H
                    + For example, H = 100, blocks = 0, 100, 200, 300, ...
                    + In Ethereum, H = 32
                + Checkpoint tree
                    ![](https://i.imgur.com/WPBiM6r.png)
                    + Finality on Checkpoint blocks
                    + Checkpoint blocks form a Checkpoint tree
            + Future: single slot
                + There is a research direction to reduce H to 1
                + In this way, each block (slot) is a checkpoint block
                + Can speed up finality time (from 32 slots to 1 slots, 384s to 12s)
        + Checkpoints pair
            + The definition
                ![](https://i.imgur.com/NgNNtzo.png)
                + $checkpoint(s,t)$
                    + The pair is a link from source checkpoint to target checkpoint 
                    + Pair is the content of validator votes (i.e. attestations)
            + Supermajority Link
                + Pairs with $\frac{2}{3}$ staker votes
        + Justification and Finalization
            + Genesis Block
                + The genesis block is justified/finalized by default
            + Justification
                + a checkpoint $c$ is called justified if 
                    + (1) it is root
                    + (2)there exits a supermajority link $c' \to c$ where checkpoint $c'$ is **justified**
            + Finalization
                + a checkpoint $c$ is finalized if and only if
                    + (1) it is root
                    + (2) checkpoint $c$ is **justified**, and there exits a supermajority link $c \to c'$, checkpoints $c$ and $c'$ are not conflicting, and $h(c')=h(c)+1$ 
                + Finalized blocks as part of the canonical chain
        + Slashing conditions
            + Note
                + Validator violations will be punished
                + All violating validators can be traced (by signature)
            + The definition
                ![](https://i.imgur.com/oA4a9VI.png)
                + A validator must not publish two distinct votes for the same target height
                + A validator must not vote within the span of other votes
            + My comments
                ![](https://i.imgur.com/KbH4z2c.png)
+ Reference
    + [medium](https://medium.com/unitychain/intro-to-casper-ffg-9ed944d98b2d)
    + Casper FFG paper
    + Gasper paper (Casper FFG part)
#### Gasper
+ Re-call "general intro"
    + The Proof-of-Stake (PoS) Ethereum consensus protocol is constructed by applying the finality gadget **Casper FFG** on top of the fork choice rule **LMD-GHOST**
+ Overview
    ![](https://i.imgur.com/gv9HwVp.png)
    + Proposer proposal and run fork-choice rule to add new block
    + Attesters publishe the attestations and broadcast
        + For each slot
        + For checkpoint pair, after two rounds, new epoch will be finalized and be a part of canonical chain
+ How it works
    + Committees
    + Proposer and attester
        + proposer run LMD-GHOST
    + Justification
    + Finalization
    + Done
+ Reference
    + Gasper Paper (4, 8, 9 sections)

### Reference
+ Eth2 Book
+ BGP paper
+ pBFT paper
+ CAP paper
+ Casper FFG
+ Gasper paper
