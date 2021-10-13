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
   <br></br>
  Example: `bos balance --onchain --confirmed`
<br></br>
<br></br>

3. `bos cert-validity-days`: Returns how many days your certificate is valid.
- Flags: 
  - `below`: returns number of days below a certain number
   <br></br>
  Example: `bos cert-validity-days --below 10`
<br></br>
<br></br>

4. `bos chain-deposit`: Generates address and QR code to deposit funds to your onchain wallet.
- Options:
  - `amount`: generate an address to deposit a specific amount. `bos chain-deposit 100000`.
   <br></br>
  Example: `bos chain-deposit` or `bos chain-deposit 100000`
<br></br>
<br></br>

5. `bos chainfees`: Gives you an estimate of the chain-fees for various confirmation targets
- Flags:
  - `blocks`: Fees estimate based on block confirmation target
  - `file`: Enter path to a JSON file to write the out of the command to.
   <br></br>
  Example: `bos chainfees --blocks 10 --file /home/umbrel/blocks.json`
<br></br>
<br></br>

6. `bos chart-chain-fees`: Gives you a chart and total onchain fees you paid in the last 60 days (default and can be changed)
  - Flags:
    - `days`: Produces a chart for the last number of days specified.
     <br></br>
  Example: `bos chart-chain-fees --days 90`
  <br></br>
<br></br>

7. `bos chart-fees-earned`: Gives you a chart and total routing fees you earned in the last 60 days (default and can be changed)
  - Options:
    - `pubkey`: Enter the pubkey of for the peer to get the routing fees earned via a specific peer.
  - Flags:
    - `days`: Produces a chart for the last number of days specified.
    - `count`: Give you a count of the number of forwards instead of sats
     <br></br>
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
     <br></br>
  Example: `bos chart-fees-paid --days 15 --rebalances`
  <br></br>
<br></br>

9. `bos chart-payments-received`: Gives you a chart of all payments received on your node like keysends and settled invoices.
  - Flags:
    - `days`: Produces a chart for the last number of days specified.
     <br></br>
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
     <br></br>
  Example: `bos credentials --cleartext --days 200 --readonly`
    <br></br>
  <br></br>


12. `bos fees`: Gives a chart of fees rates set per peer. Base fees is not included.
  - Flags:
    - `set-fee-rate`: Lets you set fee rate in ppm
    - `to`: Specify the public key of the peer you want to set fee rate to, multiple public keys can be passed.
     <br></br>
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

15. `bos fund`: Lets you make a signed transaction to an address and a specific amount to spend your onchain funds.
  - Arguments:
    - `address`: Enter the address you're funding.
    - `amount`: Enter the amount you're funding.
  - Flags:
    - `dryrun`: Does a dryrun and prevents your funds (UTXOs) from getting locked.
    - `utxo`: Enter a specific tx_id:vout that you want to use to fund.
    - `select-utxos`: Opens an interactive view to select your spendable UTXOs, use "Space" to select a UTXO and hit "Enter" when done.
     <br></br>
  Example: `bos fund addressToFund amountToFund --select-utxos`
  <br></br>
<br></br>

16. `bos gateway`: Stars a LND gateway server on a port that listens to WebUI Access. Connection code has a specific expiry time.
  - Flags:
    - `port`: Starts lisening on a specified port
    - `remote`:  Enter a URL to generate a connection code for a remotegate way that expires at a specific time.
    <br></br>
<br></br>

17. `bos graph`: Returns a list of connections and other public information of a node.
  - Arguments:
    - Takes `pubkey` or `alias` as an option to return output.
  - Flags:
    - `filter`: Set a filter to filter returned results, example `--filter CAPACITY>1000000` returns channels with peers greater than 1M capacity.
    - `sort`: Sorts the rows in the table by the column specified. example `--sort out_fee`
     <br></br>
  Example: `bos graph Bitrefill --sort in_fee` 
<br></br>
<br></br>

18. `bos inbound-channel-rules`: Sets rules for other peers to open channels to you. It takes formulas as the as the rule.
  - Flags:
    - `rule`: Select the rule you want to set, examples are `CAPACITY>5000000` to only allow inbound channels of more than 5M capacity. `CAPACITIES>100*M` to only allow an inbound channel if the peer has a total of 1BTC capacity of all public channels put together. Other examples include `PUBLIC_KEY`, `CHANNEL_AGES`, `FEE_RATES` etc.
    - `reason` sends back a reason message when rejecting an inbound channel.
     <br></br>
  Example: `bos inbound-channel-rules --rule CAPACITY>5000000 --message "Will only accept a minimum 5M inbound channel"`
