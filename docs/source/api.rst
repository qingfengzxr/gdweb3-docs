API 参考
========

Web3
----

Web3 类是所有区块链网络的抽象。您可以使用此类获取与不同网络交互的实例。

示例
~~~~
.. code-block:: gdscript

    var web3 = Web3.new()
    var op = web3.get_op_instance()



Optimism
--------
Optimism 类用于与 OP 网络交互。它封装了与 OP 网络交互所需的 JSON-RPC 接口和核心协议。

示例
~~~~
.. code-block:: gdscript

    const NODE_RPC_URL := "https://snowy-capable-wave.optimism-sepolia.quiknode.pro/360d0830d495913ed76393730e16efb929d0f652"
    var op = Optimism.new()

    # 设置节点的 RPC URL
    op.set_rpc_url(NODE_RPC_URL)
    var call_msg = {
        "from": "0x0000000000000000000000000000000000000000",
        "to": CONTRACT_ADDRESS,
        "input": "0x" + packed.hex_encode(),
    }
    # 请求智能合约的方法。
    # 构造 packed 变量的过程在此不展开。
    # 请参阅 ABIHelper 文档中的构造过程部分。
    var rpc_resp = op.call_contract(call_msg, "")

如何获取节点的 RPC URL
~~~~~~~~~~~~~~~~~~~~~~~
TODO

RPC methods
~~~~~~~~~~~
1. chain_id
^^^^^^^^^^^
2. call_contract
^^^^^^^^^^^^^^^^
3. get_block_number
^^^^^^^^^^^^^^^^^^^
4. ...todo
^^^^^^^^^^^


LegacyTx
--------
在与区块链网络交互时，我们通常需要向其发送交易。
Legacy 是 ETH 协议中定义的基本交易类型。
LegacyTx 类是一个支持此交易类型的包装器。
它包括创建交易所需的变量和方法。

示例
~~~~
以下示例演示了如何构造 LegacyTx 对象并设置其各种属性。

.. code-block:: gdscript

    # 创建一个 legacyTx
    var legacyTx = LegacyTx.new()
    legacyTx.set_nonce(get_nonce)
    var gas_price = op.suggest_gas_price()
    legacyTx.set_gas_price(gas_price)
    legacyTx.set_gas_limit(828516)
    var value = BigInt.new()
    value.from_string("0")
    legacyTx.set_value(value)
    legacyTx.set_data(packed)

    var chain_id = BigInt.new()
    chain_id.from_string("11155420")
    legacyTx.set_chain_id(chain_id)
    legacyTx.set_to_address(CONTRACT_ADDRESS)

方法
~~~~
1. set_chain_id
^^^^^^^^^^^^^^^
设置交易的链 ID。

.. code-block:: gdscript

    # chain_id: BigInt
    # return: void
    legacyTx.set_chain_id(chain_id)

参数:
    chain_id: BigInt

返回:
    void

2. set_nonce
^^^^^^^^^^^^
设置交易的 nonce。

todo: 解释什么是 nonce。

.. code-block:: gdscript

    # nonce: int
    # return: void
    legacyTx.set_nonce(nonce)

参数:
    nonce: int

返回:
    void

3. set_gas_price
^^^^^^^^^^^^^^^^
设置交易的 gas 价格。

.. code-block:: gdscript

    # gas_price: BigInt
    # return: void
    legacyTx.set_gas_price(gas_price)

参数:
    gas_price: BigInt

返回:
    void

4. set_gas_limit
^^^^^^^^^^^^^^^^
设置交易的 gas 限制。

.. code-block:: gdscript

    # gas_limit: int
    # return: void
    legacyTx.set_gas_limit(gas_limit)

参数:
    gas_limit: int

返回:
    void

5. set_to_address
^^^^^^^^^^^^^^^^^
设置交易的目标地址。

.. code-block:: gdscript

    legacyTx.set_to_address("0xE85f5c8053C1fcdf2b7b517D0DC7C3cb36c81ABF")

参数:
    address: string; 以 0x 开头的 ETH 地址

返回:
    void

6. set_value
^^^^^^^^^^^^
设置交易的值。

.. code-block:: gdscript

    # value: BigInt
    # return: void
    legacyTx.set_value(value)

参数:
    value: BigInt

返回:
    void

7. set_data
^^^^^^^^^^^
设置要发送到区块链网络的数据，例如调用智能合约的 ABI 序列化数据或自定义数据。

.. code-block:: gdscript

    # data: PackedByteArray
    # return: void
    legacyTx.set_data(data)

参数:
    data: PackedByteArray

返回:
    void

8. rlp_hash
^^^^^^^^^^^
获取 LegacyTx 经过 RLP 编码后的 keccak256 哈希值。

.. code-block:: gdscript

    # return: PackedByteArray
    var hash = legacyTx.rlp_hash()

参数:
    无

返回:
    PackedByteArray

9. hash
^^^^^^^
调用此函数以获取交易的哈希值。需要在签名交易后调用。

.. code-block:: gdscript

    # return: PackedByteArray
    var hash = legacyTx.hash()

参数:
    无

返回:
    PackedByteArray

