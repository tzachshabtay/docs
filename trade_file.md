<div class="center">
<p align="center"><img src="assets/logo.png" align="center" width="20%" height="20%"></p>
  <h1 align="center">Clear Street</h1>
  <p align="center">
  	<h2 align="center">
    	Trade File
  	</h2>
	</p>
</div>

Clear Street's trade file is a CSV file that contains post-trade details. This file is used by Clear Street to manage the life-cycle of all post-trade trading activity.

The trade file should be a CSV file with `*.csv` extension. The first row of the file must be the "header" row where you specifiy column names. Column names are case-insenstive, but we recommend you always use lowercase. Column ordering does *not* matter; you can specify the columns in the order you prefer.

Each row in the file represents a single trade. Columns that are unrecognized, or columns that might be conditionally unapplicable, are ignored. A trade has a `type` that dictates what columns apply to it. Columns that do not apply to a specific trade type are ignored. Therefore, you can provide all columns in your file, and selectively populate each column for a given row.

### Columns

| Name       | Type  | Description | Example |
| -----------| ------|-------------|----------------------------------------- |
| `type` | `string` | Trade type, either `bilateral_trade`, `exchange_trade`, `allocation_trade`, `transfer_trade`, or `away_trade` | `exchange_trade` |
| `timestamp`  | `integer` |  Timestamp of when the trade occurred in milliseconds since unix epoch | `1571408966810` |
| `client_trade_id` | `string` | Your unique ID for this trade. Must be unique across days | `T-50264430-bc41` 
| `date`        | `integer` | Trade date for the trade in `YYYYMMDD` format | `20200101` |
| `account_id` | `integer` | Clear Street provided account_id the trade should be booked to | `999999` |
| `quantity` | `numeric` | The quantity of the trade (supports fractional quantities) | `100` |
| `price` | `numeric` | The price of the trade | `100.01` |
| `behalf_of_account_id` | `integer` | Clear Street provided account_id if this trade is on behalf of another account | `23` |
| `behalf_of_entity_id` | `integer` | [DEPRECATED (use `behalf_of_account_id`)] Clear Street provided entity_id if this trade is on behalf of another entity | `23` |
| `solicited` | `bool` | True if this trade was solicited, false otherwise | `false` |
| `registered_rep` | `string` | The registered rep on this trade | `joe` |
| `branch_office` | `string` | The branch office for this trade | `NY` |
| `instrument.identifier` | `string` | The identifier string, e.g. `AAPL` if `identifier_type` is `ticker` | `AAPL` |
| `instrument.identifier_type` | `string` | Identifier type, either `ticker`, `cusip`, `isin` or `sedol` | `ticker` |
| `instrument.country` | `string` | ISO 3166 alpha-3 country code where the instrument trades | `USA` |
| `instrument.currency` | `string` | ISO 4217 alpha-3 currency code in which the instrument trades | `USD` |
| `side.direction` | `string` | Either `buy` or `sell` | `buy` |
| `side.qualifier` | `string` | `short` | `short`
| `side.position`  | `string` | Either `open` or `close` | `open`
| `settlement.currency` | `string` | ISO 4217 alpha-3 currency code in which to settle this trade | `USD` |
| `settlement.date` | `string` | Explicit settlement date for irregular-way settlement in `YYYYMMDD` foramt | `20200101` |
| `capacity` | `string` | Either `principal`, `agency`, `mixed`, or `riskless_principal` | `mixed` |
| `contra_mpid` | `string` | Contra-party's MPID | `CLST` |
| `contra_clearing_num` | `string` | Contra-party's clearing number (DTCC for equities) | `9100` |
| `contra_dtc_num` | `string` | [DEPRECATED (use `contra_clearing_num`)] Contra-party's DTCC number | `9100` |
| `contra_side_qualifier` | `string` | Contra-party side qualifier, either `short`, `open`, or `close` | `short` |
| `is_when_issued` | `bool` | True if the trade should be considered as "when issued" | `false` |
| `exec_mpid` | `string` | MPID fo the executing party, if different than `contra_mpid` | `9192` |
| `fees.commission` | `string` | Commission charged or paid | `12.30` |
| `fees.omit_sec` | `bool` | True if SEC fees should not be applied | `false` |
| `fees.omit_taf` | `bool` | True if TAF fees should not be applied | `false` |
| `locate.id` | `string` | ID of the locate obtained for a short-sale | `A234` |
| `locate.source` | `string` | Identifies the firm supplying the locate (usually the MPID) | `Z322` |
| `target_account_id` | `integer` | Clear Street provided account_id that is the contra to this trade | `99999` |
| `mic` | `string` | ISO 10383 Market Identifer Code for the exchange where this trade took place | `XNYS` |
| `order_id` | `string` | The order id is to link all the executions in the avg price account to the allocation trade type | `13454` |
| `cancel_trade_id` | `string` | The original trade_id this trade is replacing | `13454` |

