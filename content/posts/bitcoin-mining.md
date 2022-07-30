---
title: "On proof of work"
date: 2022-07-11T22:24:52-04:00
draft: false
tags: [bitcoin]
---

When I first heard about Bitcoin, proof of work (PoW) sounded like the coolest and most mysterious concept tied to the protocol. Mainstream media articles trying to describe the PoW process always fell short of a proper explanation though. They usually read something like: "miners race to solve complex mathematical problems in order to validate the next block of transactions and earn bitcoins as a reward", which sounded cool and all but only added to the mystery and the confusion.

Miners are actually just trying to find a specific number called a nonce that when hashed together with the rest of the block header respects a specific constraint, which is that the produced block hash must be equal or less than another number called the target. This process can be seen as a form of restricted preimage attack, where an attacker tries to break the hashing algorithm by finding an input to a specific hashed output. SHA-256, the hashing algorithm used in PoW is preimage attack resistant though, which is what makes the PoW algorithmically hard to solve. That's why PoW is basically a brute force algorithm, because there is no better strategy than trying random numbers to find a hash that will respect the target. In Typescript that might look something like:

```typescript
import { createHash } from "crypto";

interface BlockHeader {
  ver: number; // version number to track software/protocol upgrades
  prev_block: string; // reference to the hash of the previous (parent) block in the chain
  mrkl_root: string; // hash of the root of the merkle tree of this block’s transactions
  time: number; // creation time of this block (seconds from Unix Epoch)
  bits: number; // encodes the target for this block
  nonce: number; // can be updated to generate a valid block hash
}

function hash(candidate: string) {
  return createHash("sha256").update(candidate).digest("hex");
}

function doubleHash(candidate: string) {
  return hash(hash(candidate));
}

function getTargetFromBits(bits: number) {
  const bitsInHex = bits.toString(16);
  const coeff = parseInt(bitsInHex.slice(2), 16);
  const index = parseInt(bitsInHex.slice(0, 2), 16);

  // based on Bitcoin core's original formula
  // https://github.com/bitcoin/bitcoin/blob/8e3c266a4f02093d57d563f32ba73d3ab4b5f208/src/arith_uint256.cpp#L198
  if (index <= 3) {
    return coeff * 2 ** (8 * (3 - index));
  } else {
    return coeff * 2 ** (8 * (index - 3));
  }
}

function isValidProof(hash, bits) {
    const target = getTargetFromBits(bits);

    return parseInt(hash, 16) =< target;
}

function mine() {
  const blockHeader: BlockHeader = getBlockHeader();
  const { ver, prev_block, mrkl_root, time, bits } = blockHeader;

  const blockMessage = `${ver}|${prev_block}|${mrkl_root}|${time}|${bits}`;

  let nonce = 0; // can also be initialized to a random number
  let candidateHash = doubleHash(`${blockMessage}|${nonce}`);
  while (!isValidProof(candidateHash, bits)) {
    nonce = nonce + 1;
    candidateHash = doubleHash(`${blockMessage}|${nonce}`);
  }

  // YAY you found a valid nonce, you can include it in your block as proof that you did the work
  blockHeader.nonce = nonce;
}
```

Why is this PoW process important and how is it related to the immutability of the blockchain?
The obvious thing you would like to do as an attacker is to tamper with an old block, maybe modify one of the transactions there to your advantage. Let's see what happens if one does that. As we saw in one of my previous posts the merkle tree root is a condensed hash of all the transactions in a block, so if one tries to modify a transaction's data, the merkle tree root hash will be updated. If one of the fields in the block header is modified then the block hash is also changed and will with overwhelming probability fail to respect the target. If the block hash doesn't respect the target then your block is invalid and bitcoin nodes will reject your tampered blockchain. The only way for you to convince other nodes that your chain is valid is to redo the work necessary to get a valid nonce. Moreover let's say you modified a transaction within the latest block, then you would need to redo the work for that last one. But if you forged a transaction located in the nth last block then you will have to redo the work for that nth block AND all the other blocks after that. Can you see why? It's because one of the fields part of the block header is the hash of the previous block, and so like with the merkle tree root hash if you update one of the crucial pieces belonging to the block header you need to redo the work.
Of course a motivated attacker could afford to redo all that work, but the crucial point to keep in mind here is that during all that time used to recompute new nonces and hashes, other blocks will be added to the original chain and nodes are tuned and incentivized to only look at the longest version of the chain. So as an attacker you are fighting a losing battle.

Now what happens if two miners find a new valid hash at the same time? What happens if miners get so fast and achieve such a high hash rate that for all intents and purposes it becomes infeasable for nodes to identify the correct chain because so many miners are churning new blocks at the same time? Well just adjust the target and make it more difficult to find a nonce one would say. And that's exactly what Bitcoin miners do, they adjust the target so that a new block is added every ten minutes to the chain on average. But then you could say, why would a miner care to adjust the difficulty target? After all it found a valid block. The answer here is simply that if the miner does that then it would not be using Bitcoin and other nodes would not validate that block because they would check the `bits` header field and see that it's too low. In that sense one can see that Bitcoin is as much a protocol than it is a network, an ecosystem of computers playing by a certain set of rules.

I hope you are now starting to get a general picture of how PoW helps the blockchain achieve immutability and how it ties in with the rest of the protocol to make an elegant system function and ensure that no single bad actor can tamper with existing transactions.
