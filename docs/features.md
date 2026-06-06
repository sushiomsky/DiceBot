# Feature Overview

This document keeps the high-level project and feature information inside the repository, replacing the old external forum and website pointers.

## Project purpose

DiceBot is a desktop betting automation tool for dice-style gambling sites. It can place manual and automated bets, simulate strategies, track history, verify rolls, and expose multiple strategy modes.

The bot is intended for automation, testing, and analysis of dice betting strategies. It does not guarantee profit.

## Supported site concept

DiceBot contains site-specific implementations. A selected site controls:

- Login flow
- API endpoints
- Balance parsing
- Bet placement
- Bet history import
- Chat support
- Roll verification algorithm
- Available account actions such as withdraw or invest

A site may be shown in the UI but still fail if the site's API changed, the site closed, or login requirements changed.

## Current and historical site support

The historical README listed support for many dice sites, including:

- Just-Dice
- PrimeDice
- Bitdice
- 999dice
- Safedice
- Betking
- Rollin
- BetterBets
- MoneroDice
- FortuneJack
- Crypto-Games
- Moneypot
- CoinMillions
- Bitsler
- WealthyDice

It also marked some historical integrations as inactive or unsupported, including:

- MagicalDice
- DaDice
- Coinichiwa

These names are kept for project context only. Support depends on the current code and on whether each remote service still exists and exposes a compatible API.

## Strategy modes

### Basic mode

Basic mode exposes a smaller set of controls for common Martingale-style betting.

Typical controls:

- Starting bet
- Chance to win
- Bet high or bet low
- Multiplier after loss
- Multiplier after win
- Start, stop, and stop-on-win actions

### Advanced mode

Advanced mode exposes configurable strategies and conditions.

Historically documented advanced features include:

- Custom Martingale behavior
- Labouchere
- Fibonacci
- d'Alembert
- Preset bet lists
- Stop conditions
- Reset conditions
- Balance-based rules
- Streak-based rules
- Profit and loss thresholds
- High/low switching
- Auto invest rules
- Auto withdraw rules
- Seed randomization rules
- Import and export of settings

### Programmer mode

Programmer mode allows the user to write custom strategy logic in Lua. The Lua script can read bot state, alter the next bet, switch betting direction, and stop the bot. See `docs/programmer-mode.md`.

## Bet placement features

DiceBot can place bets automatically using the configured strategy. Depending on the site and strategy mode, it can also:

- Alternate between high and low bets.
- Reset bet size after defined events.
- Increase or decrease bet size after wins or losses.
- Stop after a target profit or loss.
- Stop when a streak condition is reached.
- Stop when balance boundaries are reached.
- Randomize seeds where supported.

## Simulation

The simulator allows strategies to be tested without real funds.

Simulation can be used to inspect:

- Ending balance
- Profit or loss
- Number of wins and losses
- Largest win streak
- Largest loss streak
- Largest bet
- Bankruptcy likelihood under the simulated setup

Simulation is useful for finding obvious strategy bugs. It cannot prove that a strategy will win against a live house edge.

## Bet history

DiceBot stores bet history locally for bets placed by the bot.

The local history can be searched and filtered. Stored bet data may include:

- Site
- Account name
- Bet ID
- Amount
- Profit
- Roll
- Chance
- Payout
- High/low direction
- Nonce
- Client seed
- Server seed
- Server seed hash
- Verification status

The exact fields depend on the site implementation and stored database schema.

## Roll verification

DiceBot includes verifier logic for supported sites.

Verification compares locally calculated rolls with site-provided bet results. Depending on the site, verification may use:

- Server seed
- Server seed hash
- Client seed
- Nonce
- Bet ID
- Site-specific hash algorithm

If all required values are available and the calculated roll matches the recorded roll, the bet can be marked as verified.

## Charts and statistics

DiceBot can display live and historical information, including:

- Session profit chart
- Static chart for the current session
- All-time history chart
- Custom date or bet range chart
- Balance and profit stats
- Win and loss counts
- Streak stats
- Best and worst rolls
- Largest win and loss
- Largest bet

## Notifications

The bot has historically supported alerts such as:

- Sound notifications
- Email notifications
- Status messages
- Emergency stop key combination

The emergency stop key combination was historically documented as `Ctrl + Shift + S`.

## Account automation

Depending on site support, advanced account automation may include:

- Automatic login
- Automatic start after login
- Automatic withdraw rules
- Automatic invest rules

Account automation should be configured conservatively. Always test with minimal balances and confirm that site-specific rules still work.

## Import and export

DiceBot can import and export settings so strategies can be backed up, shared, or moved between installations.

When sharing exported settings, remove personal data first. Strategy files may contain sensitive values depending on the selected site and configuration.

## Chat

Some supported sites expose chat through an API compatible with DiceBot. When supported, DiceBot can show and send chat messages from inside the application.

Chat support is site-specific and may break when a site changes its web or API implementation.

## Risk warning

Automating a betting strategy does not remove the house edge. Strategies such as Martingale can look safe in short runs but fail when a long losing streak, balance limit, or site limit is reached.

Use DiceBot at your own risk.
