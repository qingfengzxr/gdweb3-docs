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

LegacyTx
--------

BigInt
------

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