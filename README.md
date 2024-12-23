Pyth Client in Python
=====================

[![pytest](https://github.com/pyth-network/pyth-client-py/actions/workflows/pytest.yml/badge.svg?branch=main)](https://github.com/pyth-network/pyth-client-py/actions/workflows/pytest.yml)

A Python library to retrieve data from Pyth account structures off the Solana blockchain.

**NOTE**: This library requires Python 3.7 or greater due to `from __future__ import annotations`.

Usage
--------------

Install the library:

    pip install pythclient

You can then read the current Pyth price using the following:

```python
from pythclient.pythclient import PythClient
from pythclient.pythaccounts import DEFAULT_MAPPING_KEY
from pythclient.utils import DEFAULT_PROGRAM_KEY

async def fetch_pyth_prices(network="devnet"):
    from pythclient.pythclient import PythClient
    from pythclient.utils import DEFAULT_MAPPING_KEY, DEFAULT_PROGRAM_KEY

    print("Connecting to Pyth Network...")
    async with PythClient(first_mapping_account_key=DEFAULT_MAPPING_KEY, program_key=DEFAULT_PROGRAM_KEY) as client:
        await client.refresh_all_prices()
        products = await client.get_products()
        for product in products:
            print(f"Product: {product.attrs.get('symbol', 'Unknown')}")
            prices = await product.get_prices()
            for _, price in prices.items():
                print(
                    f"Price: {price.aggregate_price} ± {price.aggregate_price_confidence_interval}"
                )


```

This code snippet lists the products on pyth and the price for each product. Sample output is:

```
{'symbol': 'Crypto.ETH/USD', 'asset_type': 'Crypto', 'quote_currency': 'USD', 'description': 'ETH/USD', 'generic_symbol': 'ETHUSD', 'base': 'ETH'}
PythPriceType.PRICE PythPriceStatus.TRADING 4390.286 p/m 2.4331
{'symbol': 'Crypto.SOL/USD', 'asset_type': 'Crypto', 'quote_currency': 'USD', 'description': 'SOL/USD', 'generic_symbol': 'SOLUSD', 'base': 'SOL'}
PythPriceType.PRICE PythPriceStatus.TRADING 192.27550000000002 p/m 0.0485
{'symbol': 'Crypto.SRM/USD', 'asset_type': 'Crypto', 'quote_currency': 'USD', 'description': 'SRM/USD', 'generic_symbol': 'SRMUSD', 'base': 'SRM'}
PythPriceType.PRICE PythPriceStatus.UNKNOWN None p/m None
...
```

The `examples` directory includes some example applications that demonstrate how to use this library:
* `examples/dump.py` is a detailed usage example that demonstrates the asynchronous API to update prices using a websocket subscription or account polling.
* `examples/read_one_price_feed.py` shows how to read the price of a single price feed using its account key.

Developer Setup
---------------

Using python 3.7 or newer, create, and activate a virtual environment:

    python3 -m venv ve
    . ve/bin/activate

To install this library in editable mode with test dependencies:

    pip install -e '.[testing]'

To run the unit tests:

    pytest

If html based test coverage is more your jam:

    pytest --cov-report=html

The coverage webpages will be in the `htmlcov` directory.


Distribution
------------

To build the package for distribution to pypi, first install dependencies:

`python3 -m pip install --upgrade twine build`

Next, edit `setup.py` and bump the `version` field.
Then build the package by running:

```
python3 -m build
```

This command will produce packages in the `dist/` directory.
Upload these packages to pypi by running:

```
python3 -m twine upload --repository pypi dist/*
```

This command will require you to log in to a pypi account.
