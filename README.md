DiceBot
=======

DiceBot is a desktop betting automation tool for dice-style gambling sites. It can place manual and automated bets, simulate strategies, track betting history, verify rolls, and run custom Lua strategies through Programmer Mode.

## Documentation

The old external getting-started and forum links have been replaced with local repository documentation:

- [Getting Started](docs/getting-started.md)
- [Feature Overview](docs/features.md)
- [Programmer Mode](docs/programmer-mode.md)
- [Roll Verification](docs/roll-verification.md)

## Supported site context

Historically, DiceBot included integrations for:

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

Some historical integrations were later marked inactive or unsupported, including MagicalDice, DaDice, and Coinichiwa.

Site support depends on the current code and on whether the remote site still exposes a compatible API.

## Main features

- Customizable Martingale strategy
- Customizable Labouchere strategy
- Customizable Fibonacci strategy
- Customizable d'Alembert strategy
- Preset bet lists
- Programmer Mode with Lua scripting
- Stop and reset conditions
- Zig-zag betting between high and low
- Auto invest and auto withdraw conditions where supported
- Live and historical charts
- Searchable local bet history
- Automatic roll verifier for supported sites
- Strategy simulator
- Independent roll verifier
- Sound and email notifications
- Automatic login and automatic start options
- Built-in chat for selected supported sites
- Import and export of settings
- Emergency stop key combination
- Session statistics

## Risk warning

Gambling remains gambling regardless of automation or strategy complexity. The house edge still applies. DiceBot can automate actions and test strategies, but it cannot guarantee profit.

Use the bot at your own risk. Test with simulation first, start with the smallest possible real bet, and never risk funds you cannot afford to lose.

## Reporting issues

When reporting a bug, include as much detail as possible:

- DiceBot version or commit
- Selected site
- Strategy mode
- Exact steps to reproduce
- Expected behavior
- Actual behavior
- Relevant logs or screenshots
- Whether the issue happens in simulation or only on a live site

Do not include passwords, API keys, private tokens, withdrawal addresses, or other sensitive account data.
