# financial-management
纯文本复式记账，记录财富

把只记录收支的方法称为**普通记账**（估计是多数人在用的方法）。那么复式记账，除了记录收支，还需记录账户（支付宝、银行卡等）的变动。

**复式记账会记录每笔交易的资金流动，各账户变化「有正有负，正负相等」。这便是复式记账的基本原理，称之为「会计恒等式」**。这种方式能够保证记账准确无误，也能提供更详细的财务分析。

**复式记账是方法论，而 Beancount 则是支持复式记账的工具**

[记账神器 Beancount 教程](https://sspai.com/post/59777)

![20220630123144](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220630123144.png)

![20220630123210](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220630123210.png)

![20220630123224](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/books/20220630123224.png)

## 安装

beancount 是个 Python 项目，安装好 python 后，执行：

```shell
pip install beancount
pip install fava
```

Fava 是关联软件，为 Beancount 提供一个更漂亮的 Web 界面（如图 1/2/3），建议同时安装。

## 账本示例

Beancount 的使用非常简单，概括为两步：

### 第一步：使用文本文件按一定格式记账。

下面是个完整示例，直接保存为 example.bean（Beancount 的文件扩展名为.bean）

```bean
;【一、账本信息】
option "title" "我的账本" ;账本名称
option "operating_currency" "CNY" ;账本主货币

;【二、账户设置】
;1、开设账户
2022-04-16 open Assets:Card:1234 CNY, USD ;尾号1234的银行卡，支持CNY和USD
2022-04-16 open Liabilities:CreditCard:5678 CNY, USD ;双币信用卡
2022-04-16 open Income:Salary CNY ;工资收入
2022-04-16 open Expenses:Tax CNY ;交税
2022-04-16 open Expenses:Traffic:Taxi CNY ;打车消费，只支持CNY
2022-04-16 open Equity:OpenBalance ;用于账户初始化，支持任意货币

;2、账户初始化
2019-08-27 * "" "银行卡，初始余额10000元"
    Assets:Card:1234           10000.00 CNY
    Equity:OpenBalance        -10000.00 CNY

;【三、交易记录】
2022-04-17 * "杭州出租车公司" "打车到公司，银行卡支付"
    Expenses:Traffic:Taxi        200.00 CNY
    Assets:Card:1234            -200.00 CNY

2022-04-18 * "" "餐饮"
    Assets:Card:1234           -1100.00 CNY
    Liabilities:CreditCard:5678 1100.00 CNY
    
2022-04-19 * "XX公司" "工资收入"
    Assets:Card:1234           12000.00 CNY
    Expenses:Tax                1000.00 CNY
    Income:Salary           
```

### 第二步：命令行执行 fava moneybook.bean，看到如下结果：

```shell
fava moneybook.bean                                                                                                                                              
Running Fava on http://localhost:5000
```

