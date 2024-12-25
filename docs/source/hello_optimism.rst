Hello Optimism
==============

对于一个从来没有进行过区块链开发的开发者来说，刚刚接触区块链开发时，可能会觉得是一个复杂的事情。它和学习一门语言不太一样，因为它涉及到了很多概念，比如区块链网络、智能合约、钱包、交易等等。
但是，我们相信，只要您有一颗学习的心，您就可以很快上手。我们为您准备了一个简单的教程，让您可以快速了解区块链的一些基础知识，以及如何使用 GDWeb3 与区块链网络进行交互。希望这个教程能够帮助您快速入门。
我们把这个教程称为 **Hello Optimism** （因为我们使用Optimism网络），就像经典的 **Hello World** 一样。

1. 准备工作
------------

区块链网络是一个由许多P2P节点构成的去中心化网络，每个节点都可以独立运行。这些节点可以通过约定的协议进行通信并达成共识，共同维护一个称为区块链的数据。
如果你不太理解这些技术，也没有关系。将它理解为网络游戏开发中的服务器部分即可。只不过这个服务器是一个完全公开的服务器，上面的内容所有人共同维护，全部可见。
在使用时我们提交一定的费用（Gas），为我们在这个服务器上的每一个操作行为买单。

因此，可以看到，我们需要有两个关键的东西，才能进入开发：

1) 用于测试的区块链网络

2) 用于测试的测试代币

下面我们来准备这两个东西。

1.1. 区块链网络
~~~~~~~~~~~~~~~

在这个教程中，我们使用 **Optimism** 测试网络。Optimism 是一个 ETH Layer 2 解决方案，它的性能非常出色，并且拥有强大的生态系统。
您可以在 `Optimism官方文档 <https://www.optimism.io/>`_ 了解更多关于 Optimism 的信息。

准备 **Optimism** 测试网络的方式通常有三种：

1) 使用官方提供的公共测试网络

2) 搭建本地节点，构建本地测试网络

3) 使用第三方服务商提供的测试网络, 如: QuickNode, Infura, Alchemy 等

这里我们选用第一种方式，使用官方提供的公共测试网络。

为此，您需要一个 **MetaMask** 钱包，并且切换到 **Optimism** 测试网络（注意：metamask并不是必须的，但是它拥有一个友好的用户界面，这有助于大家快速入门和理解各种行为，因此我们使用它）。我们使用浏览器插件版本的 **MetaMask**，教程中浏览器为Google Chrome。

关于metamask的更多信息，请参考: https://metamask.io/faqs/

1.1.1. 安装 MetaMask
~~~~~~~~~~~~~~~~~~~~~

进入谷歌浏览器的应用商店，搜索 **MetaMask**，点击 **Add to Chrome** 安装。注意选择带蓝标的插件，防止被坑。

.. image:: ./_static/metamask00.png
   :alt: MetaMask Installation

1.1.2. 创建钱包
~~~~~~~~~~~~~~~~

安装完成后，请按照 MetaMask 的指引流程创建一个新的钱包。在 `Secure your wallet` 步骤时，请务必备份好您的助记词（12 个单词），这是找回钱包的唯一方式。
在生产环境中，这一点尤为重要。每个区块链用户都必须意识到，区块链网络是去中心化的，没有中心化的管理机构，一旦丢失助记词，您的资产将无法找回。
开发者更应有此意识，并以保护用户资产安全为首要原则。

.. image:: ./_static/metamask01.png
   :alt: Secure your wallet

.. image:: ./_static/metamask02.png
   :alt:

.. image:: ./_static/metamask03.png
   :alt:

1.1.3 切换为Optimism测试网络
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

切换网络的方式有两种：

1）在metamask中，点击 **Ethereum Mainnet**，选择 **Add a custom network**，填入 **Optimism** 测试网络的信息。

2）利用OP浏览器的网站的工具，一键添加。

我们选择第二种方式，因为这种方式更加简单。