10. sign_tx
^^^^^^^^^^^
使用从私钥构造的签名者对 LegacyTx 进行签名。

.. code-block:: gdscript

    # signer: Ref<Secp256k1Wrapper>
    # return: void
    legacyTx.sign_tx(signer)

参数:
    signer: Ref<Secp256k1Wrapper>

返回:
    int: 成功返回 0，失败返回 -1

11. sign_tx_by_account
^^^^^^^^^^^^^^^^^^^^^^
使用 EthAccount 对 LegacyTx 进行签名。

.. code-block:: gdscript

    # account: Ref<EthAccount>
    # return: void
    legacyTx.sign_tx_by_account(account)

参数:
    account: Ref<EthAccount>

返回:
    int: 成功返回 0，失败返回 -1

12. signedtx_marshal_binary
^^^^^^^^^^^^^^^^^^^^^^^^^^^
将签名后的交易编组为二进制数据并编码为十六进制字符串。

.. code-block:: gdscript

    # return: PackedByteArray
    var res = legacyTx.signedtx_marshal_binary()

参数:
    无

返回:
    String


BigInt
------
BigInt 类实现了一些与大数相关的基本操作，利用 GMP 库来满足其需求。

方法
~~~~~~~
1. add
^^^^^^
将两个 BigInt 对象相加。

.. code-block:: gdscript

    # other: BigInt
    # return: BigInt
    var res = bigInt.add(other)

2. sub
^^^^^^
将两个 BigInt 对象相减。

.. code-block:: gdscript

    # other: BigInt
    # return: BigInt
    var res = bigInt.sub(other)

3. mul
^^^^^^
将两个 BigInt 对象相乘。

.. code-block:: gdscript

    # other: BigInt
    # return: BigInt
    var res = bigInt.mul(other)

4. div
^^^^^^
将两个 BigInt 对象相除。

.. code-block:: gdscript

    # other: BigInt
    # return: BigInt
    var res = bigInt.div(other)

5. mod
^^^^^^
获取两个 BigInt 对象相除的余数。

.. code-block:: gdscript

    # other: BigInt
    # return: BigInt
    var res = bigInt.mod(other)

6. abs
^^^^^^
获取 BigInt 对象的绝对值。

.. code-block:: gdscript

    # return: BigInt
    var res = bigInt.abs()

7. cmp
^^^^^^
比较两个 BigInt 对象。

.. code-block:: gdscript

    # other: BigInt
    # return: int
    var res = bigInt.cmp(other)

返回:
    int: 如果 bigInt > other 返回 1，如果 bigInt == other 返回 0，如果 bigInt < other 返回 -1

8. sgn
^^^^^^
返回 m_number 的符号作为整数：

.. code-block:: gdscript

    # return: int
    var res = bigInt.sgn()

返回:
    int: 如果 bigInt > 0 返回 1，如果 bigInt == 0 返回 0，如果 bigInt < 0 返回 -1

8. is_zero
^^^^^^^^^^
检查 BigInt 对象是否为零。

.. code-block:: gdscript

    # return: bool
    var res = bigInt.is_zero()

9. from_string
^^^^^^^^^^^^^^
从字符串创建一个 BigInt 对象。

.. code-block:: gdscript

    # str: string
    # return: void
    bigInt.from_string(str)

10. from_hex
^^^^^^^^^^^^
从十六进制字符串创建一个 BigInt 对象。

.. code-block:: gdscript

    # str: string
    # return: void
    bigInt.from_hex(str)

11. get_string
^^^^^^^^^^^^^^
获取 BigInt 对象的字符串表示。

.. code-block:: gdscript

    # return: string
    var res = bigInt.get_string()

12. to_hex
^^^^^^^^^^
获取 BigInt 对象的带有 0x 前缀的十六进制字符串表示。

.. code-block:: gdscript

    # return: string
    var res = bigInt.to_hex()


JsonrpcHelper
-------------
.. note::
   TODO


ABIHelper
---------
Ethereum ABI（应用二进制接口）是以太坊智能合约与外部应用程序之间的标准化接口。ABI 定义了如何对智能合约的函数和事件进行编码和解码，从而允许外部应用程序与智能合约进行交互。

有关 ABI 的更多详细信息，您可以参考 `这里 <https://docs.soliditylang.org/en/latest/abi-spec.html>`_。

ABIHelper 类是一个用于处理 ABI 编码和解码的实用类。它提供了用户友好的方法来处理 ABI 编码和解码。

单元测试代码文件：abihelper_unit_test.gd

方法
~~~~~~~
1. unmarshal_from_json
^^^^^^^^^^^^^^^^^^^^^^
解析智能合约的 ABI JSON 字符串到 ABIHelper 对象中。这为 ABIHelper 对象设置了一系列所需的属性，包括函数列表、参数类型等，以便进行 ABI 编码和解码。

