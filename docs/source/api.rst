API Reference
=============

Web3
----

The web3 class is an abstraction for all blockchain networks. You can use this class to obtain instances for interacting with different networks.

Example
~~~~~~~
.. code-block:: gdscript

   var web3 = Web3.new()
   var op = web3.get_op_instance()



Optimism
--------
Optimism is a class used for interacting with the OP network. It encapsulates the JSON-RPC interfaces and core protocols needed for interacting with the OP network.

Example
~~~~~~~
.. code-block:: gdscript

    const NODE_RPC_URL := "https://snowy-capable-wave.optimism-sepolia.quiknode.pro/360d0830d495913ed76393730e16efb929d0f652"
    var op = Optimism.new()

    # Set the node's RPC URL
    op.set_rpc_url(NODE_RPC_URL)
    var call_msg = {
        "from": "0x0000000000000000000000000000000000000000",
        "to": CONTRACT_ADDRESS,
        "input": "0x" + packed.hex_encode(),
    }
    # Request a method of the smart contract.
    # The process of constructing packed variable is not expanded here.
    # Refer to the documentation at ABIHelper for this part of the construction process.
    var rpc_resp = op.call_contract(call_msg, "")

How to get Node's RPC URL
~~~~~~~~~~~~~~~~~~~~~~~~~
TODO

RPC methods
~~~~~~~~~~~

For RPC requests, we offer two processing methods:

  * Synchronous call
  * Asynchronous call

For synchronous calls, the interface directly returns the final execution result. For asynchronous calls, the interface returns related request parameters, requiring developers to use Godot engine capabilities for asynchronous processing.
Asynchronous call interfaces are uniformly prefixed with "async_" before the synchronous interface name.


1. chain_id
^^^^^^^^^^^
chain_id() retrieves the current chain ID for transaction replay protection.

Example
~~~~~~~
.. code-block:: gdscript
    # id: empty or self-defined id, used to identify this request
    # return: BigInt
   var chain_id = op.chain_id()

2. network_id
^^^^^^^^^^^^^

Example
~~~~~~~
.. code-block:: gdscript
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var chain_id = op.network_id()


3. block_by_hash
^^^^^^^^^^^^^^^^

block_by_hash() returns the given full block information.

.. note::

    Note that loading full blocks requires two requests. Use header_by_hash()
    if you don't need all transactions or uncle headers.

Example
~~~~~~~
.. code-block:: gdscript
    # hash: string
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var chain_id = op.block_by_hash(hash)



4. header_by_hash
^^^^^^^^^^^^^^^^^

header_by_hash() returns the block header with the given hash.

Example
~~~~~~~
.. code-block:: gdscript
    # hash: string
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var chain_id = op.header_by_hash(hash)


5. block_by_number
^^^^^^^^^^^^^^^^^^^

block_by_number() returns a block from the current canonical chain. If number is null, the
latest known block is returned.

.. note::

    Note that loading full blocks requires two requests. Use header_by_number()
    if you don't need all transactions or uncle headers.

Example
~~~~~~~
.. code-block:: gdscript
    # number: BigInt; block number
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var number = BigInt.new()
    var chain_id = op.block_by_number(number)

6. header_by_number
^^^^^^^^^^^^^^^^^^^

HeaderByNumber returns a block header from the current canonical chain. If number is
null, the latest known header is returned.

Example
~~~~~~~
.. code-block:: gdscript
    # number: BigInt; block number
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var number = BigInt.new()
    var chain_id = op.header_by_number(number)

7. block_number
^^^^^^^^^^^^^^^

block_number() returns the most recent block number

Example
~~~~~~~
.. code-block:: gdscript
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var chain_id = op.block_number()


8. block_receipts_by_number
^^^^^^^^^^^^^^^^^^^^^^^^^^^

block_receipts_by_number() returns the receipts of a given block number. The block number can be specified as follows:

* "earliest": 0
* "pending": -1
* "latest": -2
* "finalized": -3
* "safe": -4
* "default": any positive integer, representing the block number

Example
~~~~~~~
.. code-block:: gdscript
    # number: BigInt; block number
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var number = BigInt.new()
    var chain_id = op.block_receipts_by_number(number)


9. block_receipts_by_hash
^^^^^^^^^^^^^^^^^^^^^^^^^

block_receipts_by_hash() returns the receipts of a given block hash.