进入网站：`OP Sepolia 浏览器 <https://sepolia-optimism.etherscan.io/>`_，下拉到页面底部，点击 **Add Optimism Sepolia Network** 即可添加。

.. image:: ./_static/optestnet00.png
   :alt:

在添加过程中，会唤醒 **MetaMask** 插件，进行添加交互。

.. image:: ./_static/optestnet01.jpg
   :alt:

添加完成后，就可以在metamask中看到 **Op Sepolia** 的测试网络了。

.. image:: ./_static/optestnet02.jpg
   :alt:

1.2 获取测试代币
~~~~~~~~~~~~~~~~

在区块链网络中，我们需要一种特殊的货币来支付网络上的操作。这种货币被称为 **Gas**。在 **Optimism** 网络中，我们使用 **ETH** 作为 **Gas**。

获取测试ETH的主要方式是通过水龙头。您可以在Google中搜索 **Optimism Sepolia Testnet Faucet**，找到一个可用的水龙头，输入您的钱包地址，然后点击 **Get ETH** 来获取测试ETH。

然而，由于今年区块链行业的火热，水龙头使用频率增加，有时会导致无法使用或需要额外的验证要求，特别是财务验证，这使得获取测试代币变得更加困难。
如果您找不到合适的方法，可以在 **Github** **Issue** 中留下您的邮箱地址和 optimism sepolia 测试网地址，并说明您需要测试代币 - 我们会尽快为您提供 0.5 ETH 测试代币。


2. 开始开发
-----------

完成上述准备工作后，我们就可以开始开发了。本教程将一步步指导您实现一个简单的功能，该功能可以向 **Optimism** 网络发送交易来调用 `sendHello` 合约接口。
然后您的 **Say Hello** 操作将被永久记录在区块链上。您可以为这笔交易中使用的账户地址命名。当您将来使用该账户地址调用 `whoami` 接口时，合约将返回您的名字。

如前所述，我们可以将区块链网络视为一个服务器。我们的智能合约是这个服务器上的程序，我们可以通过这个程序的接口与服务器进行交互。因此，在游戏开发中，我们需要开发两个部分：

前端：我们使用 **Godot** 引擎来支持游戏交互和游戏逻辑的开发。

智能合约（服务器程序）：我们使用 **Solidity** 语言编写智能合约并将其部署到区块链网络。

2.1. 创建新的Godot项目
~~~~~~~~~~~~~~~~~~~~~~~~

我们从创建一个新的Godot项目开始。在Godot项目管理器中，点击左上角的 **+Create** 按钮来创建一个新的Godot项目。

.. image:: ./_static/setting-up-project-01.png
   :alt:

您可以在面板中指定您喜欢的项目名称和项目路径。通过勾选 **Create Folder**，项目将被放置在 `/path/project_name` 中。如果不勾选，项目将直接创建在 `/path` 中。点击左下角的 `Create & Edit` 按钮，您就可以进入并编辑您新创建的Godot项目。

.. image:: ./_static/setting-up-project-02.png
   :alt:

您可以在官方文档中找到更多关于设置新项目的详细信息：https://docs.godotengine.org/en/stable/getting_started/first_2d_game/01.project_setup.html


2.2. 编写Hello Optimism智能合约
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

在ETH生态系统中，大多数虚拟机都使用EVM，Optimism也不例外。编写EVM智能合约需要使用Solidity语言（也有其他项目可以支持使用其他语言编写EVM智能合约，感兴趣的读者可以自行探索这方面的内容）。

下面是一个简单的 **HelloWorld** 智能合约。这个合约有三个接口：

.. code-block:: solidity

   // SPDX-License-Identifier: MIT

   pragma solidity ^0.8.2;

   contract HelloWorld {
      mapping(address => string) public users;

      function callHello() public pure returns (string memory) {
         return "Hello, Optimism!";
      }

      function sendHello(string memory _username) public returns (string memory) {
         users[msg.sender] = _username;
         return string(abi.encodePacked("Hello, ", _username, "!"));
      }

      function whoami() public view returns (string memory) {
         return string(abi.encodePacked("Hello, ", users[msg.sender], "!"));
      }
   }

