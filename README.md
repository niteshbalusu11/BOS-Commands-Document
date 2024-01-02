# Balance of Satoshis Commands

**This document helps with BOS Commands:**

### **Updated until version `17.5.4`**
<br></br>

Most bos commands follow the following format.
`bos CommandName Argument --Flag` or `bos CommandName --Flag` or `bos CommandName Option --Flag`if a command does not have an argument.
**Arguments are always mandatory, options and flags are optional.**
<br> </br>

1. `bos` or `bos --help` or `bos -h`: This brings up the help menu with the list of all BOS Commands
2. `bos --version` or `bos -V`: Shows the current version of BOS
3. `bos help CommandName` or `bos CommandName --help` or `bos CommandName -h`: Displays help section and usage of each command. Example usage `bos help peers`
4. All bos commands can use a node flag that can be used to call any saved node you might have saved in the `~/.bos` directory. If you have multiple nodes you can remotely control your node from bos. Example: `bos peers --node=alice` where alice is the name of your saved node.
<br></br>
### **Always double check with `bos commandName -h` before running a command**
<br></br>

If this guide was of help and you want to share some ❤️, please feel free to send a ⚡ tip to the ⚡ address: 0x49f4f513b6752c95@ln.tips

**LNURL:** LNURL1DP68GURN8GHJ7MRW9E6XJURN9UH8WETVDSKKKMN0WAHZ7MRWW4EXCUP0X9UR2VRYX4NRWCFNVVMKYCMXXCURQGXD649

![LNURL](./images/lnurl.jpg)
<br></br>
## Commands List

