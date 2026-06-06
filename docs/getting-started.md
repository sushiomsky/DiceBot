# Getting Started

This guide is the local onboarding documentation for DiceBot. It replaces the old external getting-started page with repo-native Markdown.

## 1. Download or build

DiceBot is a Windows desktop application. Use a trusted build of this repository, or build it yourself from the source tree. Avoid binaries from random mirrors, reposts, private messages, or unofficial archives.

## 2. Extract the application

Release builds are typically distributed as an archive. Extract the archive into its own folder before running the program.

A normal extracted folder should contain the executable, support files, configuration files, and the local database file after first use.

## 3. Run DiceBot

Open the extracted folder and start the DiceBot executable. Depending on Windows settings, the `.exe` extension may or may not be visible.

On first launch, the application may show a help prompt. The documentation in this repository is intended to be the local replacement for that prompt.

## 4. Select a site

Open the site menu and select the dice site you want to use.

The selected site controls which login implementation, API behavior, roll verifier, and supported features are available. Not every site supports every feature.

## 5. Log in

Use the login panel to enter the credentials required by the selected site.

Typical fields are:

- Username or account name
- Password or API key, depending on the site
- Two-factor authentication code when the selected site requires it

Click **Log In** after filling the required fields. The bot may appear frozen for a few seconds while it waits for the remote site to answer. A popup or status message should report success or failure.

If login fails, check the following before changing strategy settings:

- Correct site selected
- Correct username, password, API key, or token
- Correct two-factor authentication code
- No unsupported characters in the password
- The selected site is online and its API has not changed

## 6. Understand views and panels

DiceBot uses multiple panels for account access, manual bets, live bet history, charts, statistics, and strategy settings.

The **View** menu can show or hide panels such as:

- Login panel
- Manual betting panel
- Live chart
- Stats window
- Simulation window
- Roll verifier
- Bet history
- Chat window, where supported

Panel visibility is a convenience setting. If a panel disappears, open the **View** menu and enable it again.

Some panels can be resized by dragging the divider between the chart, bet list, and settings areas.

## 7. Place a single manual bet

Manual betting is useful for confirming that login, balance detection, and site communication work before enabling automatic betting.

Steps:

1. Open the manual betting panel.
2. Enter the bet amount.
3. Enter the chance to win.
4. Click **Bet High** or **Bet Low**.
5. Confirm that the bet appears in the live bet list.

The live bet list usually shows recent bets placed by the bot. Important columns include:

- Bet ID: the site-side identifier for the bet.
- Roll: the lucky number returned by the site.
- Nonce: the sequence value used by provably fair algorithms on sites that expose it.
- Profit: the win or loss for that bet.

## 8. Use the live chart

The live chart shows session profit over time while the bot is running.

You can:

- Reset the visible chart.
- Stop chart drawing.
- Hide the chart panel.
- Re-enable it from the **View** menu.

The chart is visual feedback only. Hiding it does not stop betting.

## 9. Basic Martingale mode

The default basic mode is a simple Martingale-style configuration.

Core fields:

### Starting bet

The base bet. After a normal reset, the bot returns to this amount.

### Chance to win

The win chance controls payout. Higher win chance means lower payout. Lower win chance means higher payout and higher variance.

Approximate payout relation:

```text
payout = (100 - site_edge) / chance_to_win
chance_to_win = (100 - site_edge) / payout
```

### On loss: multiply bet by

After a losing bet, the next bet is multiplied by this value.

Example with multiplier `2` and starting bet `10`:

```text
10 -> 20 -> 40 -> 80
```

### On win: multiply bet by

The same idea as the loss multiplier, but applied after wins. In the default Martingale flow the bot normally resets to base after the first win following a loss streak. Advanced settings can alter that behavior.

## 10. Start and stop automatic betting

Use **Start High** or **Start Low** to begin automatic betting.

- **Start High** starts betting over the target roll.
- **Start Low** starts betting under the target roll.
- **Stop** stops automatic betting.
- **Stop on Win** continues until the next winning bet and then stops.