其中：

* `callHello` 是一个只读接口，不会改变合约的状态，只是返回一个字符串。

* `sendHello` 是一个写入接口，会改变合约的状态，并绑定调用者的地址和传入的用户名。

* `whoami` 是一个只读接口，返回调用者的用户名。


2.3. Compiling and Deploying Hello Optimism Smart Contracts
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

在合约部署时，这个教程使用 Remix 因为它是一个非常实用的在线IDE，可以帮助快速上手。
当然，您也可以选择其他开发框架，如 Truffle, Hardhat 等，这些框架通常具有更强大的功能，但需要一些时间来学习。您可以在本教程中自行探索这方面的内容。

Remix地址：https://remix.ethereum.org/#lang=en&optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.28+commit.7893614a.js

打开Remix，在左侧文件夹中创建一个新的文件 **HelloWorld.sol**，并将上述合约代码复制到文件中。

.. image:: ./_static/remix00.png
   :alt:

然后我们点击 **Solidity Compiler**，编译合约。

.. image:: ./_static/remix01.jpg
   :alt:

注意：编译后，在图示的3，4处，可以复制后续编写gdscript调用代码所需要的ABI, Bytecode。

接下来，我们点击 **Deploy & Run Transactions** 来进行合约部署。部署合约时，有一些选项，其中 **Environment** 选项，可以选择部署环境。选择 `Remix VM(Cancun)` 会将合约部署在一个remix构建的本地环境上。
这里我们选择 **Injected Provider - Metamask** 选项，这样我们可以使用metamask来部署合约。合约最终会被部署到当前metamask配置的网络上。

.. image:: ./_static/remix03.jpg
   :alt:

部署完成后，我们可以在 **Deployed Contractd** 一栏，看到我们部署的合约，以及合约的地址。并可以使用其提供的交互界面，和合约进行交互。在编写本教程时，我们的合约被部署到了：

   `0x71b215024ed4d2603b654379809feabf726c66f0`

可以在OP浏览器上查看该合约的信息: https://sepolia-optimism.etherscan.io/address/0x71b215024ed4d2603b654379809feabf726c66f0



2.4. 使用GDScript调用Hello Optimism智能合约
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

接下来，我们使用GDScript来编写调用我们部署的合约的代码。需要用到集成了GDWeb3模块编译出的Godot引擎可执行程序。

在开始部署之前，我们需要准备以下四个东西：

1. **合约地址**：在前面部署合约时，我们得到了合约地址，这个地址是合约在区块链网络上的唯一标识。

2. **合约ABI**：在前面编译合约时，有提到如何获取ABI，这个ABI是一个json格式的数据，描述了合约的接口。

3. **节点RPC请求地址**：在使用GDWeb3模块时，我们需要一个节点RPC请求地址，这个地址是一个可以访问到区块链网络的节点地址。大家可以在QuickNode上快速创建一个Endpoints，然后获取这个地址。
教程中用到的RPC URL是：https://snowy-capable-wave.optimism-sepolia.quiknode.pro/360d0830d495913ed76393730e16efb929d0f652

也可以直接用教程中的这个地址，不过不保证长期可用。

4. **私钥**: 私钥可以通过metamask导出当前账户的私钥来获取。切记不要向其他人泄露你的私钥，这是非常危险的行为，获取了私钥就获取了账户的控制权。


接下来，我们在GDScript中定义它们。