Example
~~~~~~~
.. code-block:: gdscript
    # hash: string; block hash
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var chain_id = op.block_receipts_by_hash(hash)


10. transaction_by_hash
^^^^^^^^^^^^^^^^^^^^^^^^

transaction_by_hash() returns the transaction with the given hash.

Example
~~~~~~~
.. code-block:: gdscript
    # hash: string; transaction hash
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var chain_id = op.transaction_by_hash(hash)


11. transaction_receipt_by_hash
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

transaction_receipt_by_hash() returns the receipt of a given transaction hash.

.. note::

    Note that the receipt is not available for pending transactions.

Example
~~~~~~~
.. code-block:: gdscript
    # hash: string; transaction hash
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var chain_id = op.transaction_receipt_by_hash(hash)

12. balance_at
^^^^^^^^^^^^^^

balance_at() returns the wei balance of the given account.
The block number can be nil, in which case the balance is taken from the latest known block.


Example
~~~~~~~
.. code-block:: gdscript
    # acount: string; address
    # number: BigInt; block number
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var chain_id = op.balance_at(address, number)


13. nonce_at
^^^^^^^^^^^^

nonce_at() returns the nonce of the given account.

Example
~~~~~~~
.. code-block:: gdscript
    # acount: string; address
    # number: BigInt; block number
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var chain_id = op.nonce_at(address, number)

14. send_transaction
^^^^^^^^^^^^^^^^^^^^

send_transaction() injects a signed transaction into the pending pool for execution.

If the transaction was a contract creation use the TransactionReceipt method to get the
contract address after the transaction has been mined.

Example
~~~~~~~
.. code-block:: gdscript
    # tx: string; signed transaction in hex format
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var chain_id = op.send_transaction(tx)


15. call_contract
^^^^^^^^^^^^^^^^^

call_contract() executes a message call transaction, which is directly executed in the VM
of the node, but never mined into the blockchain.

blockNumber selects the block height at which the call runs. It can be nil, in which
case the code is taken from the latest known block. Note that state from very old
blocks might not be available.

Example
~~~~~~~
.. code-block:: gdscript
    # call_msg: Dictionary; call message
    # block_number: String; block number
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var block_number = ""
    var chain_id = op.call_contract(call_msg, block_number, "")


16. suggest_gas_price
^^^^^^^^^^^^^^^^^^^^^

suggest_gas_price() retrieves the currently suggested gas price to allow a timely
execution of a transaction.

Example
~~~~~~~
.. code-block:: gdscript
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var chain_id = op.suggest_gas_price()

17. estimate_gas
^^^^^^^^^^^^^^^^

estimate_gas() tries to estimate the gas needed to execute a specific transaction based on
the current pending state of the backend blockchain. There is no guarantee that this is
the true gas limit requirement as other transactions may be added or removed by miners,
but it should provide a basis for setting a reasonable default.

Example
~~~~~~~
.. code-block:: gdscript
    # call_msg: Dictionary; call message
    # id: empty or self-defined id, used to identify this request
    # return: Dictionary
    var chain_id = op.estimate_gas(call_msg, "")


LegacyTx
--------
When interacting with a blockchain network, we usually need to send a transaction to it.
Legacy is a basic transaction type defined in the ETH protocol.
The LegacyTx class is a wrapper designed to support this transaction type.
It includes the variables and methods needed to create a transaction.

Example
~~~~~~~
The following example demonstrates how to construct a LegacyTx object and set its various properties.

.. code-block:: gdscript

    # create a legacyTx
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

Methods
~~~~~~~
1. set_chain_id
^^^^^^^^^^^^^^^
Sets the chain ID for the transaction.

.. code-block:: gdscript

    # chain_id: BigInt
    # return: void
    legacyTx.set_chain_id(chain_id)

Parameters:
    chain_id: BigInt

Returns:
    void

2. set_nonce
^^^^^^^^^^^^
Sets the nonce for the transaction.

todo: explain what is nonce.

.. code-block:: gdscript

    # nonce: int
    # return: void
    legacyTx.set_nonce(nonce)

Parameters:
    nonce: int

Returns:
    void

3. set_gas_price
^^^^^^^^^^^^^^^^
Sets the gas price for the transaction.

.. code-block:: gdscript

    # gas_price: BigInt
    # return: void
    legacyTx.set_gas_price(gas_price)

Parameters:
    gas_price: BigInt

Returns:
    void

