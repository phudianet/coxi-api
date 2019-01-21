# Coxi.io Public API

## Usage

To use public API you can send a simple requests in format:
```
https://coxi.io/public/graphs/<FIRST_CURRENCY><SECOND_CURRENCY>/<TIME_PERIOD>.json
```

Example for last day in BTC to XRP market:
```
https://coxi.io/public/graphs/BTCXRP/1day.json
```

### Time
* `6hour`
* `1day`
* `7days`
* `1month`


## Ticker
```
https://coxi.io/public/graphs/ticker/ticker.json
```

Ticker returns an array of Coxi.io markets' statuses.

### Response
Server will respond with 70 entries for chosen parameters.

```json
[{"time":1499628480,"open":"9680.30000000","high":"9680.30000000","low":"9680.30000000","close":"9680.30000000","vol":"1.50000000"},
{"time":1499629440,"open":"10000.00000000","high":"10000.00000000","low":"10000.00000000","close":"10000.00000000","vol":"2.70000000"},
{"time":1499630400,"open":"10000.00000000","high":"10000.00000000","low":"10000.00000000","close":"10000.00000000","vol":"0.00000000"}]
```

#### Explanation
* `time` - POSIX timestamp
* `open` - first offer in given time period
* `high` - highest offer in given time period
* `low` - lowest offer in given time period
* `close` - last offer in given time period
* `vol` - a volume in given time period
