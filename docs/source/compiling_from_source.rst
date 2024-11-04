Compiling From Source
=====================

1. Setup Godot Engine compilation environment
---------------------------------------------

`Building from Source <https://docs.godotengine.org/en/stable/contributing/development/compiling/index.html>`_

You can refer to this tutorial to set up your own Godot Engine compilation environment. Trust me, it's unbelievably simple! Definitely worth a try!

2. Clone our project
--------------------

.. code-block:: shell

   git clone https://github.com/qingfengzxr/gdscript-web3.git

3. Copy web3 module
-------------------

Copy web3 directory into Godot Engine project's modules directory.

.. code-block:: shell

   cp -rf ~/gdscript-web3/modules/web3 ~/godot/modules

.. note::

   Don't forget to replace your own path

4. Install gmp library
----------------------

ubuntu:

.. code-block:: shell

   sudo apt install libgmp-dev

Also, it can be installed by source code. Reference: https://gmplib.org/

5. Compile Godot Engine
-----------------------

Linux:

.. code-block:: shell

   scons platform=linuxbsd

MacOS:

.. code-block:: shell

   scons platform=osx arch=arm64


.. autosummary::
   :toctree: generated