4. set_gas_limit
^^^^^^^^^^^^^^^^
Sets the gas limit for the transaction.

.. code-block:: gdscript

    # gas_limit: int
    # return: void
    legacyTx.set_gas_limit(gas_limit)

Parameters:
    gas_limit: int

Returns:
    void

5. set_to_address
^^^^^^^^^^^^^^^^^
Sets the address for the transaction.

.. code-block:: gdscript

    legacyTx.set_to_address("0xE85f5c8053C1fcdf2b7b517D0DC7C3cb36c81ABF")

Parameters:
    address: string; ETH address with 0x prefix

Returns:
    void

6. set_value
^^^^^^^^^^^^
Sets the value for the transaction.

.. code-block:: gdscript

    # value: BigInt
    # return: void
    legacyTx.set_value(value)

Parameters:
    value: BigInt

Returns:
    void

7. set_data
^^^^^^^^^^^
Sets the data to be sent to the blockchain network, such as ABI-serialized data for calling a smart contract or custom data.

.. code-block:: gdscript

    # data: PackedByteArray
    # return: void
    legacyTx.set_data(data)

Parameters:
    data: PackedByteArray

Returns:
    void

8. rlp_hash
^^^^^^^^^^^
Can obtain the keccak256 hash value of LegacyTx after RLP encoding.

.. code-block:: gdscript

    # return: PackedByteArray
    var hash = legacyTx.rlp_hash()

Parameters:
    None

Returns:
    PackedByteArray

9. hash
^^^^^^^
Call this function to get the hash of the transaction. It nceessary to call after sign the transaction.

.. code-block:: gdscript

    # return: PackedByteArray
    var hash = legacyTx.hash()

Parameters:
    None

Returns:
    PackedByteArray

10. sign_tx
^^^^^^^^^^^
Sign the LegacyTx using a signer constructed from a private key.

.. code-block:: gdscript

    # signer: Ref<Secp256k1Wrapper>
    # return: void
    legacyTx.sign_tx(signer)

Parameters:
    signer: Ref<Secp256k1Wrapper>

Returns:
    int: 0 if success, -1 if failed

11. sign_tx_by_account
^^^^^^^^^^^^^^^^^^^^^^
Sign the LegacyTx by use EthAccount.

.. code-block:: gdscript

    # account: Ref<EthAccount>
    # return: void
    legacyTx.sign_tx_by_account(account)

Parameters:
    account: Ref<EthAccount>

Returns:
    int: 0 if success, -1 if failed


12. signedtx_marshal_binary
^^^^^^^^^^^^^^^^^^^^^^^^^^^
Marshal the signed transaction into binary data and encode to hex string.

.. code-block:: gdscript

    # return: PackedByteArray
    var res = legacyTx.signedtx_marshal_binary()

Parameters:
    None

Returns:
    String


BigInt
------
The BigInt class implements some basic operations related to large numbers, utilizing the GMP library to fulfill its requirements.

Methods
~~~~~~~
1. add
^^^^^^
Add two BigInt objects.

.. code-block:: gdscript

    # other: BigInt
    # return: BigInt
    var res = bigInt.add(other)

2. sub
^^^^^^
Subtract two BigInt objects.

.. code-block:: gdscript

    # other: BigInt
    # return: BigInt
    var res = bigInt.sub(other)

3. mul
^^^^^^
Multiply two BigInt objects.

.. code-block:: gdscript

    # other: BigInt
    # return: BigInt
    var res = bigInt.mul(other)

4. div
^^^^^^
Divide two BigInt objects.

.. code-block:: gdscript

    # other: BigInt
    # return: BigInt
    var res = bigInt.div(other)

5. mod
^^^^^^
Get the remainder of the division of two BigInt objects.

.. code-block:: gdscript

    # other: BigInt
    # return: BigInt
    var res = bigInt.mod(other)

6. abs
^^^^^^
Get the absolute value of a BigInt object.

.. code-block:: gdscript

    # return: BigInt
    var res = bigInt.abs()

7. cmp
^^^^^^
Compare two BigInt objects.

.. code-block:: gdscript

    # other: BigInt
    # return: int
    var res = bigInt.cmp(other)

Returns:
    int: 1 if bigInt > other, 0 if bigInt == other, -1 if bigInt < other

8. sgn
^^^^^^
Returns the sign of m_number as an integer:

.. code-block:: gdscript

    # return: int
    var res = bigInt.sgn()