.. code-block:: gdscript

    # json: string
    # return: bool
    const CONTRACT_ABI := """
    [{"inputs":[{"components":[{"internalType":"uint32","name":"a","type":"uint32"},{"internalType":"uint256[]","name":"b","type":"uint256[]"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T[]","name":"c","type":"tuple[]"},{"internalType":"int256[3]","name":"d","type":"int256[3]"}],"internalType":"struct Test.S","name":"s","type":"tuple"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T","name":"t","type":"tuple"},{"internalType":"uint256","name":"u","type":"uint256"},{"internalType":"address","name":"user","type":"address"},{"internalType":"bytes10","name":"b10","type":"bytes10"}],"name":"f","outputs":[],"stateMutability":"pure","type":"function"},{"inputs":[],"name":"g","outputs":[{"components":[{"internalType":"uint32","name":"a","type":"uint32"},{"internalType":"uint256[]","name":"b","type":"uint256[]"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T[]","name":"c","type":"tuple[]"},{"internalType":"int256[3]","name":"d","type":"int256[3]"}],"internalType":"struct Test.S","name":"","type":"tuple"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T","name":"","type":"tuple"},{"internalType":"address","name":"","type":"address"},{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"pure","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"}],"name":"h","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"nonpayable","type":"function"}]
    """
    var h = ABIHelper.new()
    var res = h.unmarshal_from_json(CONTRACT_ABI)

参数:
    json: string; ABI JSON 字符串。TODO: 如何获取 ABI 字符串。

返回:
    bool: 成功返回 true，失败返回 false


2. pack
^^^^^^^
根据 ABI 编码规则将参数打包成字节数组。

这里我们需要用两个例子来说明 pack 函数的用法。一个简单的例子用于解释基本用法，另一个复杂的例子用于解释如何处理复杂的数据结构。

**示例 1: ERC20 转账**

对于一个 ERC20 合约，其 transfer 函数的定义如下：

.. code-block:: solidity

    function transfer(address recipient, uint256 amount) public returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

可以看到，transfer 函数接受两个参数：一个类型为 address 的 recipient 和一个类型为 uint256 的 amount。其接口 ABI 定义类似于：

.. code-block:: json

    [
        {
            "constant": false,
            "inputs": [
            {
                "name": "recipient",
                "type": "address"
            },
            {
                "name": "amount",
                "type": "uint256"
            }
            ],
            "name": "transfer",
            "outputs": [
            {
                "name": "",
                "type": "bool"
            }
            ],
            "payable": false,
            "stateMutability": "nonpayable",
            "type": "function"
        }
    ]

那么我们如何调用这个函数呢？以下代码演示了使用 GDScript 调用此函数的过程。

.. code-block:: gdscript

    const CONTRACT_ABI := ... # ERC20 合约的 ABI 定义
    var h = ABIHelper.new()
    var res = h.unmarshal_from_json(CONTRACT_ABI)

    var params = [
        # 接收者
        "0xeB98753449AD50d30561a66CA48BF69EEcaD4bC3",
        # 数量
        "123456"
    ]

    var packed = h.pack("transfer", params)

参数:
    name: string; 方法名称

    params: Array; 调用方法的参数

返回:
    PackedByteArray: 打包后的数据

让我们详细解释这段代码。首先，我们创建一个 ABIHelper 对象 `h`，然后调用 `unmarshal_from_json` 函数，传入 ERC20 合约的 ABI 定义。这使得 `h` 对象能够理解 ERC20 合约的 ABI 定义。

接下来，我们定义一个包含两个元素的 `params` 数组。第一个元素是接收者的地址，第二个元素是数量值。这些对应于 `transfer` 函数的两个参数。

你可以理解为：`params[0] => recipient`，`params[1] => amount`。

因此，你可以很容易地推断出 `params` 数组中元素的顺序必须与合约函数中参数的顺序一致。这非常重要，否则你会得到错误的结果。

最后，我们调用 `pack` 函数，传入函数名称 `transfer` 和参数数组，以获得打包结果。

我们将使用打包结果作为 `LegacyTx` 的数据，并将其发送到区块链以调用 ERC20 合约的 `transfer` 函数。


**示例 2: 复杂结构调用示例**

对于一些复杂的业务场景，人们通常定义自定义数据结构并在调用函数时传递它们，例如数组、结构体、嵌套结构体等。

这里，我们使用一个复杂的嵌套结构体作为示例，说明如何使用 `pack` 函数处理复杂的数据结构。通过这个示例，你将完全理解 `pack` 函数的行为。

现在我们有一个定义了许多复杂数据结构的合约，其中结构体嵌套在其他结构体中。合约定义如下：

