# Balance-of-Satoshis-Docs

**This document helps with BOS Commands:**

Most bos commands follow the following format.
`bos commandName argument --flag` or `bos commandName --flag` if a command does not have an argument. 
**Arguments are mandatory, flags are optional.** <br> </br>



1. `bos` or `bos --help` or `bos -h`: This brings up the help menu with the list of all BOS Commands
2. `bos --version` or `bos -V`: Shows the current version of BOS
3. `bos help CommandName` or `bos CommandName --help` or `bos CommandName -h`: Displays help section and usage of each command. Example usage `bos help peers`
4. All bos commands can use a node flag that can be used to call any saved node you might have saved in the `~/.bos` directory. If you have multiple nodes you can remotely control your node from bos. Example: `bos peers --node=alice` where alice is the name of your saved node.

<br></br>
## Commands List:

1. `bos accounting`: There are 6 different categories for accounting:
- There are 6 different aruguments for accouting. `chain-fees, chain-receives, chain-sends, forwards, invoices, payments`.
  - chain-fees: All on-chain fees paid, example channel opens and closures.
  - chain-receives: All onchain payments you receive including any from channel closures.
  - chain-sends: Any onchain payments you made.
  - forwards: Fees you earned from routing payments
  - invoices: any invoices settled
  - payments: any payments made on the LN including keysends.
- Usage example: `bos accounting chain-fees`. Displays amounts spent on chain-fees in a table.
- Flags available: 
  - `--csv`: outputs the accounting results to a csv file: Example: `bos accounting chain-fees --csv > chainfees.csv`
  - `disable-fiat`: Disables the usage of fiat in accounting, it defaults to sats as the unit or account.
  - `month`: select the month number to get accounting only for that specific month. `bos accounting forwards --month 8` returns results for August.
  - `rate-provider`: BOS provides two rate providers, coindesk and coingecko to provide accounting in fiat, this flag is defaulted to coindesk. To switch provider if the other provider is down or results take too long to pop-up use `bos accounting forwards --rate-provider coingecko`
  - `year`: returns accounting results for a specifc year, it can be used in combination with month or separately to reults for the entire year. `bos accounting payments --month 10 --year 2021`
- Flags can be used together, example: `bos accounting forwards --month 10 --disable-fiat`
<br></br>

2. `bos balance`: Gives total of on-chain, off-chain, pending and commit fees.
- Flags:
  - `above`: Returns balance above a certain number, `bos balance --above 10000`
  - `below`: Returns balance below a certain number, `bos balance --below 10000`
  - `confirmed`: Returns confirmed balance only. Removes any pending funds.
  - `detailed`: Returns detailed balance, a break down of offchain and onchain
  - `offchain`: Returns offchain balance only, amount on channels.
  - `onchain`: Returns onchain balance only, amount in your LND onchain wallet.
- Example usage: `bos balance --onchain --confirmed`
<br></br>

3. `bos cert-validity-days`: Returns how many days your certificate is valid.
- Flags: 
  - `below`: returns number of days below a certain number
<br></br>

4. 