Returns:
    int: 1 if bigInt > 0, 0 if bigInt == 0, -1 if bigInt < 0

8. is_zero
^^^^^^^^^^
Check if the BigInt object is zero.

.. code-block:: gdscript

    # return: bool
    var res = bigInt.is_zero()

9. from_string
^^^^^^^^^^^^^^
Create a BigInt object from a string.

.. code-block:: gdscript

    # str: string
    # return: void
    bigInt.from_string(str)

10. from_hex
^^^^^^^^^^^^
Create a BigInt object from a hex string.

.. code-block:: gdscript

    # str: string
    # return: void
    bigInt.from_hex(str)

11. get_string
^^^^^^^^^^^^^^
Get the string representation of the BigInt object.

.. code-block:: gdscript

    # return: string
    var res = bigInt.get_string()

12. to_hex
^^^^^^^^^^
Get the hex string with 0x prefix representation of the BigInt object.

.. code-block:: gdscript

    # return: string
    var res = bigInt.to_hex()


JsonrpcHelper
-------------
.. note::
   This is an internal class and is not intended for direct user use at the moment. We will provide relevant documentation when needed.



ABIHelper
---------
The Ethereum ABI (Application Binary Interface) is a standardized interface between Ethereum smart contracts and external applications. The ABI defines how to encode and decode the functions and events of a smart contract, allowing external applications to interact with the smart contract.

For more details on ABI, you can refer to `here <https://docs.soliditylang.org/en/latest/abi-spec.html>`_.

The ABIHelper class is a utility class for handling ABI encoding and decoding. It provides user-friendly methods for processing ABI encoding and decoding.

Unit test code file: abihelper_unit_test.gd

Methods
~~~~~~~
1. unmarshal_from_json
^^^^^^^^^^^^^^^^^^^^^^
Parse the ABI JSON string of the smart contract into the ABIHelper object. This sets a series of properties required for ABI encoding and decoding, including function lists, parameter types, etc., for the ABIHelper object.

.. code-block:: gdscript

    # json: string
    # return: bool
    const CONTRACT_ABI := """
    [{"inputs":[{"components":[{"internalType":"uint32","name":"a","type":"uint32"},{"internalType":"uint256[]","name":"b","type":"uint256[]"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T[]","name":"c","type":"tuple[]"},{"internalType":"int256[3]","name":"d","type":"int256[3]"}],"internalType":"struct Test.S","name":"s","type":"tuple"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T","name":"t","type":"tuple"},{"internalType":"uint256","name":"u","type":"uint256"},{"internalType":"address","name":"user","type":"address"},{"internalType":"bytes10","name":"b10","type":"bytes10"}],"name":"f","outputs":[],"stateMutability":"pure","type":"function"},{"inputs":[],"name":"g","outputs":[{"components":[{"internalType":"uint32","name":"a","type":"uint32"},{"internalType":"uint256[]","name":"b","type":"uint256[]"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T[]","name":"c","type":"tuple[]"},{"internalType":"int256[3]","name":"d","type":"int256[3]"}],"internalType":"struct Test.S","name":"","type":"tuple"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T","name":"","type":"tuple"},{"internalType":"address","name":"","type":"address"},{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"pure","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"}],"name":"h","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"nonpayable","type":"function"}]
    """
    var h = ABIHelper.new()
    var res = h.unmarshal_from_json(CONTRACT_ABI)

Parameters:
    json: string; ABI JSON string. TODO: how to get abi string.

Returns:
    bool: true if success, false if failed

2. pack
^^^^^^^
Pack the parameters into a byte array according to the ABI encoding rules.

Here we need to use two examples to illustrate the usage of the pack function. A simple example is used to explain the basic usage, and a complex example is used to explain how to handle complex data structures.

**Example 1: ERC20 transfer**

For an ERC20 contract, the definition of its transfer function is as follows:

.. code-block:: solidity

    function transfer(address recipient, uint256 amount) public returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

You can see that the transfer function accepts two parameters: a recipient of type address and an amount of type uint256. Its interface ABI definition is similar to:

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

So how do we call this function? The following code demonstrates the process of calling this function using GDScript.

.. code-block:: gdscript

    const CONTRACT_ABI := ... # ABI definition of the ERC20 contract
    var h = ABIHelper.new()
    var res = h.unmarshal_from_json(CONTRACT_ABI)

    var params = [
        # receipient
        "0xeB98753449AD50d30561a66CA48BF69EEcaD4bC3",
        # amount
        "123456"
    ]

    var packed = h.pack("transfer", params)

