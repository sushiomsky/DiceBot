# Roll Verification

DiceBot includes roll verification support for sites that expose enough provably fair data.

This document keeps the verification workflow inside the repository instead of relying on external documentation pages.

## What roll verification proves

Roll verification proves that a recorded roll matches the seeds and algorithm exposed by the selected site.

It can confirm:

- The revealed server seed matches the previous server seed hash.
- The client seed was included in the roll calculation.
- The nonce or bet sequence matches the recorded bet.
- The calculated lucky number matches the roll stored in bet history.

It cannot prove that a future roll will be favorable.

## Required values

A verifier usually needs some combination of:

- Server seed
- Server seed hash
- Client seed
- Nonce
- Bet ID
- Lucky number / roll result
- Site-specific algorithm details

Different sites use different algorithms. Always select the matching site in the verifier.

## Manual verification workflow

1. Open the roll verifier.
2. Select the site.
3. Paste the server seed.
4. Paste the client seed.
5. Enter the nonce or starting nonce when required.
6. Enter how many rolls to generate.
7. Generate rolls.
8. Compare the generated results with the site bet history.

## Automatic history verification

DiceBot stores placed bets in the local database. When enough seed data is available, it can compare stored bets with calculated rolls.

A bet can be marked verified when:

- Required seed values exist.
- The server seed hash matches the revealed server seed.
- The calculated roll equals the recorded roll.

If a server seed has not yet been revealed, verification must wait until the site exposes it.

## Server seed hash

A server seed hash is a commitment. It lets the site publish a hash before the bets and reveal the seed after the bets.

Verification flow:

1. Before betting, the site provides a server seed hash.
2. Bets are placed using the hidden server seed.
3. The site later reveals the server seed.
4. The bot hashes the revealed server seed.
5. The bot compares that hash to the original server seed hash.

If the hash does not match, the seed data is invalid.

## Nonce

The nonce is a counter used by many dice sites to make each roll unique while using the same seed pair.

A typical sequence is:

```text
client seed + server seed + nonce 0
client seed + server seed + nonce 1
client seed + server seed + nonce 2
```

If the nonce is off by one, generated rolls will not match recorded rolls.

## Common verification problems

### Wrong site selected

The same seeds can produce different rolls on different sites because each site can use a different algorithm.

### Wrong nonce

A missing or shifted nonce is one of the most common causes of mismatch.

### Seed randomized after the target bets

Seeds must match the exact period in which the bets were placed.

### Bet history imported from outside DiceBot

Imported or manually entered history may lack seed values needed for automatic verification.

### Site API changed

A site can change its seed format, hash algorithm, or API response. In that case, DiceBot's site-specific verifier may need code updates.

## Practical checklist

Before reporting a verification bug, collect:

- Site name
- DiceBot version or commit
- Bet IDs
- Recorded rolls
- Client seed
- Revealed server seed
- Original server seed hash
- Starting nonce
- Expected and actual verifier output

Never publish account credentials, API keys, withdrawal addresses, or private tokens with a bug report.
