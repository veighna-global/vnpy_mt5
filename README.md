# vn.py框架的MT5交易接口

<p align="center">
  <img src ="https://vnpy.oss-cn-shanghai.aliyuncs.com/vnpy-logo.png"/>
</p>

<p align="center">
    <img src ="https://img.shields.io/badge/version-9.81.1-blueviolet.svg"/>
    <img src ="https://img.shields.io/badge/platform-windows|linux|macos-yellow.svg"/>
    <img src ="https://img.shields.io/badge/python-3.7-blue.svg" />
    <img src ="https://img.shields.io/github/license/vnpy/vnpy.svg?color=orange"/>
</p>

关于使用VeighNa框架进行Crypto交易的话题，新开了一个[Github Discussions论坛](https://github.com/vn-crypto/vnpy_crypto/discussions)，欢迎通过这里来进行讨论交流。

## 说明

基于MetaTrader 5的5.00版本开发的MT5交易接口。

使用时需要注意本接口只支持净仓模式（Netting）。

请在MetaTrader 5完成账户的相应设置后再使用。

## 安装

安装需要基于2.8.0版本以上的[VN Studio](https://www.vnpy.com)。

直接使用pip命令：

```
pip install vnpy_mt5
```

下载解压后在cmd中运行：

```
python setup.py install
```

## 使用

以脚本方式启动（script/run.py）：

```
from vnpy.event import EventEngine
from vnpy.trader.engine import MainEngine
from vnpy.trader.ui import MainWindow, create_qapp

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

## 连接

### Mt5配置

1. 确保已经安装好了MT5客户端，并注册登录了一个模拟或者实盘账户（注意经纪商提供的一定要是Netting模式的账户，Hedging模式的用不了）。

2. 在Github下载vnpy_mt5源代码，进入vnpy_mt5.vnpy_mt5目录，找到其中包含的Experts、Include和Libraries三个文件夹。

3. 从开始菜单栏中找到MetaEditor启动，在左侧【导航器】中找到MQL5文件夹，点击鼠标右键选择【打开文件夹】，将之前解压出来的三个文件夹复制到该目录。

4. 回到MetaEditor，再次右键点击MQL5目录，在弹出的菜单栏中点击【刷新】按钮，然后点击Experts目录左侧的+号按钮，看到vnpy_server.mq5文件，双击vnpy_server.mq5文件打开，点击上图红圈中的绿色播放按钮执行编译操作，此时底部的【错误】信息栏中会输出若干编译信息（注意这里要保证0 errors）。

5. 此时MT5会弹出vnpy_server 1.00的对话框，在弹出的对话框中，首先勾选【依存关系】标签页下的【允许DLL导入】，然后切换到【普通】标签，勾选【允许算法交易】后点击【确定】按钮此时图表的右上角会出现vnpy_server的文字提示（字体非常小），右侧有个小人图标，上面应该有个绿色圆形（表示正在运行中）。

6. 然后点击MT5顶部菜单栏的【工具】->【选项】按钮，打开MT5选项对话框，切换到【EA交易】标签，勾选下面的所有选项，最后一定要记住要点击【确定】按钮保存设置。至此就完成了MT5上的全部配置工作。

### Vnpy配置

启动VN Station后加载MetaTrader 5接口后启动，在弹出的连接对话框中什么都不用修改，直接点击底部【连接】按钮即可（记得先配置连接MT5再启动VN Station）。

注意启动时请勿勾选RPC服务接口、RPC服务模块或Web服务器模块这类需要用到zmq的接口或者模块，会导致zmq报错。


### 注意事项

 - MT5的禁止可以立即成交的限价单委托，以买入为例，挂单价格必须低于ask_price_1，否则会被拒单；
 - 在希望立即成交的情况下，请使用市价单来执行；
 - 针对希望满足条件后立即触发的停止单STOP委托，MT5提供服务端停止单委托（Mt5Gateway已支持），所以在CTA策略中下达的停止单会以服务端停止单的方式发出（而不是使用CTP柜台时的vn.py本地停止单）；