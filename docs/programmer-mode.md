# Programmer Mode

Programmer Mode lets DiceBot run custom betting logic from Lua scripts.

This document is a local repository-native guide for the scripting mode. It replaces external documentation links with a maintainable in-repo reference.

## What Programmer Mode is for

Use Programmer Mode when the built-in strategy modes are not flexible enough.

Common use cases:

- Custom staking systems
- Custom stop and reset rules
- Hybrid strategies
- Strategy experiments
- Reproducing external strategy examples
- Debugging and simulation before live betting

Programmer Mode should always be tested with the simulator before placing live bets.

## Script lifecycle

A Programmer Mode script is evaluated by the bot while automatic betting runs.

A typical loop is:

1. The bot starts with configured initial values.
2. A bet is placed.
3. The bot updates state such as balance, profit, win/loss result, streaks, and last bet data.
4. The Lua script runs.
5. The script changes variables such as the next bet size or betting direction.
6. The next bet is placed unless the script or bot conditions stop betting.

The script usually controls the next action by assigning values to exposed variables.

## Core variables

The exact exposed variable set can vary by DiceBot version. The commonly used Programmer Mode variables are listed below.

### Betting control

| Variable | Purpose |
| --- | --- |
| `nextbet` | Amount for the next bet. |
| `chance` | Win chance for the next bet. |
| `bethigh` | Betting direction. `true` means bet high, `false` means bet low. |
| `stop()` | Stops the bot when called. |

### Balance and profit

| Variable | Purpose |
| --- | --- |
| `balance` | Current account balance known by the bot. |
| `profit` | Current session profit. |
| `currentprofit` | Profit from the most recent bet. |
| `previousbet` | Amount of the previous bet. |

### Roll and result state

| Variable | Purpose |
| --- | --- |
| `win` | Whether the previous bet won. |
| `lastBet` | Previous bet information, depending on version. |
| `lastbet` | Previous bet amount or previous bet object, depending on version. |
| `lucky` | Last roll / lucky number, where exposed. |
| `nonce` | Current nonce, where exposed by the selected site. |

### Streaks and counters

| Variable | Purpose |
| --- | --- |
| `bets` | Number of bets placed in the current session. |
| `wins` | Number of wins in the current session. |
| `losses` | Number of losses in the current session. |
| `currentstreak` | Current win/loss streak. Usually positive for wins and negative for losses. |
| `beststreak` | Best win streak, where exposed. |
| `worststreak` | Worst loss streak, where exposed. |

## Minimal safe script template

```lua
-- Minimal Programmer Mode template.
-- Test in simulator before live use.

basebet = balance / 10000
nextbet = basebet
chance = 49.5
bethigh = true

function dobet()
    if win then
        nextbet = basebet
    else
        nextbet = previousbet * 2
    end

    if profit >= balance * 0.01 then
        stop()
    end

    if nextbet > balance * 0.05 then
        stop()
    end
end
```

## Martingale example

```lua
-- Simple Martingale example.
-- Resets after a win and doubles after a loss.

basebet = 0.00000001
nextbet = basebet
chance = 49.5
bethigh = false

function dobet()
    if win then
        nextbet = basebet
    else
        nextbet = previousbet * 2
    end
end
```

## Flat betting example

```lua
-- Flat betting example.
-- Keeps the same bet amount regardless of result.

basebet = 0.00000001
nextbet = basebet
chance = 49.5
bethigh = true

function dobet()
    nextbet = basebet
end
```

## Reverse Martingale example

```lua
-- Reverse Martingale example.
-- Increases after wins and resets after losses.

basebet = 0.00000001
nextbet = basebet
chance = 49.5
bethigh = true

function dobet()
    if win then
        nextbet = previousbet * 2
    else
        nextbet = basebet
    end
end
```

## Zig-zag high/low example

```lua
-- Alternates bet direction every bet.

basebet = 0.00000001
nextbet = basebet
chance = 49.5
bethigh = true

function dobet()
    nextbet = basebet
    bethigh = not bethigh
end
```

## Stop-loss and take-profit example

```lua
-- Stops when session profit reaches a target or drawdown reaches a limit.

basebet = 0.00000001
nextbet = basebet
chance = 49.5
bethigh = true

startbalance = balance
takeprofit = startbalance * 0.01
stoploss = startbalance * 0.02

function dobet()
    nextbet = basebet

    if profit >= takeprofit then
        stop()
    end

    if profit <= -stoploss then
        stop()
    end
end
```

## Loss streak guard example

```lua
-- Stops after too many consecutive losses.
-- currentstreak is commonly negative during a loss streak.

basebet = 0.00000001
nextbet = basebet
chance = 49.5
bethigh = true
max_loss_streak = 8

function dobet()
    if win then
        nextbet = basebet
    else
        nextbet = previousbet * 2
    end

    if currentstreak <= -max_loss_streak then
        stop()
    end
end
```

## Bankroll-aware bet cap

```lua
-- Prevents the script from betting more than a fixed percentage of balance.

basebet = 0.00000001
nextbet = basebet
chance = 49.5
bethigh = true
max_bet_fraction = 0.02

function dobet()
    if win then
        nextbet = basebet
    else
        nextbet = previousbet * 2
    end

    maxbet = balance * max_bet_fraction
    if nextbet > maxbet then
        nextbet = basebet
        stop()
    end
end
```

## Common Lua syntax notes

### Comments

```lua
-- Single line comment
```

### Variables

```lua
basebet = 0.00000001
chance = 49.5
bethigh = true
```

### If/else

```lua
if win then
    nextbet = basebet
else
    nextbet = previousbet * 2
end
```

### Functions

```lua
function dobet()
    -- Code executed after each completed bet.
end
```

### Boolean values

```lua
bethigh = true
bethigh = false
```

### Math

```lua
nextbet = previousbet * 2
nextbet = previousbet / 2
nextbet = previousbet + basebet
nextbet = math.max(basebet, previousbet / 2)
```

## Debugging strategy scripts

Use these checks when a script behaves unexpectedly:

1. Run the script in the simulator first.
2. Confirm that `dobet()` exists and is spelled correctly.
3. Confirm that every `if` has a matching `end`.
4. Confirm that numeric values use a dot as decimal separator.
5. Confirm that `nextbet` is always positive.
6. Confirm that `chance` is inside the selected site's allowed range.
7. Confirm that `bethigh` is explicitly `true` or `false`.
8. Add conservative stop rules before live testing.

## Practical safety defaults

Every live script should include at least:

- A fixed base bet.
- A maximum next bet cap.
- A stop-loss.
- A take-profit.
- A loss streak guard.
- Simulation testing before real funds.

## Full defensive template

```lua
-- Defensive Programmer Mode template.
-- Adjust values only after simulation.

startbalance = balance
basebet = startbalance / 100000
nextbet = basebet
chance = 49.5
bethigh = true

maxbet = startbalance * 0.01
stoploss = startbalance * 0.03
takeprofit = startbalance * 0.01
max_loss_streak = 8

function dobet()
    if win then
        nextbet = basebet
    else
        nextbet = previousbet * 2
    end

    if nextbet > maxbet then
        nextbet = basebet
        stop()
    end

    if profit <= -stoploss then
        stop()
    end

    if profit >= takeprofit then
        stop()
    end

    if currentstreak <= -max_loss_streak then
        stop()
    end
end
```

## Important warning

A Lua strategy can lose the full configured balance very quickly if it increases bet size aggressively or lacks stop conditions. Programmer Mode gives flexibility, not a statistical edge.
