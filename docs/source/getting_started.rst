入门指南
========

GDWeb3项目在 Godot 引擎的自定义 C++ 模块的基础上进行开发。

`自定义 C++ 模块 <https://docs.godotengine.org/en/stable/contributing/development/core_and_modules/custom_modules_in_cpp.html>`_

基于此功能，我们可以直接使用 C++ 实现代码，而无需担心使用 GDScript 带来的性能损失。同时，游戏开发者却可以直接使用 GDScript 来编写相关逻辑。所有底层代码最终会以 GDScript 的形式呈现。

.. note::

   感谢 Godot 团队的出色工作！

由于 GDWeb3 尚未合并到 Godot 的主分支中（未来可能会实现），您需要重新编译引擎或使用我们预编译的引擎程序。

您可以选择以下两种不同的入门路径：

1. 如果您想从源码构建或贡献开发，可以从 :doc:`从源码编译 <compiling_from_source>` 开始。

2. 如果您想直接使用编译好的工具并学习如何使用 GDWeb3 开发游戏，可以从 :doc:`Hello Optimism <hello_optimism>` 开始。这是一个简单的教程，就像经典的 **Hello World** 一样。

.. autosummary::
   :toctree: generated