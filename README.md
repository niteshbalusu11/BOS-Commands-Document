# Balance of Satoshis Commands

**This document helps with BOS Commands:**

Most bos commands follow the following format.
`bos CommandName Argument --Flag` or `bos CommandName --Flag` or `bos CommandName Option --Flag`if a command does not have an argument. 
**Arguments are always mandatory, options and flags are optional.**
<br> </br>



1. `bos` or `bos --help` or `bos -h`: This brings up the help menu with the list of all BOS Commands
2. `bos --version` or `bos -V`: Shows the current version of BOS
3. `bos help CommandName` or `bos CommandName --help` or `bos CommandName -h`: Displays help section and usage of each command. Example usage `bos help peers`
4. All bos commands can use a node flag that can be used to call any saved node you might have saved in the `~/.bos` directory. If you have multiple nodes you can remotely control your node from bos. Example: `bos peers --node=alice` where alice is the name of your saved node.

<br></br>
## Commands List:

1. `bos accounting`: There are 6 different categories for accounting:
- Arguments:
  - `chain-fees`: All on-chain fees paid, example channel opens and closures.
  - `chain-receives`: All onchain payments you receive including any from channel closures.
  - `chain-sends`: Any onchain payments you made.
  - `forwards`: Fees you earned from routing payments
  - `invoices`: any invoices settled
  - `payments`: any payments made on the LN including keysends.
- Usage example: `bos accounting chain-fees`. Displays amounts spent on chain-fees in a table.
- Flags: 
  - `--csv`: outputs the accounting results to a csv file: Example: `bos accounting chain-fees --csv > chainfees.csv`
  - `disable-fiat`: Disables the usage of fiat in accounting, it defaults to sats as the unit or account.
  - `month`: select the month number to get accounting only for that specific month. `bos accounting forwards --month 8` returns results for August.
  - `rate-provider`: BOS provides two rate providers, coindesk and coingecko to provide accounting in fiat, this flag is defaulted to coindesk. To switch provider if the other provider is down or results take too long to pop-up use `bos accounting forwards --rate-provider coingecko`
  - `year`: returns accounting results for a specifc year, it can be used in combination with month or separately to reults for the entire year. `bos accounting payments --month 10 --year 2021`
- Flags can be used together, example: `bos accounting forwards --month 10 --disable-fiat`
<br></br>
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
<br></br>

3. `bos cert-validity-days`: Returns how many days your certificate is valid.
- Flags: 
  - `below`: returns number of days below a certain number
- Example: `bos cert-validity-days --below 10`
<br></br>
<br></br>

4. `bos chain-deposit`: Generates address and QR code to deposit funds to your onchain wallet.
- Options:
  - `amount`: generate an address to deposit a specific amount. `bos chain-deposit 100000`.
- Example: `bos chain-deposit` or `bos chain-deposit 100000`
<br></br>
<br></br>

5. `bos chainfees`: Gives you an estimate of the chain-fees for various confirmation targets
- Flags:
  - `blocks`: Fees estimate based on block confirmation target
  - `file`: Enter path to a JSON file to write the out of the command to.
- Example: `bos chainfees --blocks 10 --file /home/umbrel/blocks.json`
<br></br>
<br></br>

6. `bos chart-chain-fees`: Gives you a chart and total onchain fees you paid in the last 60 days (default and can be changed)
  - Flags:
    - `days`: Produces a chart for the last number of days specified.
  Example: `bos chart-chain-fees --days 90`
  <br></br>
<br></br>

7. `bos chart-fees-earned`: Gives you a chart and total routing fees you earned in the last 60 days (default and can be changed)
  - Options:
    -`pubkey`: Enter the pubkey of for the peer to get the routing fees earned via a specific peer.
  - Flags:
    - `days`: Produces a chart for the last number of days specified.
    - `count`: Give you a count of the number of forwards instead of sats
  Example: `bos chart-chain-earned --days 90`
  <br></br>
<br></br>

8. `bos chart-fees-paid`: Gives you a chart and total routing fees you paid in the last 60 days (default and can be changed)
  - Flags:
    - `days`: Produces a chart for the last number of days specified.
    - `most-fees`: Gives a table for fees paid per peer/network and amount forwarded per peer.
    - `network`: Fees paid to the network who are not your peers, example are other hops in a rebalance or payment made.
    - `peer`: Fees paid only to your peers excluding the others in the network
    - `rebalances`: shows only fees paid for rebalances or payments made to yourself
  Example: `bos chart-fees-paid --days 15 --rebalances`
  <br></br>
<br></br>

9. `bos chart-payments-received`: Gives you a chart of all payments received on your node like keysends and settled invoices.
  - Flags:
    - `days`: Produces a chart for the last number of days specified.
  Example: `bos chart-payments-received --days 15 --rebalances`
  <br></br>
<br></br>

10. `bos closed`: Returns a list of confirmed channel closures.
  - Flags:
    - `limit`: Limits the number of records returned.  
  Example: `bos closed --limit 20`
<br></br>
<br></br>

11. `bos credentials`: Outputs credentials to access your node. Needs to be used in combination with `bos nodes --add`. Running the command without any flag will ask you a question to enter a pubkey to transfer the credentials in an encrypted way.
  - Flags:
    - `cleartext`: Outputs a cleartext format of macaroon, cert and socket and the credentials expire with default number of days=365
    - `days`: Sets the number of days the credentials produced expire in
    - `readonly`: Outputs credentials that can only be used for read only
    - `nospend`: Outputs credentials that do not let you spend funds on the node
  Example: `bos credentials --cleartext --days 200 --readonly`
    <br></br>
  <br></br>


12. `bos fees`: Gives a chart of fees rates set per peer. Base fees is not included.
  - Flags:
    - `set-fee-rate`: Lets you set fee rate in ppm
    - `to`: Specify the public key of the peer you want to set fee rate to, multiple public keys can be passed.
  Example: `bos fees --set-fee-rate 1000 --to pubkey1 --to pubkey2`
  <br></br>
<br></br>

13. `bos find`: Lets you find something that is stored in the data base, like a transaction, payment information, peer info, channel ID etc.
  - Arguments:
      - Takes differnt kinds arguments:
    Example: `bos find 703539x1305x0` `bos find Bitrefill` `bos find 02816caed43171d3c9854e3b0ab2cf0c42be086ff1bd4005acc2a5f7db70d83774`
  <br></br>
<br></br>

14. `bos forwards`: Outputs a chart of forwards that took place from both inbound and outbound.
  - Flags:
    - `days`: Table view only shows forwards per peer for the last number of days selected
    - `complete`: Shows complete results in a non table format
<br></br>
<br></br>