If bets are not appearing, check the status bar first. Common causes include insufficient balance, rejected bet size, invalid login, changed API behavior, or site-side limits.

## 11. Stats window

The stats window shows session information since the bot was opened.

Typical stats include:

- Current balance
- Profit
- Number of bets
- Wins and losses
- Current streaks
- Largest win and loss streaks
- Profit per hour or day estimates
- Largest bet information
- Bankruptcy or max-loss projections where available

Session stats are not necessarily all-time stats. Restarting the application can reset parts of the displayed session state.

## 12. Run simulations

The simulator lets you test a configured strategy without wagering real funds.

Steps:

1. Open the simulation window.
2. Enter a mock starting balance.
3. Enter the number of simulated bets.
4. Run the simulation.
5. Review ending balance, profit, wins, losses, and streaks.
6. Export the simulation to CSV when deeper analysis is needed.

The simulator is useful for strategy debugging, but simulated success does not imply real-world profitability.

## 13. Verify rolls manually

DiceBot includes roll verification logic for supported sites.

Manual verification workflow:

1. Select the site whose rolls you want to verify.
2. Open the roll verifier.
3. Paste the server seed.
4. Paste the client seed.
5. Enter the starting nonce when the site uses a nonce-based algorithm.
6. Enter the number of bets to generate.
7. Generate the roll list.
8. Compare the generated lucky numbers with the site-side bet records.

For sites that do not use nonce-based algorithms, use the values expected by that site's verifier implementation.

## 14. Verify rolls from bet history

DiceBot stores bets it places in a local SQLite database, usually named `dicebot.db`.

The bet history window can use locally stored data such as:

- Bet ID
- Roll
- Nonce
- Client seed
- Server seed
- Server seed hash
- Verification status

When enough seed data is available, DiceBot can automatically mark matching rolls as verified.

If only a server seed hash is known, the bot may try to fetch the revealed server seed later after the user randomizes seeds on the site. This depends on the selected site's API and may not work on every site.

## 15. Charts

DiceBot can draw several chart types:

- Live session chart
- Static session chart
- All-time chart from stored bet history
- Custom range chart by date or bet ID range

Charts can be saved as images. Chart export is for analysis and sharing; it does not affect betting behavior.

## 16. Chat

Some supported sites allow the bot to connect to chat. Others block bot chat access or do not expose a compatible chat API.

When available, open chat from the **View** menu. You can send messages using the input field and the send button or Enter key.

## 17. Settings

The settings window contains options that are not directly part of a single bet, such as:

- Sound alerts
- Email notifications
- Automatic login behavior
- Number of live bets to display
- Seed collection behavior
- Bot speed delay
- Import and export behavior

Bot speed can slow the bot down. It cannot make the bot faster than the selected site, the network, and the local machine allow.

## 18. Basic, advanced, and programmer modes

DiceBot has multiple settings modes.

### Basic mode

Shows a simplified Martingale configuration. If advanced rules already exist, switching to basic mode changes the display but does not necessarily remove advanced behavior.

### Advanced mode

Adds configurable strategies and conditions, including:

- Martingale customization
- Labouchere
- Fibonacci
- d'Alembert
- Stop conditions
- Reset conditions
- Balance thresholds
- Auto withdraw
- Auto invest
- Server seed randomization
- High/low zig-zag behavior
- Quick strategy switching

### Programmer mode

Allows custom strategy logic through Lua scripting. See `docs/programmer-mode.md` for the local Programmer Mode guide.

## 19. Safety notes

DiceBot automates betting; it does not make gambling profitable by itself.

Important rules:

- Test every strategy with simulation first.
- Start with the smallest possible real bet when testing against a live site.
- Expect APIs and site behavior to change.
- Never gamble funds you cannot afford to lose.
- Stop conditions reduce risk but do not remove risk.
- Martingale-style strategies can fail quickly when balance or site limits are reached.
