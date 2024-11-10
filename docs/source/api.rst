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
When interacting with a blockchain network, we usually need to send a transaction to it.
Legacy is a basic transaction type defined in the ETH protocol.
The LegacyTx class is a wrapper designed to support this transaction type.
It includes the variables and methods needed to create a transaction.

Example
~~~~~~~

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


JsonrpcHelper
-------------

ABIHelper
---------

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