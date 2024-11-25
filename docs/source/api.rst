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

    var callret = "000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000003600000000000000000000000008eee12bd33ec72a277ffa9ddf246759878589d3b0000000000000000000000000000000000000000000000000000000000000009000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001207fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff7ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffe80000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000030000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000400400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b535452494e475f54455354000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000b737472696e675f7465737400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000400800000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b535452494e475f74657374000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004d535452494e475f544553545f4d4f52455f5448414e5f33325f42595445535f6162636465666768696a6b6c6d6e6f707172737475767778797a5f3030303030303031313131313132323232323200000000000000000000000000000000000000"
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

    var callret = "000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000003600000000000000000000000008eee12bd33ec72a277ffa9ddf246759878589d3b0000000000000000000000000000000000000000000000000000000000000009000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001207fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff7ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffe80000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000030000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000400400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b535452494e475f54455354000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000b737472696e675f7465737400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000400800000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b535452494e475f74657374000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004d535452494e475f544553545f4d4f52455f5448414e5f33325f42595445535f6162636465666768696a6b6c6d6e6f707172737475767778797a5f3030303030303031313131313132323232323200000000000000000000000000000000000000"
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

EthAccount
----------

EthWallet
---------

EthWalletManager
----------------

Secp256k1Wrapper
----------------

KeccakWrapper
--------------





.. autosummary::
   :toctree: generated