Parameters:
    name: string; method name

    params: Array; parameters for calling the method

Returns:
    PackedByteArray: packed data

Let's explain this code in detail. First, we create an ABIHelper object `h`, and then call the `unmarshal_from_json` function, passing in the ABI definition of the ERC20 contract. This allows the `h` object to understand the ABI definition of the ERC20 contract.

Next, we define a `params` array containing two elements. The first element is the recipient's address, and the second element is the amount value. These correspond to the two parameters of the `transfer` function.

You can understand it as: `params[0] => recipient`, `params[1] => amount`.

Therefore, you can easily deduce that the order of elements in the `params` array must match the order of parameters in the contract function. This is very important; otherwise, you will get incorrect results.

Finally, we call the `pack` function, passing in the function name `transfer` and the parameters array, to get the packed result.

We will use the packed result as the data for the `LegacyTx` and send it to the blockchain to call the `transfer` function of the ERC20 contract.


**Example 2: Complex Structure Call Example**

For some complex business scenarios, people often define custom data structures and pass them when calling functions, such as arrays, structs, nested structs, etc.

Here, we use a complex nested struct as an example to illustrate how to use the `pack` function for complex data structures. Through this example, you will fully understand the behavior of the `pack` function.

Now we have a contract that defines many complex data structures, with structs nested within other structs. The contract is defined as follows:

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
    This Test contract will also be used when introducing the unpack function.

We will use the f() function as an example to illustrate how to use the pack function.

First, we need to obtain the ABI definition of the contract. For the above contract code, its ABI definition is as follows:

.. code-block:: gdscript

    const CONTRACT_ABI := """
    [{"inputs":[{"components":[{"internalType":"uint32","name":"a","type":"uint32"},{"internalType":"uint256[]","name":"b","type":"uint256[]"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T[]","name":"c","type":"tuple[]"},{"internalType":"int256[3]","name":"d","type":"int256[3]"}],"internalType":"struct Test.S","name":"s","type":"tuple"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T","name":"t","type":"tuple"},{"internalType":"uint256","name":"u","type":"uint256"},{"internalType":"address","name":"user","type":"address"},{"internalType":"bytes10","name":"b10","type":"bytes10"}],"name":"f","outputs":[],"stateMutability":"pure","type":"function"},{"inputs":[],"name":"g","outputs":[{"components":[{"internalType":"uint32","name":"a","type":"uint32"},{"internalType":"uint256[]","name":"b","type":"uint256[]"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T[]","name":"c","type":"tuple[]"},{"internalType":"int256[3]","name":"d","type":"int256[3]"}],"internalType":"struct Test.S","name":"","type":"tuple"},{"components":[{"components":[{"internalType":"string[]","name":"t","type":"string[]"}],"internalType":"struct Test.C[]","name":"x","type":"tuple[]"},{"internalType":"bytes10","name":"y","type":"bytes10"}],"internalType":"struct Test.T","name":"","type":"tuple"},{"internalType":"address","name":"","type":"address"},{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"pure","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"}],"name":"h","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"nonpayable","type":"function"}]
    """

Now, let's explain in detail how to pack the f() function of this contract. The following is an example code in GDScript that demonstrates how to encode the f() function call using the pack function:

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

We can see that the `f()` function accepts 5 parameters, which are:

    * s: struct S
    * t: struct T
    * u: uint
    * user: address
    * b10: bytes10

The struct S and struct T are composite structures containing multiple fields, with nested structures within them. We need to fill these fields into the params array in the order defined by the contract.

    * params[0] => s
    * params[1] => t
    * params[2] => u
    * params[3] => user
    * params[4] => b10

In the above code, comments are used to explain the value of each field. This should help you clearly understand how to fill the params array.

Additionally, this example contract includes common data types and some special data, such as large numbers, mixed-case strings, and strings longer than 32 bytes. This should help you better understand how to use the pack function.

.. note::

    Now, you should be able to infer the following logical relationship:

    For fields within a struct, we use an array to represent them, where each element in the array corresponds to a field in the struct.
    The order of the elements in the array must match the order of the fields defined in the struct; otherwise, it will result in incorrect outcomes.