.. code-block:: solidity

    // SPDX-License-Identifier: MIT
    pragma solidity ^0.7.6;
    pragma experimental ABIEncoderV2;

    contract Test {
        struct S {
            uint32 a;
            uint[] b;
            T[] c;
            int256[3] d;
        }

        struct C {
            string[] t;
        }

        struct T {
            C[] x;
            bytes10 y;
        }

        mapping (address=>uint256) balance;

        function f(S memory s, T memory t, uint u, address user, bytes10 b10) public pure {
            // This is a pure function, it does not modify the state nor read the state
        }

        function g() public pure returns (S memory, T memory, address, uint) {
            // Initialize S struct
            S memory s;
            s.a = 1;
            s.b = new uint[](2);
            s.b[0] = 2;
            s.b[1] = 3;
            s.c = new T[](1);
            s.c[0].x = new C[](1);
            s.c[0].x[0].t = new string[](2);
            s.c[0].x[0].t[0] = "STRING_TEST"; // Uppercase string
            s.c[0].x[0].t[1] = "string_test"; // Lowercase string
            s.c[0].y = bytes10(0x04000000000000000000); // 4 in bytes10
            s.d[0] = 0x7FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF; // Large number 1
            s.d[1] = 0x7FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFE; // Large number 2
            s.d[2] = ~int256(0x7FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF); // Large number 3 (negative number)

            // Initialize T struct
            T memory t;
            t.x = new C[](1);
            t.x[0].t = new string[](2);
            t.x[0].t[0] = "STRING_test"; // Mixed case string
            t.x[0].t[1] = "STRING_TEST_MORE_THAN_32_BYTES_abcdefghijklmnopqrstuvwxyz_0000000111111222222"; // String longer than 32 bytes
            t.y = bytes10(0x08000000000000000000); // 8 in bytes10

            // Initialize address
            address addr = address(0x8eee12Bd33Ec72a277ffA9ddF246759878589D3b);

            // Initialize uint
            uint u = 9;

            return (s, t, addr, u);
        }

        function h(address user) public returns (uint256) {
            balance[user] += 1;
            return balance[user];
        }
    }

.. note::
    这个 Test 合约也将在介绍 unpack 函数时使用。

我们将使用 f() 函数作为示例来说明如何使用 pack 函数。

首先，我们需要获取合约的 ABI 定义。对于上述合约代码，其 ABI 定义如下：

.. code-block:: gdscript

    const CONTRACT_ABI := """
    [{"inputs":[{"components":[{"internalType":"uint32","name":"a","type":"uint32"},{"internalType":"uint256[]","name":"b","type":"uint256[]"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T[]","name":"c","type":"tuple[]"},{"internalType":"int256[3]","name":"d","type":"int256[3]"}],"internalType":"struct Test.S","name":"s","type":"tuple"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T","name":"t","type":"tuple"},{"internalType":"uint256","name":"u","type":"uint256"},{"internalType":"address","name":"user","type":"address"},{"internalType":"bytes10","name":"b10","type":"bytes10"}],"name":"f","outputs":[],"stateMutability":"pure","type":"function"},{"inputs":[],"name":"g","outputs":[{"components":[{"internalType":"uint32","name":"a","type":"uint32"},{"internalType":"uint256[]","name":"b","type":"uint256[]"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T[]","name":"c","type":"tuple[]"},{"internalType":"int256[3]","name":"d","type":"int256[3]"}],"internalType":"struct Test.S","name":"","type":"tuple"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T","name":"","type":"tuple"},{"internalType":"address","name":"","type":"address"},{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"pure","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"}],"name":"h","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"nonpayable","type":"function"}]
    """

现在，让我们详细解释如何打包此合约的 f() 函数。以下是一个在 GDScript 中的示例代码，演示了如何使用 pack 函数对 f() 函数调用进行编码：

.. code-block:: gdscript

    var h = ABIHelper.new()
    var res = h.unmarshal_from_json(CONTRACT_ABI)

    var params = [
        [
            1, # s.a
            [2, 3], # s.b
            [
                [
                    [
                        [
                            ["STRING_TEST", "string_test"], # s.c[0].x[0].t
                        ], # s.c[0].x[0]
                    ], # s.c[0].x
                    [4, 0, 0, 0, 0, 0, 0, 0, 0, 0], # s.c[0].y
                ], # s.c[0]
            ], # s.c
            [
                # 7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
                "57896044618658097711785492504343953926634992332820282019728792003956564819967", # s.d[0]
                # 7ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffe
                "57896044618658097711785492504343953926634992332820282019728792003956564819966", # s.d[1]
                146 # s.d[2]
            ], # s.d
        ], # s
        [
            [
                [
                    ["STRING_test", "STRING_TEST_MORE_THAN_32_BYTES_abcdefghijklmnopqrstuvwxyz_0000000111111222222"], # t.x[0].t
                ], # t.x[0]
            ], # t.x
            [8, 0, 0, 0, 0, 0, 0, 0, 0, 0], # t.y
        ], # t
        9, # u
        "0x8eee12Bd33Ec72a277ffA9ddF246759878589D3b", # user
        [116, 119, 111, 101, 102, 103, 104, 105, 106, 107] # b10
    ]
    var packed = h.pack("f", params)

我们可以看到 `f()` 函数接受 5 个参数，它们是：

    * s: struct S
    * t: struct T
    * u: uint
    * user: address
    * b10: bytes10

结构体 S 和结构体 T 是包含多个字段的复合结构体，其中嵌套了其他结构体。我们需要按照合约定义的顺序将这些字段填充到 params 数组中。

    * params[0] => s
    * params[1] => t
    * params[2] => u
    * params[3] => user
    * params[4] => b10

在上述代码中，注释用于解释每个字段的值。这应该有助于您清楚地了解如何填充 params 数组。