### Exchange Trade

This trade represents a trade between a trading entity and an exchange. For example, trading firm XYX buys 100 shares of AAPL directly on Nasdaq.

| Column | Required? | Default | Notes |
| - | - | - | - |
| `type` | Yes |
| `timestamp` | Yes |
| `client_trade_id` | Yes |
| `date` | Yes |
| `account_id` | Yes |
| `quantity` | Yes |
| `price` | Yes |
| `behalf_of_account_id` | No | `null` |
| `solicited` | Yes |
| `registered_rep` | No | `null`
| `branch_office` | No | `null` |
| `instrument.identifier` | Yes |
| `instrument.identifier_type` | Yes |
| `instrument.country` | Yes |
| `instrument.currency` | Yes |
| `side.direction` | Yes |
| `side.qualifier` | No | `null` |
| `side.position` | No | `null` |
| `settlement.currency` | No | `USD` |
| `settlement.date` | No | `null` |
| `capacity` | Yes |
| `mic` | Yes |
| `exec_mpid` | Yes |
| `is_when_issued` | No | `false` |
| `fees.commission` | No | `null` |
| `fees.omit_sec` | No | `false` |
| `fees.omit_taf` | No | `false` |
| `locate.id` | Conditional | `null` | Required if short-sale |
| `locate.source` | Conditional | `null` | Required if short-sale |
| `order_id` | No | `null` | Links executions in avg price account to allocation trade type
| `cancel_trade_id` | No | `null` | Straight cancel vs a correction of a trade from one account into another account


### Bilateral Trade

This trade represents a trade between two trading entities. For example, trading firm XYZ buys 100 share of AAPL from trading firm ABC.

| Column | Required? | Default | Notes |
| - | - | - | - |
| `type` | Yes |
| `timestamp` | Yes |
| `client_trade_id` | Yes |
| `date` | Yes |
| `account_id` | Yes |
| `quantity` | Yes |
| `price` | Yes |
| `behalf_of_account_id` | No | `null` |
| `solicited` | Yes |
| `registered_rep` | No | `null`
| `branch_office` | No | `null` |
| `instrument.identifier` | Yes |
| `instrument.identifier_type` | Yes |
| `instrument.country` | Yes |
| `instrument.currency` | Yes |
| `side.direction` | Yes |
| `side.qualifier` | No | `null` |
| `side.position` | No | `null` |
| `settlement.currency` | No | `USD` |
| `settlement.date` | No | `null` |
| `capacity` | Yes |
| `contra_mpid` | Yes |
| `contra_clearing_num` | No | Derived Value | If not supplied the value will be derived from an internal MPID to clearing number mapping | 
| `is_when_issued` | No | `false` |
| `exec_mpid` | Yes |
| `fees.commission` | No | `null` |
| `fees.omit_sec` | No | `false` |
| `fees.omit_taf` | No | `false` |
| `locate.id` | Conditional | `null` | Required if short-sale |
| `locate.source` | Conditional | `null` | Required if short-sale |
| `order_id` | No | `null` | Links executions in avg price account to allocation trade type
| `cancel_trade_id` | No | `null` | Straight cancel vs a correction of a trade from one account into another account
| `last_market` | No | `null` | Contains the exchange where orders were routed to by broker on a bilateral trade
| `nscc_clearing` | No | `null` | Bilateral trade type, either `contra`, `agu`, `qsr`, `corr`, `corr_fees` or `null`


### Allocation Trade