3. unpack
^^^^^^^^^
First, it is important to note that we do not provide a direct `unpack` function. Instead, we offer the following two methods to achieve the unpack functionality:

    * unpack_into_dictionary: Decodes the ABI-encoded data into a dictionary object.
    * unpack_into_array: Decodes the ABI-encoded data into an array.

Both methods decode the ABI-encoded data into specific data structures based on the ABI definition, but the resulting data structures provided to the developer for manipulation are different.

We will continue using the smart contract introduced in the `pack` function section. However, this time we will use the `g()` function for illustration.

The `g()` function returns 4 parameters, which are:

    * 0: struct S
    * 1: struct T
    * 2: address
    * 3: uint

.. note::

    These parameters are defined as anonymous in the ABI definition, where the name is empty. Therefore, we use numerical indices to represent these parameters.

Additionally, the return value of the `g()` function is designed to include complex data and data structures, such as large numbers, structs, arrays, etc. This helps you better understand how to use the unpack function.

.. note::

    For large numbers, we uniformly use strings for wrapping instead of directly using the BigInt class. This approach avoids some unnecessary issues. If you need to perform operations on these numbers, you can use the BigInt class for processing.

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

The above two examples correspond to using `unpack_into_dictionary` and `unpack_into_array` to decode ABI-encoded data. Their usage is as follows:

1. Initialize an `ABIHelper` object.
2. Call the `unmarshal_from_json` function, passing in the ABI definition of the contract.
3. Define `callret`. Here, a test return value after calling the `g()` function is directly provided. Typically, this value is obtained by calling a function of the smart contract, usually by calling the `call_contract()` method.
4. Define a `result` variable to store the decoded data. Depending on the chosen function, `result` needs to be either a dictionary object or an array object.
5. Call the `unpack_into_dictionary` or `unpack_into_array` function to decode `callret` into `result`.


For `unpack_into_dictionary`, the decoded value is a dictionary where the keys are the indices of the parameters or the variable names of the struct members, and the values are the parameter values.

.. note::

    Solidity allows naming the return values of functions. If the return values are named, the keys in the returned dictionary will be those names. If they are not named, the keys will be the indices of the parameters.

    function getDetails() public pure returns (uint256 id, string memory name, bool isActive);

For `unpack_into_array`, the decoded value is an array where each element corresponds to a parameter value.

---------------------------------------------------------------------------------------------------------

EthAccountManager
-----------------
`EthAccountManager` is a class for managing Ethereum accounts. It provides functionality to create new Ethereum accounts and import accounts from private keys.

.. warning::
     The APIs provided by this class have not been audited and may have security vulnerabilities. Please take precautions, properly clear memory, securely store private keys, and thoroughly test transaction receiving and sending functions before using in production!

Methods
~~~~~~~
1. create
^^^^^^^^^
Creates a new Ethereum account.

.. code-block:: gdscript

    # entropy: PackedByteArray (optional)
    # return: EthAccount
    var account_manager = EthAccountManager.new()
    var account = account_manager.create()

Parameters:
    1. ``entropy``: ``PackedByteArray`` (optional); Entropy used to generate a random private key (must be 32 bytes). If empty, the internal system random number is used.
Returns:
   ``EthAccount``: An instance of the created Ethereum account object.


.. note::
    To ensure the security of the generated account, although a certain degree of entropy is provided internally, we still recommend that you provide a higher entropy as a parameter.

---------------------------------------------------------------------------------------------------------

2. privateKeyToAccount
^^^^^^^^^^^^^^^^^^^^^^
Creates an Ethereum account based on the provided private key.

.. code-block:: gdscript

    # privkey: PackedByteArray
    # return: EthAccount
    var account_manager = EthAccountManager.new()
    var account = account_manager.privateKeyToAccount(private_key)

Parameters:
    1. ``privkey``: ``PackedByteArray``; The private key bytes of the account. Must be in a valid Ethereum private key format.

Returns:
    ``EthAccount``: An instance of the Ethereum account created from the private key.

---------------------------------------------------------------------------------------------------------

Example
~~~~~~~
The following example demonstrates how to use EthAccountManager to create a new account and import an account from a private key:

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
`EthAccount` is a class that handles basic operations for Ethereum accounts. It provides access to account information and signing data. Through `EthAccount`, users can obtain the private key, public key, and address of the account, and use the account's private key to sign data.

.. note::
    Please note that instances of `EthAccount` must be generated through `EthAccountManager` to ensure proper initialization and management of the account.

