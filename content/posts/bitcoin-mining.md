---
title: "On proof of work"
date: 2022-07-11T22:24:52-04:00
draft: false
tags: [bitcoin]
---

When I first heard about Bitcoin, proof of work (PoW) sounded like the coolest and most mysterious concept tied to the protocol. But mainstream media articles trying to describe the PoW process always fell short of a proper explanation for me. They usually read something like: _"miners race to solve complex mathematical problems in order to validate the next block of transactions and earn bitcoins as a reward"_, which sounded cool and all but only added to the mystery and the confusion.

Miners are actually just trying to find a specific number called a nonce that when hashed together with the rest of the block header respects a specific constraint, which is that the produced block hash must be equal or less than another number called the target. This process can be seen as a form of restricted [preimage attack](https://en.wikipedia.org/wiki/Preimage_attack), where an attacker tries to break the hashing algorithm by finding an input to a specific hashed output. SHA-256, the hashing algorithm used in PoW is preimage resistant though, which is why PoW is algorithmically hard to solve. PoW is basically a brute force algorithm, because there is no better strategy than trying random numbers to find a hash that will respect the target. In Typescript that might look something like:

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

function hash(candidate: string): string {
  return createHash("sha256").update(candidate).digest("hex");
}

function doubleHash(candidate: string): string {
  return hash(hash(candidate));
}

function getTargetFromBits(bits: number): number {
  const bitsInHex = bits.toString(16);
  const coeff = parseInt(bitsInHex.slice(2), 16);
  const index = parseInt(bitsInHex.slice(0, 2), 16);

  // based on Bitcoin core's original formula
  // https://github.com/bitcoin/bitcoin/blob/8e3c266a4f02093d57d563f32ba73d3ab4b5f208/src/arith_uint256.cpp#L198
  if (index <= 3) {
    return coeff * 2 ** (8 * (3 - index));
  }

  return coeff * 2 ** (8 * (index - 3));
}

function isValidProof(hash: string, bits: number): boolean {
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

  // ... other complicated things
}

mine()
```

### Immutability

Why is this PoW process important and how is it related to the immutability of the blockchain?

The obvious thing you would like to do as an attacker is to tamper with an old block, maybe modify one of the transactions there to your advantage (assuming you even can since transactions are digitally signed hence unforgeable) or at least do one of those infamous [double spends](https://www.youtube.com/watch?v=hixM4u7ep58). Let's see what happens if one tries to do that.

As we saw in a previous [post](https://trebaud.github.io/posts/merkle-tree/) the Merkle tree root is a condensed hash of all the transactions in a block, so if one tries to modify a transaction, delete or add a new one, the Merkle tree root hash will get updated. If one of the fields in the block header is modified then the block hash is also changed, and will with overwhelming probability fail to respect the target. If the block hash doesn't respect the target then your block is invalid and bitcoin nodes will reject your tampered blockchain. The only way for you to convince other nodes that your chain is valid is to redo the work necessary to get a valid nonce. Moreover let's say you modified a transaction within the latest block, then you would need to redo the work for that last one, but if you forged a transaction located in the nth last block then you will have to redo the work for that nth block AND all the other blocks after that. Can you see why? It's because one of the fields in the block header is the hash of the previous block, and so like with the Merkle tree root hash if you update one of the crucial pieces belonging to the block header you need to redo the work.

Of course a motivated attacker could afford to redo all that work, but the crucial point to keep in mind here is that during all that time spent on recomputing new nonces and hashes, other blocks will be added by other miners to the original chain and nodes are tuned and incentivized to only care about the longest version of the chain (because by doing that they have higher chances of earning bitcoins, more on that later). So as an attacker you are fighting a losing battle.

### Difficulty adjustment

Now what happens if two miners find a new valid hash at the same time? What happens if miners achieve such a high hash rate that for all intents and purposes it becomes impractical for all nodes to agree on a single chain because so many miners are churning out new blocks at such a fast pace that they can't propagate in time through the network to reach consensus? Well just adjust the target and make it more difficult to find a nonce one would say. And that's exactly what Bitcoin nodes do, they adjust the target so that a new block is added every ten minutes to the chain on average.

But then why would a miner care to adjust the difficulty target? After all it found a valid block. The answer here is simply that if the miner does that then it would not be using Bitcoin anymore and other nodes would not validate that block because they would check the `bits` header field and see that it's too low. And again here, you can't just do the work with a lower difficulty first and then try to change the `bits` after to make it look like you did the actual work with the correct difficulty, because that would update the block hash to an invalid value, forcing you to do the right amount of work you should have done in the first place.

### Economic incentives

Another big part of what makes this whole system work like an well oiled machine is that incentives are aligned in such a way that miners are strongly discouraged from generating invalid blocks. That is because when a node mines, it commits to resources (electricity), and so it is in fact very costly to generate a block that will be rejected by the network. A miner only gets retributed with the [coinbase transaction](https://river.com/learn/terms/c/coinbase/) if it scrupulously followed all of the protocol checks that make a block valid and found the right nonce of course. An evil miner that would tamper with old blocks and then try to outpace the longest chain would not only fail but would also waste tremendous amounts of physical resources doing that.
Thus in game theoretical terms one could say that miners reach a Nash equilibrium by behaving in an honest way, since it is the strategy that optimizes their benefits (you can read this interesting [article](https://saltlending.com/game-theory-and-bitcoin/) which goes into further details on the game theoretical aspects of bitcoin mining).

### Conclusion

In a sense PoW consensus is an emergent phenomenon. Miners and validating nodes are just following a set of rules specified by the Bitcoin protocol and out of that comes out a shared knowledge and understanding that is the current state of the peer-to-peer distributed timestamp server like Satoshi would call it, also known as blockchain. However like is often the case in cyber security some types of attacks on the protocol remain possible, like the famous 51% attack. But that is probably a topic for another post.

I hope I gave you a general picture of how PoW helps the blockchain achieve immutability and how it ties in with the rest of the protocol to make an elegant system function and ensure that no single bad actor can tamper with the history of transactions.
