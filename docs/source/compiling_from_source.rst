从源码编译
==========

1. 设置 Godot 引擎编译环境
---------------------------

`从源码构建 <https://docs.godotengine.org/en/stable/contributing/development/compiling/index.html>`_

你可以参考这个教程来设置你自己的 Godot 引擎编译环境。相信我，这非常简单！绝对值得一试！

2. 克隆我们的项目
-------------------

.. code-block:: shell

   git clone https://github.com/qingfengzxr/gdscript-web3.git

3. 复制 web3 模块
------------------

将 web3 目录复制到 Godot 引擎项目的 modules 目录中。

.. code-block:: shell

   cp -rf ~/gdscript-web3/modules/web3 ~/godot/modules

.. note::

   不要忘记替换成你自己的路径

4. 安装 gmp 库
--------------

ubuntu:

.. code-block:: shell

   sudo apt install libgmp-dev

也可以通过源码安装。参考：https://gmplib.org/

5. 编译 Godot 引擎
------------------

Linux:

.. code-block:: shell

   scons platform=linuxbsd

MacOS:

.. code-block:: shell

   scons platform=osx arch=arm64


.. autosummary::
   :toctree: generated

Windows:

.. note::
   TODO

