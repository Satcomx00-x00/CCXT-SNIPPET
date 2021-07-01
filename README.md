# CCXT-SNIPPET

## MARKET Close position Futures with ccxt 
```python
close_sell_position = exchange.create_order(
        symbol=SYMBOL,
        type="MARKET",
        side="buy",       # buy//sell
        amount=VOLUME,
        params={"reduceOnly": "true"},
    )
```
```python

```

## LIMIT Close position Futures with ccxt
```python
trade_output = exchange.create_order(
        symbol=symbol, type="limit", price=price, side="sell", amount=volume
    )
```
## LIMIT Close position Binance Futures with ccxt

```python
exchange.create_order(
                symbol=SYMBOL,
                type="TAKE_PROFIT",
                side="sell",      #buy/sell
                price=TP_PRICE,
                amount=VOLUME,
                params={"stopPrice": STOP_PRICE_VAL, "reduceOnly": True},
            )
```