此外，这个示例合约包含常见的数据类型和一些特殊数据，例如大数、混合大小写字符串和超过 32 字节的字符串。这应该有助于您更好地理解如何使用 pack 函数。

.. note::

    现在，您应该能够推断出以下逻辑关系：

    对于结构体中的字段，我们使用数组来表示它们，其中数组中的每个元素对应结构体中的一个字段。
    数组中元素的顺序必须与结构体中定义的字段顺序一致，否则会导致错误的结果。

3. unpack
^^^^^^^^^
首先，需要注意的是我们并不直接提供 `unpack` 函数。相反，我们提供以下两种方法来实现解包功能：

    * unpack_into_dictionary: 将 ABI 编码的数据解码为字典对象。
    * unpack_into_array: 将 ABI 编码的数据解码为数组。

这两种方法根据 ABI 定义将 ABI 编码的数据解码为特定的数据结构，但提供给开发者操作的结果数据结构不同。

我们将继续使用 `pack` 函数部分介绍的智能合约，但这次我们将使用 `g()` 函数进行说明。

`g()` 函数返回 4 个参数，分别是：

    * 0: struct S
    * 1: struct T
    * 2: address
    * 3: uint

.. note::

    这些参数在 ABI 定义中被定义为匿名的，即名称为空。因此，我们使用数字索引来表示这些参数。

此外，`g()` 函数的返回值设计为包含复杂数据和数据结构，如大数、结构体、数组等。这有助于您更好地理解如何使用解包函数。

.. note::

    对于大数，我们统一使用字符串进行包装，而不是直接使用 BigInt 类。这种方法避免了一些不必要的问题。如果您需要对这些数字进行操作，可以使用 BigInt 类进行处理。

**Example 1: unpack_into_dictionary**

.. code:: gdscript

    var h = ABIHelper.new()
    var res = h.unmarshal_from_json(CONTRACT_ABI)

    var callret = "000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000003600000000000000000000000008eee12bd33ec72a277ffa9ddf246759878589d3b0000000000000000000000000000000000000000000000000000000000000009000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001207fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff7ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffe800000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000300000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000004004000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b535452494e475f54455354000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000b737472696e675f746573740000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000400800000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b535452494e475f74657374000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004d535452494e475f544553545f4d4f52455f5448414e5f33325f42595445535f6162636465666768696a6b6c6d6e6f707172737475767778797a5f3030303030303031313131313132323232323200000000000000000000000000000000000000"
    var result = {}
    var err = h.unpack_into_dictionary("g", callret.hex_decode(), result)
    if err != OK:
        assert(false, "unpack_into_dictionary failed!")
    # example correct output:
    # { "0": { "a": 1, "b": ["2", "3"], "c": [{ "x": [{ "t": ["STRING_TEST", "string_test"] }], "y": [4, 0, 0, 0, 0, 0, 0, 0, 0, 0] }], "d": ["57896044618658097711785492504343953926634992332820282019728792003956564819967", "57896044618658097711785492504343953926634992332820282019728792003956564819966", "-57896044618658097711785492504343953926634992332820282019728792003956564819968"] }, "1": { "x": [{ "t": ["STRING_test", "STRING_TEST_MORE_THAN_32_BYTES_abcdefghijklmnopqrstuvwxyz_0000000111111222222"] }], "y": [8, 0, 0, 0, 0, 0, 0, 0, 0, 0] }, "2": "8eee12bd33ec72a277ffa9ddf246759878589d3b", "3": "9" }

**Example 2: unpack_into_array**

.. code:: gdscript

    var h = ABIHelper.new()
    var res = h.unmarshal_from_json(CONTRACT_ABI)

    var callret = "000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000003600000000000000000000000008eee12bd33ec72a277ffa9ddf246759878589d3b0000000000000000000000000000000000000000000000000000000000000009000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001207fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff7ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffe80000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000030000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000400400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b535452494e475f54455354000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000b737472696e675f7465737400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000400800000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b535452494e475f74657374000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004d535452494e475f544553545f4d4f52455f5448414e5f33325f42595445535f6162636465666768696a6b6c6d6e6f707172737475767778797a5f3030303030303031313131313132323232323200000000000000000000000000000000000000"
    var result = []
    err = h.unpack_into_array("g", callret.hex_decode(), result)
    if err != OK:
        assert(false, "unpack_into_array failed!")

    # example correct output:
    # [[1, ["2", "3"], [[[[["STRING_TEST", "string_test"]]], [4, 0, 0, 0, 0, 0, 0, 0, 0, 0]]], ["57896044618658097711785492504343953926634992332820282019728792003956564819967", "57896044618658097711785492504343953926634992332820282019728792003956564819966", "-57896044618658097711785492504343953926634992332820282019728792003956564819968"]], [[[["STRING_test", "STRING_TEST_MORE_THAN_32_BYTES_abcdefghijklmnopqrstuvwxyz_0000000111111222222"]]], [8, 0, 0, 0, 0, 0, 0, 0, 0, 0]], "8eee12bd33ec72a277ffa9ddf246759878589d3b", "9"]