.. code-block:: gdscript

   const CONTRACT_ADDRESS := "0x71b215024ed4d2603b654379809feabf726c66f0"
   const CONTRACT_ABI := """
   [{"inputs":[{"internalType":"string","name":"_username","type":"string"}],"name":"sendHello","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"nonpayable","type":"function"},{"inputs":[],"name":"callHello","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"pure","type":"function"},{"inputs":[{"internalType":"address","name":"","type":"address"}],"name":"users","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"whoami","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"view","type":"function"}]
   """
   const NODE_RPC_URL := "https://snowy-capable-wave.optimism-sepolia.quiknode.pro/360d0830d495913ed76393730e16efb929d0f652"

现在，我们来编写调用 `sendHello` 合约接口的代码：

.. code-block:: gdscript

   func send_hello(username, prikey):
    # create a new instance of the ABIHelper class and unmarshal the ABI JSON string into it
    var h = ABIHelper.new()
    var res = h.unmarshal_from_json(CONTRACT_ABI)
    if !res:
        print("unmarshal_from_json failed!")
        return

    var params = [
        username,
    ]
    var packed = h.pack("sendHello", params)

    # get Optimism instance and set rpc url
    var op = Optimism.new()
    op.set_rpc_url(NODE_RPC_URL)
    var ethaccount_manager = EthAccountManager.new()
    var ethaccount = ethaccount_manager.from_private_key(prikey.hex_decode())
    #print("send eth account: %s" % [ethaccount.get_hex_address()])
    current_address = ethaccount.get_hex_address()
    op.set_eth_account(ethaccount)
    var transaction = {
        "to": CONTRACT_ADDRESS,
        "data": packed,
    }
    var signed_tx_data = op.sign_transaction(transaction)
    var rpc_result = op.send_transaction(signed_tx_data)
    print("rpc_result: ", rpc_result)
    # example rpc_result:  { "success": true, "errmsg": "", "txhash": "0xe3b18398db6371a47c1795f4a09ab412ddeceaa29ffda3d5dbae514a99e6caed" }
    if rpc_result["success"] == false:
        print("rpc reqeust failed! errmsg: ", rpc_result["errmsg"])
        return
    var tx_hash = rpc_result["txhash"]
    print("tx_hash: ", tx_hash)
    return tx_hash

对于 `send_hello` 来说，我们使用ABIHelper类来解析合约的ABI，然后调用 `pack` 方法打包调用参数，最后使用私钥来签名一笔交易，然后调用 `send_transaction` 方法来将交易发送的区块链网络上。

对于这种会修改合约状态的交易，我们需要支付一定的Gas费用。这个Gas费用会被区块链网络的矿工收取，用于维护网络的运行。同时，合约的执行结果也不会同步返回，它返回的是交易哈希，我们可以通过这个哈希来查询交易的执行结果。

接下来，我们来编写调用 `whoami` 合约接口的代码，它可以简洁的查询 `sendHello` 的执行结果，即将当前地址的用户名返回。

.. code-block:: gdscript

   func whoami():
      # create a new instance of the ABIHelper class and unmarshal the ABI JSON string into it
      var h = ABIHelper.new()
      var res = h.unmarshal_from_json(CONTRACT_ABI)
      if !res:
         print("unmarshal_from_json failed!")
         return []

      var packed = h.pack("whoami", [])

      var op = Optimism.new()
      op.set_rpc_url(NODE_RPC_URL)
      var call_msg = {
         "from": current_address,
         "to": CONTRACT_ADDRESS,
         "input": "0x" + packed.hex_encode(),
      }
      var rpc_resp = op.call_contract(call_msg, "")
      print("gd: origin rpc_result: ", rpc_resp)
      print("gd: rpc_result: ", rpc_resp["response_body"])

      var call_result = JSON.parse_string(rpc_resp["response_body"])
      print("!!! result: %s" % [call_result])

      # create a new instance of the ABIHelper class and unmarshal the ABI JSON string into it
      var call_ret = call_result["result"]
      call_ret = call_ret.substr(2, call_ret.length() - 2)
      var result = []
      var err = h.unpack_into_array("callHello", call_ret.hex_decode(), result)
      if err != OK:
         assert(false, "unpack_into_dictionary failed!")
      print("call result: ", result)
      return result

