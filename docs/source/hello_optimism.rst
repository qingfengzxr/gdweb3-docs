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

在区块链网络中，我们需要一种特殊的货币来支付我们在网络上的操作费用。这种货币称为 **Gas**。在 **Optimism** 网络中，我们使用 **ETH** 作为 **Gas**。

获取测试ETH的方式，主要依靠水龙头。你可以在Google搜索 **Optimism Sepolia Testnet Faucet**，找到一个可用的水龙头，然后输入您的钱包地址，点击 **Get ETH** 即可获取测试ETH。

不过，今年以来，由于区块链行业的火爆，水龙头的使用频率也越来越高，因此有时候会出现水龙头无法使用的情况，或者添加了更多的验证，尤其是资金验证，使得获取测试代币的过程变得困难。
如果你没有找到合适的方法，可以通过在 **Github** 的 **Issue** 中留言邮箱地址，并备注获取测试代币，我们会尽快为您提供0.5 ETH的测试代币。


2. 正式进入开发
---------------

完成上述的准备工作后，我们就可以开始正式进入开发了。这个教程会带着大家Step by Step的实现一个简单的功能，可以向 **Optimism** 网络发起一笔调用 `sendHello` 合约接口的交易。
然后你的这一次 **Say Hello** 的行为将永久的记录在区块链上。你可以在这次调用中，为使用的账户地址取个名字，合约会记录这个信息。在未来的任何时候，
你使用发起这次交易的账户调用 `whoami` 接口，合约都会返回你的名字。

正如前面所说，我们可以将区块链网络理解为一个服务器。我们的智能合约就是这个服务器上的一个程序，我们可以通过调用这个程序的接口来与这个服务器进行交互。因此，在游戏开发中，我们其实也需要开发两个部分：

前端：我们使用 **Godot** 引擎来支撑游戏交互和游戏逻辑的开发。

智能合约（服务器程序）：我们使用 **Solidity** 语言来编写智能合约，并将其部署到区块链网络上。

2.1. 创建一个新的Godot项目
~~~~~~~~~~~~~~~~~~~~~~~~~~

TODO: @shanghua

2.2. 编写Hello Optimism智能合约
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
在ETH生态中，绝大多数虚拟机都是使用的EVM，Optimism也不例外。编写EVM的智能合约，需要用到Solidity语言（也有很多其他的项目，可以支撑大家使用其他语言来编写EVM的智能合约，这一方面的内容读者如果感兴趣，可以自行探索）。

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

* `callHello` 接口是一个只读接口，不会改变合约的状态，只是返回一个字符串。

* `sendHello` 接口是一个写接口，会改变合约的状态，将调用者的地址和传入的用户名绑定。

* `whoami` 接口是一个只读接口，会返回调用者的用户名。


2.3. 编译&部署 Hello Optimism 智能合约
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

在部署合约的工作上，本教程选用Remix来完成，因为它是一个非常好用的在线IDE，可以帮助大家快速上手。
当然，大家也可以选择其他的开发框架，比如Truffle，Hardhat等，它们往往有更强大的能力，但需要花点时间学习，大家可以自行钻研。在本教程中，暂时不涉及这部分内容。

Remix地址: https://remix.ethereum.org/#lang=en&optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.28+commit.7893614a.js

打开Remix后，我们在左侧的文件夹中新建一个文件，命名为 **HelloWorld.sol** ，然后将上面的合约代码复制到文件中。

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

其中 `current_address` 代表的是当前使用的账户地址，这个地址是通过私钥生成的，我们可以通过这个地址来查询当前账户的用户名。在示例代码中，它是一个全局变量。


2.5. 编写游戏的UI界面
~~~~~~~~~~~~~~~~~~~~~

TODO: @shanghua


2.6. 运行游戏
~~~~~~~~~~~~~~



.. autosummary::
   :toctree: generated