.. warning::
    The APIs provided by this class have not been audited and may have security vulnerabilities. Please take precautions, properly clear memory, securely store private keys, and thoroughly test transaction receiving and sending functions before using in production!

Methods
~~~~~~~
1. get_private_key
^^^^^^^^^^^^^^^^^^
Gets the private key of the account.

.. code-block:: gdscript

    # return: PackedByteArray
    var private_key = account.get_private_key()

Returns:
    ``PackedByteArray``: The private key bytes of the account.

---------------------------------------------------------------------------------------------------------

2. get_public_key
^^^^^^^^^^^^^^^^^
Gets the public key of the account.

.. code-block:: gdscript

    # return: PackedByteArray
    var public_key = account.get_public_key()

Returns:
    ``PackedByteArray``: The public key bytes of the account.

---------------------------------------------------------------------------------------------------------

3. get_address
^^^^^^^^^^^^^^
Gets the address of the account (in byte format).

.. code-block:: gdscript

    # return: PackedByteArray
    var address = account.get_address()

Returns:
    ``PackedByteArray``: The address bytes of the account.

---------------------------------------------------------------------------------------------------------

4. get_hex_address
^^^^^^^^^^^^^^^^^^
Gets the address of the account in hexadecimal format.

.. code-block:: gdscript

    # return: String
    var hex_address = account.get_hex_address()

Returns:
    ``String``: The hexadecimal string address of the account.

---------------------------------------------------------------------------------------------------------

5. sign_data
^^^^^^^^^^^^
Signs the provided data using the account's private key.

.. code-block:: gdscript

    # data: PackedByteArray
    # return: PackedByteArray
    var signature = account.sign_data(data)

Parameters:
    1. ``data``: ``PackedByteArray``; The data to be signed.

Returns:
    ``PackedByteArray``: The signed data.

---------------------------------------------------------------------------------------------------------

6. sign_data_with_prefix
^^^^^^^^^^^^^^^^^^^^^^^^
Signs the data after computing the keccak256 hash of the Ethereum signed message prefix.

.. code-block:: gdscript

    # data: PackedByteArray
    # return: PackedByteArray
    var signature = account.sign_data_with_prefix(data)

Parameters:
    1. ``data``: ``PackedByteArray``; The data to be signed.

Returns:
    ``PackedByteArray``: The signed data.

.. note::
    - This method follows the Ethereum signed message standard:
    - keccak256("\\x19Ethereum Signed Message:\\n" + len(message) + message)

---------------------------------------------------------------------------------------------------------

Example
~~~~~~~
The following example demonstrates how to perform basic operations using EthAccount:

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
`EthWalletManager` is a class for managing multiple Ethereum HD wallets. It provides functionality to create, load, and save wallets. Through this manager, new wallet instances can be generated or existing wallets can be restored from mnemonic phrases.

.. warning::
    The APIs provided by this class have not been audited and may have security vulnerabilities. Please take precautions, properly clear memory, securely store private keys, and thoroughly test transaction receiving and sending functions before using in production!

Methods
~~~~~~~
1. create
^^^^^^^^^
Creates a new Ethereum wallet.

.. code-block:: gdscript

    # strength: int (optional)
    # entropy: PackedByteArray (optional)
    # passphrase: String (optional)
    # return: EthWallet
    var wallet = wallet_manager.create()
    var wallet = wallet_manager.create(1, entropy, "my passphrase")

Parameters:
    1. ``strength``: ``int`` (optional); create number of accounts.
    2. ``entropy``: ``PackedByteArray`` (optional); entropy used when generating the wallet.
    3. ``passphrase``: ``String`` (optional); passphrase for additional security.

Returns:
    ``EthWallet``: instance of the created wallet.

---------------------------------------------------------------------------------------------------------

2. from_mnemonic
^^^^^^^^^^^^^^^^
Restore a wallet from mnemonic phrase.

.. code-block:: gdscript

    # mnemonic: PackedStringArray
    # passphrase: String (optional)
    # return: EthWallet
    var wallet = wallet_manager.from_mnemonic(mnemonic, "optional passphrase")

Parameters:
    1. ``mnemonic``: ``PackedStringArray``;The mnemonic phrase, currently only supports a mnemonic phrase consisting of 12 English words.
    2. ``passphrase``: ``String`` (optional); passphrase.

Returns:
    ``EthWallet``: instance of the created wallet.

---------------------------------------------------------------------------------------------------------