上面的两个示例对应于使用 `unpack_into_dictionary` 和 `unpack_into_array` 解码 ABI 编码的数据。它们的用法如下：

1. 初始化一个 `ABIHelper` 对象。
2. 调用 `unmarshal_from_json` 函数，传入合约的 ABI 定义。
3. 定义 `callret`。这里直接提供了调用 `g()` 函数后的测试返回值。通常，这个值是通过调用智能合约的函数获得的，通常通过调用 `call_contract()` 方法。
4. 定义一个 `result` 变量来存储解码后的数据。根据选择的函数，`result` 需要是字典对象或数组对象。
5. 调用 `unpack_into_dictionary` 或 `unpack_into_array` 函数，将 `callret` 解码到 `result` 中。

对于 `unpack_into_dictionary`，解码后的值是一个字典，其中键是参数的索引或结构体成员的变量名，值是参数值。

.. note::

    Solidity 允许为函数的返回值命名。如果返回值被命名，返回的字典中的键将是这些名称。如果没有命名，键将是参数的索引。

    function getDetails() public pure returns (uint256 id, string memory name, bool isActive);

对于 `unpack_into_array`，解码后的值是一个数组，其中每个元素对应一个参数值。


EthAccountManager
-----------------
`EthAccountManager` 是一个用于管理以太坊账户的类。它提供了创建新的以太坊账户和从私钥导入账户的功能。

.. warning::
    此类提供的 API 尚未经过审计，可能存在安全漏洞。在生产环境中使用之前，请采取预防措施，正确清理内存，安全存储私钥，并彻底测试交易接收和发送功能！

方法
~~~~~~~
1. create
^^^^^^^^^
创建一个新的以太坊账户。

.. code-block:: gdscript

    # entropy: PackedByteArray (optional)
    # return: EthAccount
    var account_manager = EthAccountManager.new()
    var account = account_manager.create()

参数:
    1. ``entropy``: ``PackedByteArray`` (可选); 用于生成随机私钥的熵值(必须是32字节)。如果为空，则使用内部系统随机数。
返回:
   ``EthAccount``: 创建的以太坊账户对象实例。

.. note::
    为确保生成账户的安全性，尽管内部提供了一定程度的熵值，我们仍建议您提供更高的熵值作为参数。

---------------------------------------------------------------------------------------------------------

2. privateKeyToAccount
^^^^^^^^^^^^^^^^^^^^^^
创建一个基于提供的私钥的以太坊账户。

.. code-block:: gdscript

    # privkey: PackedByteArray
    # return: EthAccount
    var account_manager = EthAccountManager.new()
    var account = account_manager.privateKeyToAccount(private_key)

参数:
    1. ``privkey``: ``PackedByteArray``; 账户的私钥字节。必须是一个有效的以太坊私钥格式。

返回:
    ``EthAccount``: 从私钥创建的以太坊账户实例。

---------------------------------------------------------------------------------------------------------

Example
~~~~~~~
以下示例演示了如何使用 EthAccountManager 创建一个新账户并从私钥导入账户：

.. code-block:: gdscript

    # Create an instance of the account manager
    var account_manager = EthAccountManager.new()

    # Create a new account
    var new_account = account_manager.create(1)
    print("New account address: ", new_account.get_address())

    # Import an account from a private key
    var private_key = PackedByteArray([...]) # 32-byte private key
    var imported_account = account_manager.privateKeyToAccount(private_key)
    print("Imported account address: ", imported_account.get_address())

---------------------------------------------------------------------------------------------------------

EthAccount
----------
`EthAccount` 是一个处理以太坊账户基本操作的类。它提供了访问账户信息和签名数据的功能。通过 `EthAccount`，用户可以获取账户的私钥、公钥和地址，并使用账户的私钥对数据进行签名。

.. note::
    请注意，`EthAccount` 的实例必须通过 `EthAccountManager` 生成，以确保账户的正确初始化和管理。

.. warning::
    此类提供的 API 尚未经过审计，可能存在安全漏洞。在生产环境中使用之前，请采取预防措施，正确清理内存，安全存储私钥，并彻底测试交易接收和发送功能！

方法
~~~~~~~
1. get_private_key
^^^^^^^^^^^^^^^^^^
获取账户的私钥。

.. code-block:: gdscript

    # return: PackedByteArray
    var private_key = account.get_private_key()

返回:
    ``PackedByteArray``: 账户的私钥字节。

---------------------------------------------------------------------------------------------------------

2. get_public_key
^^^^^^^^^^^^^^^^^
获取账户的公钥。

.. code-block:: gdscript

    # return: PackedByteArray
    var public_key = account.get_public_key()

返回:
    ``PackedByteArray``: 账户的公钥字节。

---------------------------------------------------------------------------------------------------------

3. get_address
^^^^^^^^^^^^^^
获取账户的地址（以字节格式）。

.. code-block:: gdscript

    # return: PackedByteArray
    var address = account.get_address()

返回:
    ``PackedByteArray``: 账户的地址字节。

---------------------------------------------------------------------------------------------------------

4. get_hex_address
^^^^^^^^^^^^^^^^^^
获取账户的十六进制地址。