This trade type is used to facilitate average-price workflows, i.e. averaging many trades for a customer and allocating it to them as a single trade.

| Column | Required? | Default | Notes |
| - | - | - | - |
| `type` | Yes |
| `timestamp` | Yes |
| `client_trade_id` | Yes |
| `date` | Yes |
| `account_id` | Yes |
| `quantity` | Yes |
| `price` | Yes |
| `behalf_of_account_id` | No | `null` |
| `solicited` | Yes |
| `registered_rep` | No | `null`
| `branch_office` | No | `null` |
| `instrument.identifier` | Yes |
| `instrument.identifier_type` | Yes |
| `instrument.country` | Yes |
| `instrument.currency` | Yes |
| `side.direction` | Yes |
| `side.qualifier` | No | `null` |
| `side.position` | No | `null` |
| `settlement.currency` | No | `USD` |
| `settlement.date` | No | `null` |
| `target_account_id` | Yes |
| `capacity` | Yes |
| `contra_side_qualifier` | Yes |
| `fees.commission` | No | `null` |
| `fees.omit_sec` | No | `false` |
| `fees.omit_taf` | No | `false` |
| `order_id` | No | `null` | Links executions in avg price account to allocation trade type
| `cancel_trade_id` | No | `null` | Straight cancel vs a correction of a trade from one account into another account

### Transfer Trade

This trade type is to facilitate trade movement between Clear Street internal accounts. For example, trade movement from a proprietary account to an average price account.

| Column | Required? | Default | Notes |
| - | - | - | - |
| `type` | Yes |
| `timestamp` | Yes |
| `client_trade_id` | Yes |
| `date` | Yes |
| `account_id` | Yes |
| `quantity` | Yes |
| `price` | Yes |
| `behalf_of_account_id` | No | `null` |
| `solicited` | Yes |
| `registered_rep` | No | `null`
| `branch_office` | No | `null` |
| `instrument.identifier` | Yes |
| `instrument.identifier_type` | Yes |
| `instrument.country` | Yes |
| `instrument.currency` | Yes |
| `side.direction` | Yes |
| `side.qualifier` | No | `null` |
| `side.position` | No | `null` |
| `settlement.currency` | No | `USD` |
| `settlement.date` | No | `null` |
| `target_account_id` | Yes |
| `capacity` | Yes |
| `fees.commission` | No | `null` |
| `fees.omit_sec` | No | `false` |
| `fees.omit_taf` | No | `false` |
| `cancel_trade_id` | No | `null` | Straight cancel vs a correction of a trade from one account into another account

### Away Trade
This trade type allows customers to execute away from Clear Street. An example would be a customer executing at another broker/dealer but settling the trade at Clear Street. 
| Column | Required? | Default | Notes |
| - | - | - | - |
| `type` | Yes |
| `client_trade_id` | Yes |
| `timestamp` | Yes |
| `date` | Yes |
| `account_id` | Yes |
| `quantity` | Yes |
| `price` | Yes |
| `behalf_of_account_id` | No | `null` |
| `registered_rep` | No | `null`
| `branch_office` | No | `null` |
| `instrument.identifier` | Yes |
| `instrument.identifier_type` | Yes |
| `instrument.country` | Yes |
| `instrument.currency` | Yes |
| `side.direction` | Yes |
| `side.qualifier` | No | `null` |
| `capacity` | Yes |
| `exec_mpid` | Yes |
| `contra_mpid` | Yes |
| `contra_clearing_num` | No | Derived Value | If not supplied the value will be derived from an internal MPID to clearing number mapping |
| `fees.commission` | No | `null` |
| `cancel_trade_id` | No | `null` | Straight cancel vs a correction of a trade from one account into another account

### Insert vs. Cancel

By default, the trades provided in your file are trades you wish to *insert* into our systems. When you insert a trade into Clear Street, it is remembered forever with a `(account_id, client_trade_id)` pair. If you wish to cancel a trade, because perhaps your provided incorrect details, then you only need to provide us a file that contains the `(account_id, client_trade_id)` pair at a minimum.

Therefore, to cancel all trades in a file, you can provide us the same exact file you used to insert the trades. Or you can construct a new file that contains just the `account_id` and `client_trade_id` columns.
