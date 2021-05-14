Git 使用比较有用的知识点
=========================

递归克隆
----------

不知道你遇到过这种情况没有，一个仓库里还引用了另一个仓库，比如这种。

.. image:: _static/refrence.jpg

这时候我们直接克隆会怎么样

.. code:: bash

    git clone https://github.com/FreeRTOS/coreHTTP.git

克隆下载后发现引用的仓库并没有克隆下来，编译也依赖这个仓库里面的文件的话，我们编译肯定是不能通过的。

.. image:: _static/direct_clone.jpg

所以我们需要把引用的仓库也克隆下来，需要加的参数就是 ``--recursive``，现在看看加了参数的效果。

.. image:: _static/recursive_clone.jpg

可以看到使用递归克隆后，引用的仓库也会被克隆下来，如果是引用仓库里面也有引用的话也一样。这样我们就能完全拉取我们所需要的所有资源。