.. code-block:: gdscript

    # return: String
    var hex_address = account.get_hex_address()

返回:
    ``String``: 账户的十六进制字符串地址。

---------------------------------------------------------------------------------------------------------

5. sign_data
^^^^^^^^^^^^
使用账户的私钥对提供的数据进行签名。

.. code-block:: gdscript

    # data: PackedByteArray
    # return: PackedByteArray
    var signature = account.sign_data(data)

参数:
    1. ``data``: ``PackedByteArray``; 要签名的数据。

返回:
    ``PackedByteArray``: 签名后的数据。

---------------------------------------------------------------------------------------------------------

6. sign_data_with_prefix
^^^^^^^^^^^^^^^^^^^^^^^^
对数据进行签名，数据经过 keccak256 哈希计算的 Ethereum 签名消息前缀。

.. code-block:: gdscript

    # data: PackedByteArray
    # return: PackedByteArray
    var signature = account.sign_data_with_prefix(data)

参数:
    1. ``data``: ``PackedByteArray``; 要签名的数据。

返回:
    ``PackedByteArray``: 签名后的数据。

.. note::
    - 此方法遵循 Ethereum 签名消息标准：
    - keccak256("\\x19Ethereum Signed Message:\\n" + len(message) + message)

---------------------------------------------------------------------------------------------------------

Example
~~~~~~~
以下示例演示了如何使用 EthAccount 执行基本操作：

.. code-block:: gdscript

    # Assume we have an account instance
    var account = eth_account_manager.create()

    # Get account information
    print("Address: ", account.get_hex_address())
    print("Public Key: ", account.get_public_key().hex_encode())

    # Sign some data
    var data = "Hello, Ethereum!".to_utf8_buffer()
    var signature = account.sign_data(data)
    print("Signature: ", signature.hex_encode())

    # Sign with Ethereum message prefix
    var prefixed_signature = account.sign_data_with_prefix(data)
    print("Prefixed Signature: ", prefixed_signature.hex_encode())

---------------------------------------------------------------------------------------------------------

EthWalletManager
----------------
`EthWalletManager` 是一个用于管理多个以太坊 HD 钱包的类。它提供了创建、加载和保存钱包的功能。通过此管理器，可以生成新的钱包实例或从助记词恢复现有钱包。

.. warning::
    此类提供的 API 尚未经过审计，可能存在安全漏洞。在生产环境中使用之前，请采取预防措施，正确清理内存，安全存储私钥，并彻底测试交易接收和发送功能！

方法
~~~~~~~
1. create
^^^^^^^^^
创建一个新的以太坊钱包。

.. code-block:: gdscript

    # strength: int (optional)
    # entropy: PackedByteArray (optional)
    # passphrase: String (optional)
    # return: EthWallet
    var wallet = wallet_manager.create()
    var wallet = wallet_manager.create(1, entropy, "my passphrase")

参数:
    1. ``strength``: ``int`` (可选); 创建的账户数量。
    2. ``entropy``: ``PackedByteArray`` (可选); 生成钱包时使用的熵值。
    3. ``passphrase``: ``String`` (可选); 用于增加安全性。

返回:
    ``EthWallet``: 创建的钱包实例。

---------------------------------------------------------------------------------------------------------

2. from_mnemonic
^^^^^^^^^^^^^^^^
从助记词恢复钱包。

.. code-block:: gdscript

    # mnemonic: PackedStringArray
    # passphrase: String (optional)
    # return: EthWallet
    var wallet = wallet_manager.from_mnemonic(mnemonic, "optional passphrase")

参数:
    1. ``mnemonic``: ``PackedStringArray``; 助记词，目前仅支持由 12 个英文单词组成的助记词。
    2. ``passphrase``: ``String`` (可选); 密码。

返回:
    ``EthWallet``: 创建的钱包实例。

---------------------------------------------------------------------------------------------------------

3. load
^^^^^^^
加载钱包。

.. code-block:: gdscript

    # return: EthWallet
    var wallet = wallet_manager.load()

返回:
    ``EthWallet``: 加载的钱包实例。

---------------------------------------------------------------------------------------------------------

4. save
^^^^^^^
保存钱包。

.. code-block:: gdscript

    # hd_wallet: EthWallet
    # return: bool
    var success = wallet_manager.save(wallet)

参数:
    1. ``hd_wallet``: ``EthWallet``; 要保存的钱包。

返回:
    ``bool``: ``true`` 表示成功，``false`` 表示失败

---------------------------------------------------------------------------------------------------------

Example
~~~~~~~
以下是一个完整的示例代码，供参考。

.. code-block:: gdscript

    # Create a wallet manager
    var wallet_manager = EthWalletManager.new()

    # Create a new wallet with 1 account
    var wallet = wallet_manager.create(1)

    # Save the wallet
    wallet_manager.save(wallet)

    # Load the wallet later
    var loaded_wallet = wallet_manager.load()

---------------------------------------------------------------------------------------------------------

EthWallet
---------
`EthWallet` 是一个实现分层确定性（HD）钱包功能的类。它允许用户管理多个以太坊账户，并提供添加、删除账户和处理助记词的功能。

