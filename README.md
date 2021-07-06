# CCXT-SNIPPET
## Usefull and hard to fine CCXT Python snippets
#### MARKET Close position Futures with ccxt 
```python
close_sell_position = exchange.create_order(
    symbol=SYMBOL,
    type="MARKET",
    side="buy",  # buy//sell
    amount=VOLUME,
    params={"reduceOnly": "true"},
)
```


#### LIMIT Close position Futures with ccxt
```python
trade_output = exchange.create_order(
    symbol=SYMBOL, type="limit", price=PRICE, side="sell", amount=VOLUME
)
```
#### LIMIT Close position Futures with ccxt

```python
exchange.create_order(
    symbol=SYMBOL,
    type="TAKE_PROFIT",
    side="sell",  # buy/sell
    price=TP_PRICE,
    amount=VOLUME,
    params={"stopPrice": STOP_PRICE_VAL, "reduceOnly": True},
)
```
## API call from config file / Future market / Binance Hedge mode / Binance Set Leverage
#### settings.cfg file template
```cfg
[BINANCE]
key = XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
secret = XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

[FTX]
key = XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
secret = XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

[DEFAULTEXCHANGE]
fav_exchange=BINANCE
```
#### Python code
```python
import ccxt
import configparser
config = configparser.RawConfigParser()
config.read("settings.cfg")

exchange_name = config["DEFAULTEXCHANGE"]["fav_exchange"]
api_key = config[config["DEFAULTEXCHANGE"]["fav_exchange"]]["key"]
secret_key = config[config["DEFAULTEXCHANGE"]["fav_exchange"]]["secret"]
exchange_class = getattr(ccxt, exchange_name.lower())
exchange = exchange_class(
    {
        "apiKey": api_key,
        "secret": secret_key,
        "timeout": 30000,
        "enableRateLimit": True,
        "options": {
            "defaultType": "future",  # ←-------------- Future mode
            "hedgeMode": False,       # ←-------------- Hedge mode
        },
    }
)

markets = exchange.load_markets()
market = exchange.market(symbol)
exchange.fapiPrivate_post_leverage(
    {
        "symbol": market["id"],
        "leverage": 15,             # ←-------------- Set leverage value here
    }
)
```
## Functions 
### Check if ORDER is OPEN/CLOSE

```python
def verify_open_order(trade_id,symbol):
    orders = exchange.fetch_open_orders(symbol)
    for order in orders:
        if trade_id == order["info"]["orderId"]:
            return True                     # ORDER IS ACTIVE, ORDER_ID MATCHED
        else:
            return False                    # ORDER NOT ACTIVE, NO ORDER MATCH
```
### Check Symbol Price

```python
def check_price(exchange, symbol):
    x = exchange.fetch_ticker(symbol)
    return float(x["info"]["lastPrice"])
```
