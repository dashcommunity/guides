# Submitting a Proposal to the Dash Network

This guide will step you through submitting a proposal ([created here in the Dash Community](https://github.com/dashcommunity/guides/blob/master/create_proposal_from_template.md)) to the dash network.

Dash proposals are submitted to the network using the `dash-cli` (dash's command line interface) program, which can be executed using either a shell terminal emulator (e.g. Terminal on Mac) or using the Dash-Qt wallet console. In order to run the proper commands you will first want to gather the information required by the dash protocol.  The steps below walk you through this process.

### Gather Required Information

`dash-cli` requires the following arguments

##### Required arguments 
    `dash-cli` argument   |             Description             |                             Example                             
--------------------------|-------------------------------------|----------------------------------------------------------------
 `<proposal-name>`        | unique label, 20 characters or less | `dash-community`                                                
 `<url>`                  | webpage showing your proposal       | `https://github.com/dashcommunity/proposal-riongull-2016-10-06` 
 `<payment-count>`        | requested months of payment         | `1`                                                             
 `<block-start>`          | requested start of payments         | `564944`                                                        
 `<dash-address>`         | address to receive payments         | `XnYHMmpTpS8zYpZmdRxbvvA8RqGLSjXJVg`                            
 `<monthly-payment-dash>` | requested payment amount            | `90`                                                            


### Prepare commands using dashcentral.org

1. Go to dashcentral.org
2. Fill out form
3. Etc.


### Submit proposal using Dash-Qt commands

1. Open Dash-Qt
2. Type command x to prepare your budget
3. Type command y to submit your budget