.. warning::
    此类提供的 API 尚未经过审计，可能存在安全漏洞。在生产环境中使用之前，请采取预防措施，正确清理内存，安全存储私钥，并彻底测试交易接收和发送功能！

.. note::
    实例化 `EthWallet` 必须通过 `EthWalletManager` 生成，以确保钱包的正确初始化和管理。

方法
~~~~~~~
1. add
^^^^^^
将一个新的以太坊账户添加到钱包中。

.. code-block:: gdscript

    # privateKey: PackedByteArray (optional)
    # return: bool
    # Create a wallet manager
    var wallet_manager = EthWalletManager.new()

    var wallet = wallet_manager.create()

    var success = wallet.add()   # Generate a new account
    var success = wallet.add(private_key)  # Add an account with an existing private key

参数:
    1. ``privateKey``: ``PackedByteArray`` (可选); 新账户的私钥。如果为空，则生成一个符合 BIP32 的新账户。
返回:
    ``bool``: ``true`` 表示成功，``false`` 表示失败

---------------------------------------------------------------------------------------------------------

2. remove_address
^^^^^^^^^^^^^^^^^
根据地址删除账户。

.. code-block:: gdscript

    # address: PackedByteArray
    # return: bool
    var success = wallet.remove_address(address)

参数:
    1. ``address``: ``PackedByteArray``; 要删除的地址。

返回:
    ``bool``: ``true`` 表示成功，``false`` 表示失败

---------------------------------------------------------------------------------------------------------

3. remove
^^^^^^^^^
根据索引删除账户。
.. code-block:: gdscript

    # index: int
    # return: bool
    var success = wallet.remove(0)  # 删除第一个账户

参数:
    1. ``index``: ``uint64_t``; 要删除的账户的索引。

返回:
    ``bool``: ``true`` 表示成功，``false`` 表示失败

---------------------------------------------------------------------------------------------------------

4. clear
^^^^^^^^
安全地清除所有钱包数据，包括账户和助记词。

.. code-block:: gdscript

    # return: bool
    var success = wallet.clear()

返回:
    ``bool``: ``true`` 表示成功，``false`` 表示失败

---------------------------------------------------------------------------------------------------------

5. get_accounts
^^^^^^^^^^^^^^^
获取钱包中的所有账户。

.. code-block:: gdscript

    # return: Array[EthAccount]
    var accounts = wallet.get_accounts()

返回:
    ``Array[EthAccount]``: 一个以太坊账户数组。

---------------------------------------------------------------------------------------------------------

6. get_mnemonic
^^^^^^^^^^^^^^^
获取钱包的助记词，遵循 BIP39 协议，可用于导出到其他 BIP32 标准 HD 钱包。

.. code-block:: gdscript

    # return: PackedStringArray
    var mnemonic = wallet.get_mnemonic()

返回:
    ``PackedStringArray``: 钱包助记词。

---------------------------------------------------------------------------------------------------------

7. save
^^^^^^^
保存以太坊钱包。

.. code-block:: gdscript

    # return: bool
    var success = wallet.save()

返回:
    ``bool``: ``true`` 表示成功，``false`` 表示失败

---------------------------------------------------------------------------------------------------------

8. load
^^^^^^^
加载以太坊钱包。

.. code-block:: gdscript

    # return: EthWallet
    var wallet = wallet.load()

返回:
    ``EthWallet``: 加载的 EthWallet 实例。

---------------------------------------------------------------------------------------------------------

Example
~~~~~~~
以下是一个完整的示例代码，供参考。

.. code-block:: gdscript

    var address_const = [
        "c75185ff30635988d4ae44ab544fc66130932568",
        "fce4b4e710f93dbbb219555f71a62b4cebcfa907",
        "f85ccde06eab0391d976efff4d9de02eca09c566",
        "fb30288ec7c57819547b9b0f3cdf3941cad66790",
        "dee895839eb69fe79336c76b20045f6ca2f37f6a"
    ]
        var ethMgr = EthWalletManager.new()
        var m = ["oil", "bamboo", "reject", "omit", "gentle", "boss", "useless", "fog", "genuine", "primary", "divorce", "abstract"]
        var wallet = ethMgr.from_mnemonic(m)
        var counts = 5
        for index in counts:
            wallet.add()

        var accounts = wallet.get_accounts()

        if accounts.size() > 0:
            for i in range(accounts.size()):
                var account = accounts[i]
                # Ensure the account is of type EthAccount
                if account is EthAccount:
                    var eth_account = account as EthAccount  # Cast to EthAccount
                    var address = eth_account.get_address()  # Call the method of EthAccount
                    assert(address.hex_encode() == address_const[i])  # Check if it matches using the index
                    print("Account Address:", address.hex_encode())
                else:
                    print("Object at index", i, "is not of type EthAccount")
        else:
            print("No accounts found in the wallet.")

        pass

---------------------------------------------------------------------------------------------------------

Secp256k1Wrapper
----------------

KeccakWrapper
--------------





.. autosummary::
   :toctree: generated