- [accounting](#accounting) - Do accounting on your node
- [advertise](#advertise) - Send keysend advertisements to the network
- [balance](#balance) - Shows offchain and onchain balances
- [cert-validity-days](#cert-validity-days) - Shows your certificate validity
- [chain-deposit](#chain-deposit) - Deposit funds on your on-chain wallet
- [chainfees](#chainfees) - Shows current on-chain fees
- [chart-chain-fees](#chart-chain-fees) - On-chain fees you paid
- [chart-fees-earned](#chart-fees-earned) - Routing fees you earned
- [chart-fees-paid](#chart-fees-paid) - Routing fees you paid
- [chart-payments-received](#chart-payments-received) - Payments you received
- [clean-failed-payments](#clean-failed-payments) - Clean failed payments like probes (works with lnd 0.14.0+ only)
- [closed](#closed) - Lists your closed channels
- [create-channel-group](#create-channel-group) - Coordinate balanced channels group
- [credentials](#credentials) - Generates credentials for your node
- [fees](#fees) - Set fees to your channels
- [find](#find) - Query a string
- [forwards](#forwards) - List your forwards and fees earned
- [fund](#fund) - Fund an onchain address
- [graph](#graph) - Get node information from a graph
- [inbound-channel-rules](#inbound-channel-rules) - Set rules for nodes to open channels to you
- [inbound-liquidity](#inbound-liquidity) - Shows your inbound liquidity
- [increase-inbound-liquidity](#increase-inbound-liquidity) - Increase inbound liquidity by looping out
- [increase-outbound-liquidity](#increase-outbound-liquidity) - Increase outbound liquidity by opening channels
- [invoice](#invoice) - Create an invoice and get a BOLT 11 payment request
- [limit-forwarding](#limit-forwarding) - Add restrictions to forwardind through your node
- [lnurl](#lnurl) - Lets you perform a list of LNUrl functions
- [nodes](#nodes) - Configure a saved node
- [open](#open) - Open channels to nodes
- [open-balanced-channel](#open-balanced-channel) - Open a balanced channel with a node
- [open-group-channel](#open-group-channel) - Open balanced channels with multiple peers
- [outbound-liquidity](#outbound-liquidity) - Shows your outbound liquidity
- [pay](#pay) - Pay a payment request
- [peers](#peers) - Displays your peers
- [price](#price) - Current BTC Price
- [probe](#probe) - Probes a node with junk payments
- [rebalance](#rebalance) - Rebalance your channels
- [reconnect](#reconnect) - Attempt to reconnect to disconnected peers
- [remove-peer](#remove-peer) - Close a channel with a peer
- [send](#send) - Keysend payment to a node
- [swap](#swap) - Do a submarine swap with a node
- [tags](#tags) - Create tags for categorizing nodes
- [telegram](#telegram) - Connect bos to a telegram bot to receive updates from your node
- [trade-secret](#trade-secret) - Trade between peers by encoding and decoding a secret
- [utxos](#utxos) - Displays your UTXOs

<br></br>

## Secret Commands List

- [delete-payments-history](#delete-payments-history) - Delete all your payment history
- [gift](#gift) - Gift a peer routing fees
- [encrypt](#encrypt) - Encrypt data with a public key
- [decrypt](#decrypt) - Decrypt data with a public key
- [recover-p2pk](#recover-p2pk) - Sweep onchain funds sent to a public key

<br></br>

## Commands:

### accounting

There are 6 different categories for accounting:

- Arguments:
  - `chain-fees`: All on-chain fees paid, example channel opens and closures.
  - `chain-receives`: All onchain payments you receive including any from channel closures.
  - `chain-sends`: Any onchain payments you made.
  - `forwards`: Fees you earned from routing payments
  - `invoices`: any invoices settled
  - `payments`: any payments made on the LN including keysends.
- Usage example: `bos accounting chain-fees`. Displays amounts spent on chain-fees in a table.
- Flags:
  - `csv`: outputs the accounting results to a csv file: Example: `bos accounting chain-fees --csv > chainfees.csv`
  - `date`: Pick a date with in the month.
  - `disable-fiat`: Disables the usage of fiat in accounting, it defaults to sats as the unit of account.
  - `month`: select the month number to get accounting only for that specific month. `bos accounting forwards --month 8` returns results for August.
  - `rate-provider`: BOS provides two rate providers, coindesk and coingecko to provide accounting in fiat, this flag is defaulted to coindesk. To switch provider if the default provider is down or results take too long to pop-up use `bos accounting forwards --rate-provider coingecko`
  - `year`: returns accounting results for a specifc year, it can be used in combination with month or separately to display results for the entire year. `bos accounting payments --month 10 --year 2021`
- Flags can be used together, example: `bos accounting forwards --month 10 --day 15 --disable-fiat`
  <br></br>
  <br></br>

### advertise

Send keysend advertisements to the network.

- Flags:
  - `budget`: Max budget amount you want to spend for advertising
  - `dryrun`: Avoid sending advertisements and only calculate estimate
  - `filter`: Pass an expression for nodes matching a certain condition
  - `max-hops`: Maximum hops you want to target the advertisement. max-hops=0 is your peers only.
  - `min-hops`: Minimum hops you want to target the advertisement. 
  - `tag`: Advertise to a tag, check `tags` command to learn how to setup tags.

  Example: `bos advertise --budget 3000 --max-hops 2`
  <br></br>
  <br></br>

### balance

Gives total balance of on-chain, off-chain, pending and commit fees.

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

### cert-validity-days

Returns how many days your certificate is valid.

- Flags:
  - `below`: returns number of days below a certain number
    <br></br>
    Example: `bos cert-validity-days --below 10`
    <br></br>
    <br></br>

### chain-deposit

Generates address and QR code to deposit funds to your onchain wallet.

- Options:
  - `amount`: generate an address to deposit a specific amount. `bos chain-deposit 100000`.
- Flags:
  - `format`: set the address format, supported options are np2wpkh, p2wpkh, p2tr (default).
  - `fresh`: generates a fresh address every time.

    <br></br>
    Example: `bos chain-deposit` or `bos chain-deposit 100000 --format p2tr`
    <br></br>
    <br></br>

### chainfees

Gives you an estimate of the chain-fees for various confirmation targets

- Flags:
  - `blocks`: Fees estimate based on block confirmation target
  - `file`: Enter path to a JSON file to write the output of the command to.
    <br></br>
    Example: `bos chainfees --blocks 10 --file /home/umbrel/blocks.json`
    <br></br>
    <br></br>

### chart-chain-fees

Gives you a chart and total onchain fees you paid in the last 60 days (default and can be changed)

- Flags: 
  - `days`: Produces a chart for the last N number of days specified.
  - `end`: End date for the chart.
  - `start`: Start date for the chart.
  <br></br>
  Example: `bos chart-chain-fees --days 90`
  Example: `bos chart-chain-fees --start 2022-06-01 --end 2022-06-30`
  <br></br>
  <br></br>

### chart-fees-earned

Gives you a chart and total routing fees you earned in the last 60 days (default and can be changed)

- Options:
  - `pubkey`: Enter the pubkey of for the peer to get the routing fees earned via a specific peer.
  - `tag`: Enter a `bos tag` that returns results earned via peers in the tag
- Flags: 
  - `count`: Give you a count of the number of forwards instead of sats
  - `days`: Produces a chart for the last N number of days specified. 
  - `end`: End date for the chart.
  - `start`: Start date for the chart.
  <br></br>
  Example: `bos chart-chain-earned --days 90`
  <br></br>
  <br></br>

### chart-fees-paid

Gives you a chart and total routing fees you paid in the last 60 days (default and can be changed)

- Flags: 
  - `days`: Produces a chart for the last N number of days specified. 
  - `end`: End date for the chart
  - `in`: Takes a public key/alias and charts fees paid coming into that node.
  - `most-fees`: Gives a table for fees paid per peer/network. 
  - `most-forwarded`: Gives a table for amount forwarded per peer.  
  - `network`: Fees paid to the network who are not your peers, example are other hops in a rebalance or a payment you made. 
  - `node`: Gives a table of the fees for the node specified.
  - `out`: Takes a public key/alias and charts fees paid out through a node.
  - `peers`: Fees paid only to your peers excluding the others in the network 
  - `rebalances`: shows only fees paid for rebalances or payments made to yourself
  - `start`: Start date for the chart
  <br></br>
  Example: `bos chart-fees-paid --days 15 --rebalances`
  <br></br>
  Example: `bos chart-fees-paid --most-forwarded --days 30`
  <br></br>

### chart-payments-received

Gives you a chart of all payments received on your node like keysends and settled invoices.

- Flags: 
  - `count`: Show count of settled instead of amount received
  - `days`: Produces a chart for the last N number of days specified
  - `end`: End date for the chart.
  - `for`:  Only consider payments including a specific query
  - `start`: Start date for the chart.
  <br></br>
  Example: `bos chart-payments-received --days 15`
  Example: `bos chart-payments-received --start 2022-06-01 --end 2022-06-30`
  <br></br>
  <br></br>

### clean-failed-payments

Cleans all failed payments on your node like probes, helps reduce your channel.db size. Works with lnd 0.14.0-beta and above only. <br></br>
bos also deletes failed payments on the fly if you're on lnd 0.14.0-beta or above.

- Flags: 
  - `dryrun`: Calculates and gives you an output of the number of failed payments. (dryrun flag works with versions of lnd below 0.14.0-beta)
  <br></br>
  Example: `bos clean-failed-payments`
  <br></br>
  <br></br>

### closed

Returns a list of confirmed channel closures.

- Flags: 
  - `limit`: Limits the number of records returned.
  <br></br>
  Example: `bos closed --limit 20`
  <br></br>
  <br></br>


### create-channel-group
Coordinate balanced channels group.

- Flags:
  `allow`: Only allow a list of certain public keys to join a group and also specify the order of the group.
  `capacity`: Specify the total channel capacity of all the channels in the group.
  `fee-rate`: Specify the fee rate of the group open.
  `size`: Specify the size of the group.
  <br></br>
  Example: `bos create-channel-group --allow pubkey1 --allow pubkey2 --size 3 --capacity 10000000`
  <br></br>
  <br></br>

### credentials

Outputs credentials to access your node. Needs to be used in combination with `bos nodes --add`. Running the command without any flag will ask you a question to enter a pubkey to transfer the credentials in an encrypted way.

- Flags:
  - `cleartext`: Outputs a cleartext format of macaroon, cert and socket and the credentials expire with default number of 365 days
  - `days`: Sets the number of days the credentials produced expire in
  - `readonly`: Outputs credentials that can only be used for read only
  - `nospend`: Outputs credentials that do not let you spend funds on the node
    <br></br>
    Example: `bos credentials --cleartext --days 200 --readonly`
    <br></br>
    <br></br>

### fees

Gives a chart of fees rates set per peer. Base fees is not included.

- Flags: 
  - `set-fee-rate`: Lets you set fee rate in ppm, you can use this set fee rate to a channel that is pending open, this requires the SSH session to be open while it attempts to set fees until the channel opens 
  - `to`: Specify the public key of the peer you want to set fee rate to, multiple public keys can be passed.
  <br></br>
  Example: `bos fees --set-fee-rate 1000 --to pubkey1 --to pubkey2`
  <br></br>
  <br></br>

### find

Lets you find something that is stored in the data base, like a transaction, payment information, peer info, channel information etc.

- Arguments: 
  - Takes differnt kinds arguments:
  <br></br>
  Example: `bos find 703539x1305x0` OR `bos find Bitrefill` OR `bos find 02816caed43171d3c9854e3b0ab2cf0c42be086ff1bd4005acc2a5f7db70d83774`
  Find now also returns the size a channel is taking up on the db when you search with Alias or pubkey
  <br></br>
  <br></br>

### forwards

Outputs a chart of forwards that took place from both inbound and outbound peers.

- Flags: 
  - `days`: Table view only shows forwards per peer for the last N number of days selected 
  - `complete`: Shows complete results in a non table format
  - `sort`: Allows you to sort the table output by earnings or liquidity
  <br></br>
  Example: `bos forwards --days 15 --sort="earned_out"`
  <br></br>
  <br></br>

### fund

Lets you make a signed transaction to an address and a specific amount to spend your onchain funds.

- Arguments:
  - `address`: Enter the address you're funding.
  - `amount`: Enter the amount you're funding.
- Flags: 
  - `dryrun`: Does a dryrun and prevents your funds (UTXOs) from getting locked. 
  - `utxo`: Enter a specific tx_id:vout that you want to use to fund. Unconfirmed UTXOs are allowed.
  - `select-utxos`: Opens an interactive view to select your spendable UTXOs, use "Space" to select a UTXO and hit "Enter" when done. 
  - `fee-rate`: Set onchain fee rate for the funding transaction in sats/vByte.
  - `broadcast`: Broadcasts the transaction to the network.
  <br></br>
  Example: `bos fund addressToFund amountToFund --select-utxos --fee-rate 1`
  <br></br>
  <br></br>

### graph

Returns a list of connections and other public information of a node.

- Arguments:
  - Takes `pubkey` or `alias` as an option to return output.
- Flags: 
  - `filter`: Set a filter to filter returned results, example `--filter CAPACITY>1000000` returns channels with peers greater than 1M capacity. 
  - `sort`: Sorts the rows in the table by the column specified. example `--sort out_fee`
  <br></br>
  Example: `bos graph Bitrefill --sort in_fee`
  <br></br>
  <br></br>

### inbound-channel-rules

Sets rules for other peers to open channels to you. It takes formulas as the as the rule.

- Flags: 
  - `rule`: Select the rule you want to set, examples are `CAPACITY>5000000` to only allow inbound channels of more than 5M capacity. `CAPACITIES>100*M` to only allow an inbound channel if the peer has a total of 1BTC capacity from all public channels put together. Other examples include `CHANNEL_AGES`, `FEE_RATES`, `LOCAL_BALANCE`, `PUBLIC_KEY`, `PRIVATE`, `TOR`, `CLEARNET`, `OBSOLETE`, `JOINT_PUBLIC_CAPACITY` etc.
  - `TOR` and `CLEARNET` let you control if you want to accept inbound channels from TOR or CLEARNET peers.
  - `OBSELETE` lets you control if you want to accept inbound channels from peers that are opening a channels to you with a legacy channel type.
  - `JOIN_PUBLIC_CAPACITY` is the sum of capacities of all public channels between you and the requesting peer.
  - `reason` sends back a reason message when rejecting an inbound channel.
  - `coop-close-address`: Listens to inbound channel open requests and intercepts them to add a cooperative closing address to send funds to when the channel to closed. Can be repeatable and it will cycle through the addresses.
  <br></br>
  Example: `bos inbound-channel-rules --rule CAPACITY>=5000000 --reason "Will only accept a minimum 5M inbound channel"`
  <br></br>
  <br></br>

### inbound-liquidity

Returns your total inbound liquidity you currently have

- Flags: 
  - `above` returns tokens above a number you specify 
  - `below` returns tokens below a number you specify 
  - `min-score` set a minimum fee rate filter 
  - `max-fee-rate` set a maximum fee rate filter 
  - `top` returns liquidity in the top percentile for an individual channel 
  - `with` use a `bos tag` to view inbound liquidity with peers in a tag
  <br></br>
  Example: `bos inbound-liquidity` or `bos inbound-liquidity --max-fee-rate 200`
  <br></br>
  <br></br>

### increase-inbound-liquidity

Helps increase your inbound liquidity by doing a loop out.

- Flags: 
  - `address`: you can specify an external address to send the looped out onchain funds to 
  - `api-key`: specify a prepaid API key to use 
  - `avoid`: avoid certain pubkeys or channels IDs while taking the path to LOOP. You can use this with `bos tags` and set an avoid tag 
  - `confs`: Number of onchain confirmations to consider you have received the funds successfully, defaulted to 1 
  - `dryrun`: Does not actually loop out but can give you an estimation of how much amount can be looped out and how much it would cost in routing fees 
  - `fast`: Request LOOP server to avoid batching your onchain transaction 
  - `amount`: amount you want to increase inbound liquidity by 
  - `max-fee`: max fees you're willing to pay in total for the swap 
  - `recover`: you can use the recovery key provided by bos to recover funds in an in-progress swap 
  - `set-fee-rate`: Set a fee rate to the channel once the channel opens
  - `with`: specify the pubkey of the peer you want to increase inbound liquidity for
  <br></br>
  Example: `bos increase-inbound-liquidity --with yourPeerPubkey --max-fee 2000 --dryrun`
  <br></br>
  <br></br>

### increase-outbound-liquidity

Opens a new channel to increase your outbound liquidity. If you don't specify `with` flag, BOS chooses a peer for you to open a channel to.

- Flags: 
  - `amount`: amount to increase liquidity 
  - `fee-rate`: set channel open fee rate (sats/vByte) 
  - `private`: opens a private channel 
  - `with`: enter the pubkey to open channel with 
  - `dryrun`: avoids opening the channel but gives you a summary of the channel open
  <br></br>
  Example: `bos increase-outbound-liquidity --with yourPeerPubkey --fee-rate 1 --dryrun`
  <br></br>
  <br></br>


### invoice
Create an invoice and get a BOLT 11 payment request 

- Arguments:
  - `amount`: Amount in sats/fiat (USD/EUR)

- Flags:
  `for`: Add a description for the invoice.
  `hours`: Number of hours the invoice expires.
  `include-hints`: Include private channel hints in the invoice.
  `rate-provider`: Set a rate provider for fiat rates. coindesk (default), coinbase or coingecko.
  `reject-on-amount-increase`: Reject if fiat amount changes in a way its unfavorable for you.
  `select-hints`: Select specific private channel routing hints to add to the invoice.
  `virtual`: Adds a fake pubkey as the destination and your real node intercepts the payment.
  `virtual-fee-rate`: Add the fee rate you want to charge for the final hop to the fake pubkey.
  <br></br>
  Example: `bos invoice 10*usd --description "For 6 pack beer"`
  Example: `bos invoice 50000 --virtual --virtual-fee-rate 100`
  <br></br>
  <br></br>

### limit-forwarding

Limits forwards through your node.

- Flags:
  - `disable-forwards`: Disable all forwards through your node.
  - `max-hours-since-last-block`: Requires fresh blocks before forwarding resumes.
  - `max-new-pending-per-hour`: Limit the number of pending HTLCs.
  - `min-channel-confirmation`: Minimum channel confs required.
  - `only-allow`: Only allow forwards from/to a pubkey.

  Example: `bos limit-forwarding --disable-forwards`
  <br></br>
  <br></br>

### lnurl

Perform a list of LNUrl functions

- Arguments:
  - `auth`: Authenticate to a website or an app that supports LNUrl login/sign up.
  - `channel`: Request to open an inbound payment channel using LNUrl.
  - `pay`: Pay to a Bolt-11 pay request (invoice) returned from a LNUrl or lightning address.
  - `withdraw`: Withdraw from an lnurl withdraw server by passing a BOLT 11 invoice.
- Flags:
  - `avoid`: Avoid a node, channel or a `bos tag` while paying to a LNUrl pay request.
  - `max-fee`: Maximum fees to be paid when paying the invoice, default: 1337.
  - `max-paths`: Maximum number of paths to use while paying to a LNUrl pay request.
  - `out`: Specify an out peer to pay the lnurl pay request.
  - `url`: LNUrl that returns an invoice to pay to.
  <br></br>
  Example: `bos lnurl pay --url lightning:LNURL1DP68GURN8GHJ7MRWW4EXCTT5DAHKCCN00QHXGET8WFJK2UM0VEAX2UN09E3K7MF0W5LHZ0F5XAJXZVNYXQUNGDTRXGERYVFCXYERGCTXX33R2VR9XG6NXEP3VYUNWE3EVEJNSE3SVEJRGCNZV56KXVTXVYERQWR9X5ER2DEKVCUXYDWUW2V --max-fee 600 --avoid ban`
  <br></br>
  <br></br>

### nodes

Adds a saved node for you to control remotely

- Options:
  - `nodeName`: Enter the name of the node, new or existing
- Flags: 
  - `add`: will add a new saved node, will ask you a series of questions to fill out. 
  - `remove`: removes an existing saved node 
  - `unlock`: removes encryption on the macaroon of a saved node 
  - `lock`: encrypt a saved node using a GPG key
  <br></br>
  <br></br>

### open

Helps to open channels to the network, batch opening and funding from external/cold wallet is supported. Open also supports p2tr and multisig funding for external funding of channels. Open also supports opening trusted funding channels.
**IF USING EXTERNAL WALLET, DO NOT BROADCAST THE TRANSACTION FROM THE EXTERNAL WALLET, BOS WILL DO IT FOR YOU**

- Arguments:
  - `pubkey`: public key of the node you want to open a channel to. Can enter multiple with a space in between.
- Flags: 
  - `amount`: capacity of the channel in Sats you want to open, can specify a separate amount if batch opening channels, default 5M sats if not specified 
  - `avoid-broadcast`: Avoid broadcast of funding transaction
  - `external-funding`: give you an address for you to sign from your external wallet along with the amount. **IF USING EXTERNAL WALLET, DO NOT BROADCAST THE TRANSACTION FROM THE EXTERNAL WALLET, BOS WILL DO IT FOR YOU** 
  - `internal-fund-at-fee-rate`: Add an internal fund fee rate to open a channel to skip the interactive dialog that asks for you for internal/external funding.
  - `opening-node`: Add an opening node for each pubkey to open channels on multiple saved nodes in the same transaction.
  - `set-fee-rate`: waits until the channel is open and attempts to set a forwarding fee rate, this process needs to run in the background until a channel is open. Have to run in background process manager like tmux, nohup or keep the ssh session open 
  - `type`: public/private/public-trusted/private-trusted, default: public
  - `coop-close-address`: Add an external wallet address like your cold storage wallet to send funds when a channel is coop closed.
  - `set-fee-rate`: Set a fee rate on opening a channel when supported.
  - `skip-anchors-check`: Bos defaults to opening anchor channels only, this flag allows you to skip that check to open legacy channels.
  - `commitment`: Use this flag to specify the channel commitment type like `simple_taproot` to open a taproot channel.
  <br></br>
  Example: `bos open pubkey1 --amount 1000000 pubkey2 --amount 3000000 pubkey3 --amount 4000000`. Once you enter the command and hit enter, it will ask the onchain transaction fee you want to set and also if you want to use your internal LND wallet for funding the transaction.
  <br></br>
  <br></br>

### open-balanced-channel

Lets you open a balanced channel with your peer, both peers involved need to have keysend turned on. Funding from external/cold wallet is supported. **IF USING EXTERNAL WALLET, DO NOT BROADCAST THE TRANSACTION FROM THE EXTERNAL WALLET, BOS WILL DO IT FOR YOU**

- Flags: 
  - `recover`: Enter the address if funds were accidentally sent to it.
  - `coop-close-address`: Adds a closing addresses to send funds to when the channel is cooperatively closed.
  <br></br>
  Simple running the command `bos open-balanced-channel` will ask you a series of questions to enter, like the `pubkey`, `total capacity` of the channel and the funding `fee rate`. It then key sends all that information to your peer to fund the other half for the channel. Your peer needs to run the same command to accept the request and review all information and agree to it, then the 1st peer or initiator will broadcast the transaction.
  <br></br>
  ![Balanced Channel Open](./images/balancedopen.jpg)
  <br></br>
  <br></br>

### open-group-channel

Lets you open balanced channels with multiple peers. This is an interactive command where each participant in the group runs the command and one participant initiates. Here's a video of how it works:

https://user-images.githubusercontent.com/84944042/184359226-df90d4fb-e644-4667-ab0a-5e7fe0693f26.mov

  <br></br>
  <br></br>

### outbound-liquidity

Returns your total inbound liquidity you currently have

- Flags: 
  - `above` returns tokens above a number you specify 
  - `below` returns tokens below a number you specify 
  - `with` with a specific peer public key 
  - `top` returns liquidity in the top percentile for an individual channel 
  - `with` use a `bos tag` to view outbound liquidity with peers in a tag
  <br></br>
  Example: `bos outbound-liquidity` or `bos outbound-liquidity --with yourPeerPubkey`
  <br></br>
  <br></br>

### pay

This command is used to pay a payment request (Invoice)

- Arguments:
  - `request`: Enter the invoice you want to pay
- Flags: 
  - `avoid`: When paying the payment request you can set to avoid a node by entering a public key or a specific channel by entering a channel ID. You can also avoid multiple nodes/channels by using the avoid flag multiple times. You can also use a `bos tag` (more on this in a separate command below) to avoid a group of nodes or channels by grouping them together. 
  - `avoid-high-fee-routes`: Ignore trying paths with fees greater than specified fees.
  - `out`: Pay the payment request out from a specifc peer of yours so the first hop is through that peer. 
  - `in`: Enter a pubkey if you want the last hop to be through a specific in peer of the destination node.
  Note: If you create an invoice yourself, and you pay it using an out peer and an in peer of yours, it becomes a command that can you do rebalance with. 
  - `message`: Enter a message of your choice to be attached to the payment request 
  - `max-fee`: Max total routing fees you're willing to pay in order to pay the payment req. Default: 1337 
  - `max-paths`: You can use multi path payments to pay the payment req via multiple paths, bos splits the payment and sends it out. Default: 1
  <br></br>
  Example: `bos pay invoiceToBePaid --avoid 03f10c03894188447dbf0a88691387972d93416cc6f2f6e0c0d3505b38f6db8eb5 --avoid bannedNodes --out 02c91d6aa51aa940608b497b6beebcb1aec05be3c47704b682b3889424679ca490 --avoid bannedNodes --max-fee 100`
  <br></br>
  Here `bannedNodes` is an example `bos tag` name.
  <br></br>
  <br></br>

### peers

Lists your current peers that you have channels with in a table view.

- Flags: 
  - `active`: Shows all your active peers (not offline) 
  - `complete`: Outputs a detailed view and does not use the table view. 
  - `fee-days`: If you enter the number of days, it shows the peers you have earned fees with over N number of days along with the fees earned in a separate column 
  - `filter`: You can apply filter formulas to display results, filter takes the column names as the filters, they include AGE, INBOUND_LIQUIDITY, OUTBOUND_LIQUIDITY. You can use filters like this `OUTBOUND_LIQUIDITY>100000` and it will filter results accordingly. You can also use formula expressions like `m` for million and `k` for 100k, example `OUTBOUND_LIQUIDITY<1*m` - `idle-days`: If you enter the number of days, it shows the peers you had no activity over N number of days, it includes both routing and payments received - `omit`: enter a public key to omit that peer from the list - `private`: shows peers you have private channels with - `public`: shows peers you have public channels with - `sort`: you can sort by column name, example: `sort OUTBOUND_LIQUIDITY` - `tag`: show peers that you have added to your tag, more on this in another command called `bos tags` below.
  `filter` variable support `AGE`, `BLOCKS_SINCE_LAST_CHANNEL`, `CAPACITY`, `DISK_USAGE_MB`, `INBOUND_LIQUIDITY`, `OUTBOUND_LIQUIDITY`.
  <br></br>
  Example: `bos peers --active --filter OUTBOUND_LIQUIDITY>5*M --idle days 10`
  <br></br>
  <br></br>

### price

Shows the current price of Bitcoin from the rate provider coindesk (default)

- Options:
  - `symbols`: You can use currency ticker symbols to get the price in the fiat currency of your choice. Example: `bos price AUD` for Australian Dollar, `GBP` for British Pound, its defaulted to `USD`
- Flags: 
  - `file`: Enter the path to a JSON file to write the output to a file 
  - `from`: You can pick the rate provider from coindesk (default), coinbase or coingecko
  <br></br>
  Example: `bos price GBP --from coingecko` or just `bos price` for USD and from coindesk
  <br></br>
  <br></br>

### probe

Simulate a payment for a certain amount and it will be simulated through the conditions specified, probing sends junk HTLCs to check if a real payment can go though. Can be used to check rebalance routes, payment routes as well or check the approximate liquidity available on a node via a particular channel.

- Arguments:
  - `pubkey`: Enter the destination pubkey you want to probe, can be yours as well if you want to probe yourself for a rebalance.
  -  OR `invoice`: Enter an invoice you want to probe (e. g. for probing private nodes which are not known to the network).
- Options:
  - `amount`: Amount you want to probe for
- Flags: 
  - `avoid`: When probing you can set to avoid a node by entering a public key or a specific channel by entering a channel ID. You can also avoid multiple nodes/channels by using the avoid flag multiple times. You can also use a `bos tag` (more on this in a separate command below) to avoid a group of nodes or channels by grouping them together. 
  - `avoid-high-fee-routes`: Ignore trying paths with fees greater than specified fees.
  - `max-fee`: the maximum total fees in sats that you want to use for probing. 
  - `out`: Probe out from a specifc peer of yours so the first hop is through that peer. 
  - `in`: Enter a pubkey if you want the last hop to be through a specific in peer of the destination node.
  Note: If you enter your pubkey, and you probe it using an out peer and an in peer of yours, it becomes a command that can you simulate a rebalance with. 
  - `find-max`: Find the maximum amount you can route when you simulate a payment
  <br></br>
  Example: `bos probe 03f10c03894188447dbf0a88691387972d93416cc6f2f6e0c0d3505b38f6db8eb5 3000000 --out 02c91d6aa51aa940608b497b6beebcb1aec05be3c47704b682b3889424679ca490 --avoid crapNodes`
  <br></br>
  <br></br>

### rebalance

Rebalances your channels by moving liquidity between your peers. A rebalance moves local funds from one peer to another peer of yours which helps in gaining inbound liquidity in the channel the funds are leaving and gaining outbound liquidity in the channel the funds are arriving.

- Flags: 
  - `amount`: The maximum amount you want to rebalance 
  - `out`: this flag can take the pubkey or Alias of your peer, its the first hop where you want the funds to leave from. 
  - `in`: this flag can take the pubkey or Alias of your peer, its the last hop where the funds arrive into. 
  - `avoid`: this can take pubkey, channel ID or a `bos tag` (more on this in a separate command below) where you can group multiple pubkeys to avoid while doing a rebalance. Avoid can take filters as well, explained in example below. 
  - `avoid-high-fee-routes`: Avoids routes above the specified fee rate.
  - `in-target-outbound`: the amount of outbound you want to target for a peer's channel where the funds are coming into. 
  - `out-target-inbound`: the amount of inbound you want to target for a peer's channel through which the funds are going out of. 
  - `max-fee-rate`: the maximum fee rate in ppm that you want to pay for your rebalance 
  - `max-fee`: the maximum total fees in sats that you want to pay for your rebalance 
  - `minutes`: the maximum time you want the rebalance to run if it does not succeed or fail within the time you specified. The command will time-out after N number of minutes you specify. 
  - `in-filter`: the set of in-peers you want the rebalance to filter through, this option is used with `bos tags`. - `out-filter`: the set of out-peers you want the rebalance to filter through, this option is used with `bos tags`.
  <br></br>
  Example: ` bos rebalance --out "WalletOfSatoshi.com" --in "EDON" --out-target-inbound=capacity*0.85 --max-fee-rate 700 --max-fee 2000 --minutes 20 --avoid ban --avoid "035e4ff418fc8b5554c5d9eea66396c227bd429a3251c8cbc711002ba215bfc226/OR(FEE_RATE<80 , FEE_RATE>500)" --avoid "035e4ff418fc8b5554c5d9eea66396c227bd429a3251c8cbc711002ba215bfc226/HEIGHT<600000" --avoid "035e4ff418fc8b5554c5d9eea66396c227bd429a3251c8cbc711002ba215bfc226/opposite_fee_rate<100" --avoid "fee_rate<50/0296b46141cd8baf13f3eff9bb217c5f62ce0a871886559d661af0ef422c042d4b"`
  <br></br>
- **Breaking down this example:** - `--out "WalletOfSatoshi.com"` is the peer's channel where local funds move out from so it's the peer you want to gain inbound. - `-- in "EDON"` is the peer's channel where funds leaving from the WalletOfSatoshi.com channel are coming into and local balance is increased or outbound is gained. - `--out-target-inbound=capacity*0.85` This flag indicates that you want the channel with WalletOfSatoshi.com to have inbound liquidity of 85% of the total channel capacity, for example, if you have a 1M channel with WoS, you're targetting 850K of inbound. If you want a 50-50 channel with WoS, it would be `--out-target-inbound=capacity*0.5` OR `--out-target-inbound=capacity/2` - `--max-fee-rate 700` is the maximum fee rate in ppm you're willing to pay for the total rebalance - `--max-fee 2000` is the total maxmimum fee in sats you're willing to pay for the rebalance including base fee. <br></br>
  **Note: Specify both values during a rebalance, the rebalance will fail if it finds a route and does not satisfy one of the criterias**<br></br> - `--minutes 20` the rebalance will time out after 20 minutes of path finding, the time-out occurs if the rebalance does not succeed/fail within the specified time. - `--avoid ban` "ban" is the name of the `bos tag` that I created, a tag can be named anything of your choice and you can add multiple pubkeys to the tag to categorize nodes. In this example, nodes specified in the tag "ban" will be avoided during path finding.
  - **Avoid Filters:**
    - avoid flag can take filter formulas along with a pubkey to filter channels of the pubkey specified during path finding.
    - Syntax: `--avoid pubkey/forumla`: If the syntax is pubkey followed by formula, the filter applies to channels of the node specified in the outbound direction.
    - Syntax: `--avoid formula/pubkey`: If the syntax is formula followed by pubkey, the filter applies to channels of the node specified in the inbound direction.
  - **Example Breakdown Continued:**
    - `--avoid "035e4ff418fc8b5554c5d9eea66396c227bd429a3251c8cbc711002ba215bfc226/OR(FEE_RATE<80 , FEE_RATE>500)"`, the pubkey specified in the filter belongs to WalletOfSatoshi.com, this formula implies you want to avoid all channels of WalletOfSatoshi that they charge less than 80ppm or greater than 500ppm. For example, if you just want to avoid a peer's channels greater than 200ppm the syntax is: `--avoid "pubkey/FEE_RATE>200)`
    - `--avoid "035e4ff418fc8b5554c5d9eea66396c227bd429a3251c8cbc711002ba215bfc226/HEIGHT<680000"` the pubkey specified in the filter belongs to WalletOfSatoshi.com, this formula implies you want to avoid all channels of WalletOfSatoshi that were created before the block height of 680000, this block was mined on `2021-04-21 05:58` which implies you want to avoid all your WalletOfSatoshi's channels that were created before that date. The reason for using this could be, all old channels might not be well maintained and might not have liquidity in the direction you want.
    - `--avoid "035e4ff418fc8b5554c5d9eea66396c227bd429a3251c8cbc711002ba215bfc226/opposite_fee_rate<100"` the pubkey specified in the filter belongs to WalletOfSatoshi.com, this formula implies you want avoid all channels of WalletOfSatoshi.com where the opposite_fee_rate is less than 100 or the rate which WalletOfSatoshi's peers charge them to route in the opposite direction, this is the fee rate that does NOT apply to you in the path finding because its in the opposite direction. The purpose of this filter is, a lot of nodes today are using dynamic fee rates to indicate to the network the direction in which they have liquidity using tools like charge-lnd. In the formula, by specifying you want to avoid low fee rates in the opposite direction you're effectively saying you want to avoid channels of WoS that are charging low because the liquidity is on the side of WoS's peers which is not the direction you need it for your rebalance to succeed. If the fee is high in the opposite direction an assumption can be made that liquidity is on WoS's side which is favorable for the success of your rebalance.
  - **Example of inbound 2nd syntax:**
    - `--avoid "fee_rate<50/0296b46141cd8baf13f3eff9bb217c5f62ce0a871886559d661af0ef422c042d4b"` the pubkey specified in the filter belongs to EDON, this formula implies you want avoid all channels of coming into EDON that charge less than 50 ppm.
    - Similar inbound syntax can be applied to all other filters mentioned above.
      <br></br>
      **Note: Avoid formulas can be applied to any node in the graph, it is not limited to just your peers, you can replace the public key with any public key of your choice.**
      <br></br>
  - **Using --out-filter and --in-filter** - If you create `bos tags` for your peers (check the tags command on how to create tags), you can use them for rebalances. instead of specifying `--out` and `--in` peer you can specify a group of nodes you want and bos can pick from those nodes to do rebalances with. Note the peer must exist within a tag to be considered for rebalance attempts. Usage: `bos rebalance --out <TAG> --out-filter "inbound_liquidity<1*m" --in <TAG> --in-filter "outbound_liquidity<1*m" --out-target-inbound=capacity*0.85 --max-fee-rate 700 --max-fee 2000 --minutes 20 --avoid ban`
    <br></br>
    <br></br>

### reconnect

This command attempts to reconnect any disconnected peers, channels that are inactive are also treated as disconnected. DO NOT use this command with the `--node` flag.<br></br>
Example: `bos reconnect`, you can set to run a reconnect automatically in a cronjob like this: Run `crontab -e`, add this line and save it. `*/300 * * * * /bin/timeout -s 2 30 /home/ubuntu/.npm-global/bin/bos reconnect`, this runs the command every 5 hours. **Adjust your path according to where bos is installed on your node.** Running `which bos` can you give your path.
<br></br>
<br></br>

### remove-peer

Closes a channel with a connected peer.

- Options:
  - `public key`: Enter the public key of the peer you want to close the channel with.
- Flags: 
  - `active`: Makes sure the peer is online before closing the channel to ensure a coop close. 
  - `address`: if you want to send funds that you get back (your local balance) straight to an external wallet, enter the destination address. 
  - `fee-rate`: Set the fee rate for the closure. - `force`: Force close channel with a peer 
  - `filter`: Add a filter formula to remove a peer matching the filter, example: `--filter "capacity<1*m"`
  - `inbound-below`: close channels with peers below a certain inbound liquidity level 
  - `outbound-below`: close channels with peers whose outbound is below a certain number 
  - `omit`: omit a peer you don't want to close a channel with if they fall under filter criteria of other flags such as `inbound-below` 
  - `outpoint`: if you have multiple channels with the same peer, use this flag to only close one specific channel using `txid:vout`, you can get this value from `lncli listchannels` 
  - `private`: make sure you have a private channel with the peer 
  - `public`: make sure you have a public channel with the peer 
  - `offline`: check if the peer is offline
  <br></br>
  Example: `bos remove-peer pubkeyOfYourPeer --fee-rate 1 --force`. **You can quickly do `ctrl + c` if you accidentally selected the wrong peer.**
  <br></br>
  <br></br>

### send

This command is used to make a keysend payment using a node's pubkey.

- Arguments:
  - `pubkey or lnurl`: Enter the pubkey of the node you want to make a payment to You can also enter an lnurl or lightning address to pay to.
- Flags: 
  - `avoid`: When paying via keysend you can set to avoid a node by entering a public key or a specific channel by entering a channel ID. You can also avoid multiple nodes/channels by using the avoid flag multiple times. You can also use a `bos tag` (more on this in a separate command below) to avoid a group of nodes or channels by grouping them together. 
  - `avoid-high-fee-routes`: Ignore trying paths with fees greater than specified fees.
  - `out`: Keysend out from a specifc peer of yours so the first hop is through that peer. 
  - `in`: Enter a pubkey if you want the last hop to be through a specific in peer of the destination node.
  Note: If you enter your own pubkey, you can keysend using an out peer and an in peer of yours, it becomes a command that can you do rebalance with. Useful for rebalances below 50k sats since `bos rebalance` does not support rebalances below 50k sats. 
  - `message`: Enter a message of your choice to be attached to the keysend 
  - `max-fee`: Max total routing fees you're willing to pay in order to pay the payment req. Default: 1337
  - `max-fee-rate`: Max fee rate allowed to be paid for the keysend in ppm. 
  - `message-omit-from-key`: BOS by default adds your pubkey to the keysend message, you can add this flag to avoid it. 
  - `amount`: Add the amount in sats you want to keysend.
  <br></br>
  Example: `bos send pubKeytoPay --avoid 03f10c03894188447dbf0a88691387972d93416cc6f2f6e0c0d3505b38f6db8eb5 --avoid bannedNodes --out 02c91d6aa51aa940608b497b6beebcb1aec05be3c47704b682b3889424679ca490 --avoid bannedNodes --max-fee 100 --message "Welcome to plebnet. RTFW plebnet.wiki"`
  <br></br>
  `bos send nitesh_btc@lntxbot.com --amount 500"`
  <br></br>
  Here `bannedNodes` is an example `bos tag` name.
  <br></br>
  <br></br>

### swap

This command allows you to do a submarine swap with a peer. (Currently supports Testnet Only).

If you run a testnet node, simply run the command, it is interactive to do a submarine swap with a peer.
  <br></br>
  <br></br>

### tags

This commands allows you to create custom tags to categorize your peers. You can create a tag name of your choice and add pubkeys of nodes to the tags.

- Options:
  - `tagname`: Enter a tagname you want to create or an existing tag that you already have.
- Flags: 
  - `add`: add a public key to a tag 
  - `remove`: remove a public key from a tag 
  - `icon`: you can add an emoji to your tag
  <br></br>
  Example: `bos tags bannedNodes --add 03c2abfa93eacec04721c019644584424aab2ba4dff3ac9bdab4e9c97007491dda`.
  Simply running `bos tags` will display all your tags. Tags are stored in `~/.bos` folder, you can also edit your tags by editing the `tags.json` file. You can use `bos tags` in rebalance, peers, send, pay etc commands.
  <br></br>
  <br></br>

### telegram

This command allows you to connect bos to a personal telegram bot. https://plebnet.wiki/wiki/Umbrel_-_Installing_BoS. Scroll down to the <b>Installing Telegram Bot</b> section and follow along to setup your bot.
  - Flags:
    - `budget`: Enter a budget amount that allows you to pay invoices from telegram upto the budget amount. <b>(Not a very good idea to use this flag for security reasons).</b>
    - `connect`: Enter a connect code to connect to your bot.
    - `ignore-forwards-below`: Enter an amount and forwards below the entered amount will be ignored in the notifications you receive.
    - `reset-api-key`: Allows you to start the setup process from start and enter a new API key.
    - `use-proxy`: Pass a flag to a json file that stores socksproxy (such as Tor) information for bos to communicate with telegram. Supports host, port, userId and password keys
    - `use-small-units`: This flag switches units on telegram from BTC to Satoshis. Example: Default `0.00456123` will be displayed as `456,123.2003` which includes millisats.
    - `use-rounded-units`: Formats amounts as rounded amounts.
    <br></br>
    Sample File `proxy-config.json` (you can use any name of your choice): 
    ```
    {
      "host": "127.0.0.1",
      "port": 9050
    }
    ```
    <br></br>
  Example: `bos telegram --connect 123456789 --use-proxy="/home/ubuntu/someFolder/someConfig.json"`
  <br></br>
  <br></br>

### trade-secret

This commands allows you to trade between peers (p2p trading), for example invite to a telegram group, sell gift card codes and much more.
<br></br>
Example: Simply run `bos trade-secret`, it will ask you to create a trade or decode a trade, look at your open trades and serve stopped trades. Supports both open and closed trades. Run the command and you will be presented with options. An open trade is where you will not be entering the pubkey of the node your trading with and any node can purchase secrets from you by connecting with you and exchanging information of LN p2p messaging. A closed trade involves entering a pubkey and the trade will be encoded with the peer's pubkey and only that specific node can decode the trade.
Trade Secret now also supports p2p channel sales and all trades in fiat (USD).
<br></br>
<br></br>

### utxos

Returns a list of your UTXOS.

- Flags: 
  - `confirmed`: returns only confirmed utxos. 
  - `count`: returns the number of utxos you have available in pending and confirmed 
  - `count-below`: returns a utxos count number below the number you specified 
  - `size`: returns utxos greater than the specified amount
  <br></br>
  Example: `bos utxos` or `bos utxos --confirmed --count`
  <br></br>
  <br></br>

## **Secret BOS Commands:**

**These commands don't come up in the help section of BOS:** <br></br>

### delete-payments-history

Deletes all your payments (not invoices). Helps in reducing the size of your DB. <br></br>**Important: Make sure to export your payments to a CSV using `bos accounting` if you want to keep a copy of them or need them to do accounting on your node.**

- This command has no arguments or flags (except the `--node` flag to use a saved node). Directly run it as `bos delete-payments-history`.
- It may take a LONG TIME for the command to finish executing.
  <br></br>
  <br></br>

### gift

Send a routing fee gift to a node of your choice. It executes a rebalance command through that peer so that they make money on routing fees.

- Arguments: 
  - `target`: Takes the node pubkey 
  - `amount`: Enter the amount you want to rebalance, the minimum amount you can enter depends on the minimum HTLC size and fee rate of the peer you're gifting routing fees to.
  <br></br>
  Example: `bos gift 03c5528c628681aa17ab9e117aa3ee6f06c750dfb17df758ecabcd68f1567ad8c1 100`
  <br></br>
  <br></br>

### encrypt

Encrypts data using your public key or another node's public key. You have to then use the `bos decrypt` command to decrypt the data and can be done only on the node whose key has been used to encrypt.

- Options: 
  - `message`: The message to encrypt 
  - `to`: The pubkey of the node you want to encrypt the data for, default: your pubkey.
  <br></br>
  Example: `bos encrypt --message "I love plebnet" --to 03c5528c628681aa17ab9e117aa3ee6f06c750dfb17df758ecabcd68f1567ad8c1`
  <br></br> You can then send the encrypted message to the node you used to encrypt, they can then use `bos decrypt` to decrypt the data.
  <br></br>
  <br></br>

### decrypt

Decrypts the encrypted data from `bos encrypt`

- Arguments: - `encrypted data`: Enter the data that needs to be decrypted.
  <br></br>
  Example: `bos decrypt a569656e637279707465644ecd12dacf44c83a11e6451bfbe5d362697650f16e8eb72c0e5ec94e31c0f3aaa18f7e6473616c745820c7a8e6540ef1c9ac33d17d82eab6b4a3128300f44b17b60eba9cbceff25e4b276873657474696e6773a669616c676f726974686d6b6165732d3235362d67636d6a64657269766174696f6e66736372797074666469676573746673686135313268656e636f64696e6764757466386a6b65795f6c6`
  <br></br>
  <br></br>

### recover-p2pk

If you accidentally sent on-chain funds to your public key instead of your wallet address, this command can help you sweep the funds back to your on-chain wallet.

- Arguments:
  - `id`: Transaction id of funds sent to p2pk
  - `vout`: Transaction output index of funds sent to p2pk
    <br></br>
    Example: `bos recover-p2pk 59qadc75d655cca5fa2qwef5ab87cd39fzxc99f563b9f71e0a5e245680fc6fd5 0 `
    <br></br>



## If the latest Umbrel version broke bos, here’s how to fix it. 
```

ls ~/.bos
```
If it throws an error saying: No such file or directory
```
mkdir ~/.bos && cd ~/.bos
```
If the directory exists:
```
cd ~/.bos
```
make a saved node directory, go into it and create a credentials file
```
mkdir anySavedNodeName && cd anySavedNodeName && nano credentials.json
```
Edit the JSON file
```
{
"cert_path":"/home/umbrel/umbrel/app-data/lightning/data/lnd/tls.cert",
"macaroon_path":"/home/umbrel/umbrel/app-data/lightning/data/lnd/data/chain/bitcoin/mainnet/admin.macaroon",
"socket":"umbrel.local:10009"
}
```
If umbrel.local:10009 doesn't work, try localhost:10009

Save the file and exit
```
ctrl + x 
y
```
At this point bos commands should work like this
```
bos balance --node anySavedNodeName
```
Now make the saved node as default saved node

Go back to bos dir
```
cd ~/.bos
```
Make a config file 
```
nano config.json
```
Add the config setting
```
{"default_saved_node":"anySavedNodeName"}
```
Save and exit

Now run bos commands normally
```
bos balance
```
