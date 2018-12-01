# Coxi.io Private API

* `API URL`: https://coxi.io/api/
* `API key`: individual API key, found in Coxi.io user settings
* `API pin`: individual password, found in Coxi.io user settings

## Requests Limits

* Priority `GET` methods: 5 requests per second
* Priority `SET` methods: 1 request per 2 seconds

A priority of each method is specified in its documentation.

When the limits are exceeded, you should see error _429 - please slow down ;-)_.

## Usage

All requests should be done using HTTP `POST` method, with two obligatory data
fields:
* `method`: a name of the method to invoke
* `time`: current POSIX timestamp with microseconds

and two obligatory HTTP headers:
* `key`: your **API key**
* `hash`: a keyed SHA512 hash value of the `POST` data fields (including
  `method` and `time`) using HMAC method with your **API pin** as a key

**NOTE:** `POST` data fields for the **hash** should be provided as a string in
format:
```
FIELD1=VALUE1&FIELD2=VALUE2&FILED3=VALUE3
```

## Methods

* [`info`](#info) (`SET`)
* [`trade`](#trade) (`SET`)
* [`trades`](#trades) (`GET`)
* [`orders`](#orders) (`GET`)
* [`cancel`](#cancel) (`SET`)
* [`history`](#history) (`GET`)
* [`markethistory`](#markethistory) (`GET`)
* [`time`](#time) (`GET`)

### `info`

Get info about your account.

#### Example output
```json
{
  "balance": {
    "PLN": 0.0,
    "EUR": 0.0,
    "USD": 0.0,
    "BTC": 0.0,
    "XRP": 0.0,
    "ETH": 0.0
  },
  "locked": {
    "XRP": {
      "transaction": [
        {
          "amount": 0.0,
          "price_amount": 1.0,
          "id_action": 1
        }
      ],
      "withdrawals": 0.0
    }
  },
  "wallet": {
    "btc": "XXX",
    "xrp": "YYY",
    "eth": "ZZZ"
  }
}
```

##### Explanation
* `balance` - your current balance in each currency
* `locked` - info about your current transactions and withdrawals for each
  currency
  * `transaction` - what you want to trade
    * `amount` - amount of currency
    * `price_amount` - exchange rate
    * `id_action`:
      * `1` - standard buy / sell
      * `2` - quick buy / sell
      * `3` - bot buy / sell
  * `withdrawals` - an amount waiting for withdraw
* `wallet` - your private Coxi.io wallet addresses for each cryptocurrency

### `trade`

Make a new offer on the Coxi.io market.

#### Fields
* `action` - mandatory, should have value `standard`
* `id_action` - `1` for buy, `2` for sell (defaults to buy)
* `amount` - the amount to trade
* `price` - the exchange rate for given amount
* `currency_amount` - a currency to sell
* `currency_price` - a currency to buy

#### Example output
```json
{
  "status": "success",
  "info": {
    "transactions": {},
    "market": {}
  }
}
```

##### Explanation
* `status` - indicates whether trade was successful
* `info` - contains information about the trade
* `transactions` - an array with details of the offers
* `market` - information about your interaction with market or `false` if
  nothing happened

### `cancel`

Cancel transaction with given ID.

#### Fields
* `id` - an identifier of transaction you want to cancel

#### Example output
```json
{
  "status": "success",
  "info": {}
}
```

##### Explanation
* `status` - indicates whether transaction has been cancelled successfully
* `info` - contains detailed information about cancelled transaction

### `orders`

List all active orders in market.

#### Fields
* `id_action` - `1` for buy, `2` for sell (defaults to buy)
* `currency_amount` - a currency being sold
* `currency_price` - a currency being bought

#### Example output
```json
{
  "start": 0,
  "limit": 50,
  "total": 1,
  "result": [
    {
      "id": 1,
      "type": "sell",
      "who": "user",
      "cryptoAmount": 1.0,
      "cryptoCurrency": "XRP",
      "mainAmount": 1.0,
      "mainCurrency": "EUR",
      "total": 1.0,
      "totalCurrency": "EUR",
      "datetime": "2018-12-01 10:00:00"
    }
  ]
}
```

##### Explanation
* `start` - number of the first order to be listed (starts with `0`)
* `limit` - maximum orders listed
* `total` - actual number of listed orders
* `result` - a list of orders
  * `id` - an identifier of the offer
  * `type` - a type of the offer (`buy` or `sell`)
  * `who` - `user`, `express` or `bot`
  * `cryptoAmount` - an amount of currency you offer
  * `cryptoCurrency` - a currency you offer
  * `mainAmount` - an amount of FIAT currency you offer
  * `mainCurrency` - a FIAT currency you offer
  * `total` - a total value of your offer
  * `totalCurrency` - a currency used for the value of `total`
  * `datetime` - date and time when order was issued

### `trades`

List all active trades on the market.

#### Fields
* `id_action` - `1` for buy, `2` for sell (defaults to buy)
* `currency_amount` - a currency being sold
* `currency_price` - a currency being bought

#### Example output
```json
{
  "start": 0,
  "limit": 100,
  "total": 1,
  "result": [
    {
      "id": 1,
      "type": "sell",
      "who": "user",
      "cryptoAmount": 1.0,
      "cryptoCurrency": "XRP",
      "mainAmount": 1.0,
      "mainCurrency": "EUR",
      "total": 1.0,
      "totalCurrency": "EUR",
      "datetime": "2018-12-01 10:00:00"
    }
  ]
}
```

##### Explanation
* `start` - number of the first order to be listed (starts with `0`)
* `limit` - maximum orders listed
* `total` - actual number of listed orders
* `result` - a list of orders
  * `id` - an identifier of the offer
  * `type` - a type of the offer (`buy` or `sell`)
  * `who` - `user`, `express` or `bot`
  * `cryptoAmount` - an amount of currency you offer
  * `cryptoCurrency` - a currency you offer
  * `mainAmount` - an amount of FIAT currency you offer
  * `mainCurrency` - a FIAT currency you offer
  * `total` - a total value of your offer
  * `totalCurrency` - a currency used for the value of `total`
  * `datetime` - date and time when order was issued

### `history`

#### Fields
* `id_action` - `1` for buy, `2` for sell (defaults to buy)
* `currency_amount` - a currency being sold
* `currency_price` - a currency being bought
* `limit` - limit results from API (`2` - `800`)
* `since` - an identifier of transaction the history should start with


#### Example output
```json
{
  "start": 0,
  "limit": 100,
  "total": 1,
  "result": [
    {
      "id": 1,
      "type": "sell",
      "who": "user",
      "cryptoAmount": 1.0,
      "cryptoCurrency": "XRP",
      "mainAmount": 1.0,
      "mainCurrency": "EUR",
      "total": 1.0,
      "totalCurrency": "EUR",
      "datetime": "2018-12-01 10:00:00"
    }
  ]
}
```

##### Explanation
* `start` - number of the first order to be listed (starts with `0`)
* `limit` - maximum orders listed
* `total` - actual number of listed orders
* `result` - a list of orders
  * `id` - an identifier of the offer
  * `type` - a type of the offer (`buy` or `sell`)
  * `who` - `user`, `express` or `bot`
  * `cryptoAmount` - an amount of currency you offer
  * `cryptoCurrency` - a currency you offer
  * `mainAmount` - an amount of FIAT currency you offer
  * `mainCurrency` - a FIAT currency you offer
  * `total` - a total value of your offer
  * `totalCurrency` - a currency used for the value of `total`
  * `datetime` - date and time when order was issued

### `markethistory`

#### Fields
* `id_action` - `1` for buy, `2` for sell (defaults to buy)
* `currency_amount` - a currency being sold
* `currency_price` - a currency being bought
* `limit` - limit results from API (`2` - `800`)
* `since` - an identifier of transaction the history should start with


#### Example output
```json
{
  "start": 0,
  "limit": 100,
  "total": 1,
  "result": [
    {
      "id": 1,
      "type": "sell",
      "who": "user",
      "cryptoAmount": 1.0,
      "cryptoCurrency": "XRP",
      "mainAmount": 1.0,
      "mainCurrency": "EUR",
      "total": 1.0,
      "totalCurrency": "EUR",
      "datetime": "2018-12-01 10:00:00"
    }
  ]
}
```

##### Explanation
* `start` - number of the first order to be listed (starts with `0`)
* `limit` - maximum orders listed
* `total` - actual number of listed orders
* `result` - a list of orders
  * `id` - an identifier of the offer
  * `type` - a type of the offer (`buy` or `sell`)
  * `who` - `user`, `express` or `bot`
  * `cryptoAmount` - an amount of currency you offer
  * `cryptoCurrency` - a currency you offer
  * `mainAmount` - an amount of FIAT currency you offer
  * `mainCurrency` - a FIAT currency you offer
  * `total` - a total value of your offer
  * `totalCurrency` - a currency used for the value of `total`
  * `datetime` - date and time when order was issued

### `time`

Check server time.

#### Example output
```json
{
  "status": "success",
  "timestamp": 0.0,
  "datetime": "2018-12-01 10:00:00",
  "tolerance": 0.0
}
```

##### Explanation
* `status` - indicates whether request was successful
* `timestamp` - POSIX time with microseconds
* `datetime` - date and time in `Y-m-d %H:%i:%s` format
* `tolerance` - time difference tolerance in seconds
