# Submitting a Proposal to the Dash Network

This guide will step you through submitting a proposal ([optionally created here at Dash Community](https://github.com/dashcommunity/guides/blob/master/create_proposal_from_template.md)) to the dash network.

##### Context
Like most core functions in dash (e.g. transactions, etc), proposals are submitted to the network using the `dash-cli` program, dash's command line interface.  This program is built into the debug console of the dash-Qt wallet.  That is the most accessible place to run the program.  If you have [set up dash masternode](https://github.com/dashcommunity/guides/blob/master/set_up_masternode.md) using your own [hosted VPS](https://github.com/dashcommunity/guides/blob/master/set_up_virtual_private_server.md), `dash-cli` is also available on your server.  

This guide assumes you are submitting a proposal using your Dash-Qt wallet's console.  You can, however do this from your server by:

1. prepending each command with `./dash-cli`
2. running the commands below from the directory containing `dash-cli`

### Submit Command
The command for submitting a proposal (from Dash-Qt) takes the form:

`mnbudget submit <proposal-name> <url> <payment-count> <block-start> <dash-address> <monthly-payment-dash> <fee-tx>`

All of the `<items-in-brackets>` are place holders for information you provide.  This guide works in tandem with the [guide at dashcentral.org](https://www.dashcentral.org/budget/create) to help you prepare and submit your proposal.

##### Required Information
The following table shows the arguments/informaiton required to submit a proposal.


         Argument         |             Description          |                             Example                             
--------------------------|----------------------------------|----------------------------------------------------------------
 `<proposal-name>`        | unique label, 20 characters max  | `dash-community`                                               
 `<url>`                  | webpage of your proposal         | `https://github.com/dashcommunity/proposal-riongull-2016-10-06`
 `<payment-count>`        | requested months of payment      | `1`                                                            
 `<block-start>`          | requested start of payments      | `564944`                                                       
 `<dash-address>`         | address to receive payments      | `XnYHMmpTpS8zYpZmdRxbvvA8RqGLSjXJVg`                           
 `<monthly-payment-dash>` | requested payment amount         | `90`                                                           
 `<fee-tx>`               | ouput from `mnbudget prepare`    | `fjdklfjdskl`

### Steps to Submit a Proposal

1. Create your proposal
    * you may [create and host your proposal](https://github.com/dashcommunity/guides/blob/master/create_proposal_from_template.md) here at Dash Community, idealy using one of the [templates](https://github.com/dashcommunity/proposal-templates), or
    * you may use the templates and method show on step 1 at [dashcentral.org](https://www.dashcentral.org/budget/create)
2. Complete steps 2 through 7 at dashcentral
3. Open your Dash-Qt wallet
4. Select `Tools` > `Debug console` from the Dash-Qt main menu
5. Paste the command shown on step 8 from dashcentral; before hitting enter, note that: 
  * this will charge you 5 DASH
  * it doesn't actually submit your budget
  * it prepares the budget, i.e. it returns a receipt that you have paid the submission fee
  * the receipt is a transaction id called `fee-tx`, which is used later when you run `mnbudget submit`
6. Hit enter to prepare your proposal
7. Paste the command shown on step 9 from dashcentral, where `REPLACE_WITH_COLLATERAL_HASH` (same as `fee-tx`) is the string of characters output by the previous command
8. Hit enter to submit your proposal
9. Continue with steps 10 through 13 at dashcentral
  * one option for step 12 would be to include the Overview section on dashcentral, and then include a link to your full proposal hosted here at Dash Commmunity