Among them, `current_address` represents the account address currently used. This address is generated from the private key and can be used to query the username of the current account. In the example code, it's a global variable.


2.5. 编写游戏界面
~~~~~~~~~~~~~~~~

我们准备了一个小型演示来可视化上述功能：https://github.com/qingfengzxr/HelloOptimism

Godot拥有一个非常强大且易用的UI系统，让你能够快速优雅地构建游戏界面。

这个演示包含了一个用于处理`username`输入的**LineEdit**UI组件，一个用于调用`callHello`和`sendHello`函数的**Button**UI组件，以及一个用于显示链上响应的**Label**UI组件。如果`username`为空，点击**Button**将调用合约中的`callHello`函数，并从合约返回默认的`Hello, Optimism!`。如果设置了`username`，点击**Button**将首先调用合约中的`sendHello`函数，交易内容包含你的`username`和`privateKey`。然后它会调用whoami函数，使用从`privateKey`转换得到的地址从合约中获取之前发送的用户名。

.. image:: ./_static/gui-01.png
   :alt:

默认情况下，编辑器窗口主要包含5个部分。当前打开节点的场景树(红框)、用于文件管理的文件浏览器(绿框)、用于管理当前节点层次结构和GDScript编码的场景和脚本编辑器(黄框)、当前打开节点的检查器(蓝框)以及调试控制台(灰框)。

.. image:: ./_static/gui-02.png
   :alt:

这个演示只包含一个作为主UI场景的节点和两个gdscript文件。`main.gd`用于控制实际的UI，而`hello_optimism.gd`作为简单合约的静态API被自动加载。

自动加载的脚本可以在**Project->Project Settings->Globals->Autoload**中管理。一旦脚本被设置为自动加载，就可以在不附加到实例化节点的情况下调用它。

.. image:: ./_static/gui-04.png
   :alt:

你可以在附加了main.gd脚本的主节点的检查器中通过`Private Key`变量设置你的私钥。通过在变量声明前添加`@export`装饰器，可以使变量在检查器中可访问。

.. code-block:: gdscript

   @export var private_key: String = ""

   func _on_button_pressed() -> void:
      pass

.. image:: ./_static/gui-03.png
   :alt:

**Button**提交事件通过将`pressed`事件信号连接到`_on_button_pressed`函数传递给`main.gd`。

首先在主节点层次结构中选择**Button**节点

.. image:: ./_static/gui-07.png
   :alt:

然后在检查器中，切换到**Node**标签页来管理当前**Button**节点的信号。双击`pressed`信号。

.. image:: ./_static/gui-05.png
   :alt:

在弹出的面板中，选择包含目标函数的节点来连接信号。只要有有效的函数签名，你可以选择父层次结构中任何节点中的任何函数作为槽。

.. image:: ./_static/gui-06.png
   :alt:

2.6. 在_on_button_pressed中编写相关逻辑
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: gdscript

   func _on_button_pressed() -> void:
      var your_name = line_edit.text.strip_edges()
      print("the input name is: %s" % [your_name])
      if your_name.length() > 0:
         if private_key.length() > 0:
               var address = HelloOptimism.send_hello(your_name, private_key)
               print("return address is: %s" % [address])
               var send_hello_result = HelloOptimism.whoami()
               show_hello.text = send_hello_result[0]
         else:
               show_hello.text = "Please set privatekey first"
      else:
         var call_hello_result = HelloOptimism.callHello()
         print("call hello result: %s" % [call_hello_result])
         if call_hello_result.size() > 0:
               show_hello.text = call_hello_result[0]
         else:
               show_hello.text = "Something went wrong, please check console"

2.7. 运行游戏
~~~~~~~~~~~~~
点击绿色的三角形，开始运行游戏。


.. autosummary::
   :toctree: generated