3. load
^^^^^^^
Load an wallet.

.. code-block:: gdscript

    # return: EthWallet
    var wallet = wallet_manager.load()

Returns:
    ``EthWallet``: instance of the loaded wallet.

---------------------------------------------------------------------------------------------------------

4. save
^^^^^^^
Save the wallet.

.. code-block:: gdscript

    # hd_wallet: EthWallet
    # return: bool
    var success = wallet_manager.save(wallet)

Parameters:
    1. ``hd_wallet``: ``EthWallet``; The wallet to be saved.

Returns:
    ``bool``: ``true`` was successful, ``false`` was failed

---------------------------------------------------------------------------------------------------------

Example
~~~~~~~
A complete example code for reference.

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
`EthWallet` is a class that implements hierarchical deterministic (HD) wallet functionality. It allows users to manage multiple Ethereum accounts and provides functions for adding, removing accounts, and handling mnemonic phrases.

.. warning::
    The APIs provided by this class have not been audited and may have security vulnerabilities. Please take precautions, properly clear memory, securely store private keys, and thoroughly test transaction receiving and sending functions before using in production!

.. note::
    Instances of `EthWallet` must be generated through `EthWalletManager` to ensure proper initialization and management of the wallet.

Methods
~~~~~~~
1. add
^^^^^^
Adds a new Ethereum account to the wallet.

.. code-block:: gdscript

    # privateKey: PackedByteArray (optional)
    # return: bool
    # Create a wallet manager
    var wallet_manager = EthWalletManager.new()

    var wallet = wallet_manager.create()

    var success = wallet.add()   # Generate a new account
    var success = wallet.add(private_key)  # Add an account with an existing private key

Parameters:
    1. ``privateKey``: ``PackedByteArray`` (optional); The private key of the new account. If empty, a new account compliant with BIP32 will be generated.
Returns:
    ``bool``: ``true`` was successful, ``false`` was failed

---------------------------------------------------------------------------------------------------------

2. remove_address
^^^^^^^^^^^^^^^^^
Removes an account by address.

.. code-block:: gdscript

    # address: PackedByteArray
    # return: bool
    var success = wallet.remove_address(address)

Parameters:
    1. ``address``: ``PackedByteArray``; The address to be removed.

Returns:
    ``bool``: ``true`` was successful, ``false`` was failed

---------------------------------------------------------------------------------------------------------

3. remove
^^^^^^^^^
Removes the account by index.
.. code-block:: gdscript

    # index: int
    # return: bool
    var success = wallet.remove(0)  #  Remove the first account

Parameters:
    1. ``index``: ``uint64_t``; The index of the account to be removed.

Returns:
    ``bool``: ``true`` was successful, ``false`` was failed

---------------------------------------------------------------------------------------------------------

4. clear
^^^^^^^^
Safely clears all wallet data, including accounts and mnemonic phrases.

.. code-block:: gdscript

    # return: bool
    var success = wallet.clear()

Returns:
    ``bool``: ``true`` was successful, ``false`` was failed

---------------------------------------------------------------------------------------------------------

5. get_accounts
^^^^^^^^^^^^^^^
Gets all accounts in the wallet.

.. code-block:: gdscript

    # return: Array[EthAccount]
    var accounts = wallet.get_accounts()

Returns:
    ``Array[EthAccount]``: An array of Ethereum accounts.

---------------------------------------------------------------------------------------------------------

6. get_mnemonic
^^^^^^^^^^^^^^^
Gets the wallet's mnemonic phrase following the BIP39 protocol, which can be used for export to other BIP32 standard HD wallets.

.. code-block:: gdscript

    # return: PackedStringArray
    var mnemonic = wallet.get_mnemonic()

Returns:
    ``PackedStringArray``: The wallet mnemonic phrase.

---------------------------------------------------------------------------------------------------------

7. save
^^^^^^^
Save Ethereum wallet.

.. code-block:: gdscript

    # return: bool
    var success = wallet.save()

Returns:
    ``bool``: ``true`` was successful, ``false`` was failed

---------------------------------------------------------------------------------------------------------

8. load
^^^^^^^
Loads an Ethereum wallet.

.. code-block:: gdscript

    # return: EthWallet
    var wallet = wallet.load()

Returns:
    ``EthWallet``:  The loaded EthWallet instance.

---------------------------------------------------------------------------------------------------------

Example
~~~~~~~
A complete example code for reference.

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