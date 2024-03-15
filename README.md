# MT5 trading gateway for VeighNa Evo

<p align="center">
  <img src ="https://github.com/veighna-global/vnpy_evo/blob/dev/logo.png" width="300" height="300"/>
</p>

<p align="center">
    <img src ="https://img.shields.io/badge/version-2024.3.15-blueviolet.svg"/>
    <img src ="https://img.shields.io/badge/platform-windows-yellow.svg"/>
    <img src ="https://img.shields.io/badge/python-3.10|3.11|3.12-blue.svg"/>
    <img src ="https://img.shields.io/github/license/veighna-global/vnpy_evo.svg?color=orange"/>
</p>



## Introduction

This gateway is developed based on MT5 ZeroMQ connection and supports all MT5 trading.

**Please notice: only supports netting position mode.**

## Install

Users can easily install ``vnpy_mt5`` by pip according to the following command.

```
pip install vnpy_mt5
```

Also, users can install ``vnpy_mt5`` using the source code. Clone the repository and install as follows:

```
git clone https://github.com/veighna-global/vnpy_mt5.git && cd vnpy_mt5

python setup.py install
```

## A Simple Example

Save this as run.py.

```
from vnpy_evo.event import EventEngine
from vnpy_evo.trader.engine import MainEngine
from vnpy_evo.trader.ui import MainWindow, create_qapp

from vnpy_mt5 import Mt5Gateway


def main():
    """主入口函数"""
    qapp = create_qapp()

    event_engine = EventEngine()
    main_engine = MainEngine(event_engine)
    main_engine.add_gateway(Mt5Gateway)

    main_window = MainWindow(main_engine, event_engine)
    main_window.showMaximized()

    qapp.exec()


if __name__ == "__main__":
    main()
```


## MT5 Configuration

1. Ensure that the MT5 client is installed and logged in with either a demo or a real account (note that the account provided by the broker must be in Netting mode, Hedging mode will not work).

2. Download the vnpy_mt5 source code from GitHub, enter the vnpy_mt5.vnpy_mt5 directory, and find the Experts, Include, and Libraries folders included there.

3. From the start menu, launch MetaEditor. In the left side "Navigator", find the MQL5 folder, right-click it and choose "Open Folder". Copy the three folders you previously unzipped into this directory.

4. Return to MetaEditor, right-click the MQL5 directory again, and click the "Refresh" button from the pop-up menu. Then click the + button next to the Experts directory, find the vnpy_server.mq5 file, double-click to open it, and click the green play button in the top red circle to execute the compilation. The bottom "Errors" information bar will display several compilation messages (ensure there are 0 errors).

5. MT5 will pop up a dialog for vnpy_server 1.00. In the dialog, first check the "Allow DLL imports" option under the "Dependencies" tab, then switch to the "General" tab, check the "Allow Algorithmic Trading" option, and click the "OK" button. A text prompt for vnpy_server (with very small font) will then appear at the top right corner of the chart, with a small figure icon on the right, which should have a green circle (indicating it is running).

6. Next, click the "Tools" -> "Options" button on the top menu bar of MT5, open the MT5 Options dialog, switch to the "Expert Advisors" tab, and check all the options below. Finally, remember to click the "OK" button to save the settings. This completes all the configuration work on MT5.


## Notes

1. MT5 prohibits marketable limit orders. For example, when placing a buy order, the order price must be lower than ask_price_1, otherwise, the order will be rejected.
2. Use market orders for transactions you wish to execute immediately.
3. For stop orders that you wish to trigger immediately upon conditions being met, MT5 offers server-side stop order execution (supported by Mt5Gateway). Thus, stop orders placed in the CTA strategy will be issued as server-side stop orders.
4. Be careful not to load these app modules including RpcService, RpcGateway, WebService at startup, which require the use of ZeroMQ, as this will cause ZeroMQ to report errors.
