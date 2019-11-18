================================
使用Python封装binanceAPI
================================

Note
----
有任何开发、量化需求，请联系976308589@qq.com

I am working on a python3 version with async support to remove legacy dependencies and their related issues.

I would appreciate if you could try out the `feature/asyncio <https://github.com/sammchardy/python-binance/tree/feature/asyncio>`_ branch and give your feedback.


.. image:: https://img.shields.io/pypi/v/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

.. image:: https://img.shields.io/pypi/l/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

.. image:: https://img.shields.io/travis/sammchardy/python-binance.svg
    :target: https://travis-ci.org/sammchardy/python-binance

.. image:: https://img.shields.io/coveralls/sammchardy/python-binance.svg
    :target: https://coveralls.io/github/sammchardy/python-binance

.. image:: https://img.shields.io/pypi/wheel/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

.. image:: https://img.shields.io/pypi/pyversions/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

Make sure you update often and check the `Changelog <https://python-binance.readthedocs.io/en/latest/changelog.html>`_ for new features and bug fixes.

Features
--------

- Implementation of all General, Market Data and Account endpoints.
- Simple handling of authentication
- No need to generate timestamps yourself, the wrapper does it for you
- Response exception handling
- Websocket handling with reconnection and multiplexed connections
- Symbol Depth Cache
- Historical Kline/Candle fetching function
- Withdraw functionality
- Deposit addresses
- Margin Trading

Quick Start
-----------

`Register an account with Binance <https://www.binance.com/register.html?ref=10099792>`_.

`Generate an API Key <https://www.binance.com/userCenter/createApi.html>`_ and assign relevant permissions.

.. code:: bash

    复制binance文件夹下的内容即可


.. code:: python

    from binance.client import Client
    client = Client(api_key, api_secret)

    # get market depth
    depth = client.get_order_book(symbol='BNBBTC')

    # place a test market buy order, to place an actual order use the create_order function
    order = client.create_test_order(
        symbol='BNBBTC',
        side=Client.SIDE_BUY,
        type=Client.ORDER_TYPE_MARKET,
        quantity=100)

    # get all symbol prices
    prices = client.get_all_tickers()

    # withdraw 100 ETH
    # check docs for assumptions around withdrawals
    from binance.exceptions import BinanceAPIException, BinanceWithdrawException
    try:
        result = client.withdraw(
            asset='ETH',
            address='<eth_address>',
            amount=100)
    except BinanceAPIException as e:
        print(e)
    except BinanceWithdrawException as e:
        print(e)
    else:
        print("Success")

    # fetch list of withdrawals
    withdraws = client.get_withdraw_history()

    # fetch list of ETH withdrawals
    eth_withdraws = client.get_withdraw_history(asset='ETH')

    # get a deposit address for BTC
    address = client.get_deposit_address(asset='BTC')

    # start aggregated trade websocket for BNBBTC
    def process_message(msg):
        print("message type: {}".format(msg['e']))
        print(msg)
        # do something

    from binance.websockets import BinanceSocketManager
    bm = BinanceSocketManager(client)
    bm.start_aggtrade_socket('BNBBTC', process_message)
    bm.start()

    # get historical kline data from any date range

    # fetch 1 minute klines for the last day up until now
    klines = client.get_historical_klines("BNBBTC", Client.KLINE_INTERVAL_1MINUTE, "1 day ago UTC")

    # fetch 30 minute klines for the last month of 2017
    klines = client.get_historical_klines("ETHBTC", Client.KLINE_INTERVAL_30MINUTE, "1 Dec, 2017", "1 Jan, 2018")

    # fetch weekly klines since it listed
    klines = client.get_historical_klines("NEOBTC", Client.KLINE_INTERVAL_1WEEK, "1 Jan, 2017")

![Input Image](./data-images/image2.jpg)
![Output Image](./data-images/image2new.jpg)
Donate
------
If this library helped you out feel free to donate.
<div id="Donate"></div>