<br></br>
<br></br>

19. `bos inbound-liquidity`: Returns your total inbound liquidity you currently have
  - Flags:
    - `above` returns tokens above a number you specify
    - `below` returns tokens below a number you specify
    - `min-score` set a minimum fee rate filter
    - `max-fee-rate` set a maximum fee rate filter
    - `top` returns liquidity in the top percentile for an individual channel
<br></br>
<br></br>

20. `bos increase-inbound-liquidity`: Helps increase your inbound liquidity by doing a loop out.
  - Flags:
    - `address`: you can specify an external address to send the looped out onchain funds to
    - `api-key`: specify a prepaid API key to use
    - `avoid`: avoid certain pubkeys or channels IDs while taking the path to LOOP. You can use this with `bos tags` and set an avoid tag
    - `confs`: Number of onchain confirmations to consider you have received the funds successfully, defaulted to 1
    - `dryrun`: Does not actually loop out but can give you an estimation of how much amount can be looped out and how much it would cost in offchain fees
    - `fast`: Request LOOP server to avoid batching your onchain transaction
    - `amount`: amount you want to increase inbound liquidity by
    - `max-fee`: max fees you're willing to pay in total for the swap
    - `recover`: you can use the recovery key provided by bos to recover funds in an inprogress swap
    - `with`: specify the pubkey of the peer you want to increase inbound liquidity for
<br></br>
<br></br>

21. `bos increase-outbound-liquidity`: Opens a new channel to increase your outbound liquidity. If you don't specify `with` flag, BOS chooses a peer for you to open a channel to.
  - Flags:
    - `amount`: amount to increase liquidity
    - `fee-rate`: set channel open fee rate (sats/vByte)
    - `private`: opens a private channel
    - `with`: enter the pubkey to open channel with
    - `dryrun`: avoids opening the channel but gives you a summary of the channel open
<br></br>
<br></br>

22. `bos outbound-liquidity`: Returns your total outbound liquidity you currently have
  - Flags:
    - `above`: returns tokens above a number you specify
    - `below`: returns tokens below a number you specify
    - `min-score`: set a minimum fee rate filter
    - `max-fee-rate`: set a maximum fee rate filter
    - `top`: returns liquidity in the top percentile for an individual channel
    - `with`: specify the pubkey of a peer to return liquidity with that peer.
<br></br>
<br></br>

23. `bos nodes`: Adds a saved node to for you to control remotely
  - Options: 
    - `nodeName`: Enter the name of the node, new or existing
  - Flags:
    - `add`: will add a new saved node, will ask you a series of questions to fill out.
    - `remove`: removes an existing saved node
    - `unlock`: removes encryption on the macaroon of a saved node
    - `lock`: encrypt a saved node using a GPG key
<br></br>
<br></br>

24. `bos open`: Helps to open channels to the network, batch opening and funding from external/cold wallet is supported.
  - Arguments:
    `pubkey`: public key of the node you want to open a channel to. Can enter multiple with a space in between.
  - Flags:
    - `amount`: capacity of the channel in Sats you want to open, can specify a separate amount if batch opening channels, default 5M sats if not specified
    - `external-funding`: give you an address for you to sign from your external wallet along with the amount.
    - `set-fee-rate`: waits until the channel is open and attempts to set a forwarding fee rate, this process needs to run in the background until a channel is open. Have to run in background process manager like tmux, nohup or keep the ssh session open
    - `type`: public/private, defaulted to public
     <br></br>
  Example: `bos open pubkey1 --amount 1000000 pubkey2 --amount 3000000 pubkey3 --amount 4000000`. Once you enter the command and hit enter, it will ask the onchain transaction fee you want to set and also if you want to use your internal LND wallet for funding the transaction.
<br></br>
<br></br>

25. `bos balanced-channel-open`: Lets you open a balanced channel with your peer, both peers involved need to have keysend turned on.
  - Flags:
    - `recover`: Enter the address if funds were accidentally sent to it.
    <br></br>
  Simple running the command `bos balanced-channel-open` will ask you a series of questions to enter, like the `pubkey`, `total capacity` of the channel and the funding `fee rate`. It then key sends all that information to your peer to fund the other half for the channel. Your peer needs to run the same command to accept the request and review all information and agree to it, then the 1st peer or initiator will broadcast the transaction.
  <br></br>
  ![Balanced Channel Open](./images/balancedopen.jpg)







