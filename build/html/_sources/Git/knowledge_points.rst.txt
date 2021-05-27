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

.. code:: bash

    git clone --recursive https://github.com/FreeRTOS/coreHTTP.git

.. image:: _static/recursive_clone.png
    :alt: Alternative text

可以看到使用递归克隆后，引用的仓库也会被克隆下来，如果是引用仓库里面也有引用的话也一样。这样我们就能完全拉取我们所需要的所有资源。

子模块添加
------------

首先明确子模块的作用，当你的仓库引用了其他仓库的之后，也希望子仓库能够拉取到最新远端代码，代码编写完成后，如果你直接提交代码，不进行任何操作会发现仓库里面有个空文件夹。效果见下图：

.. image:: _static/submodule.jpg

.. image:: _static/github_test.jpg

这是子模块并没有链接到远程仓库地址造成的，所以需通过子模块添加命令让其正确链接

.. code:: bash

    git submodule add https://github.com/DaveGamble/cJSON.git cJSON # git submodule add [url] [directory name]

这样子模块就知道该链向哪个仓库了(注意如果你仓库里已存在 cJSON, 请先执行 git rm cJSON 将其删除)，然后再把代码提交到仓库,在 github 上看看效果。

.. image:: _static/added_submodule.jpg

原因就是没配置子模块

远程分支覆盖本地分支
--------------------

这种情况一般是我们本地的代码改乱了，或者是本地分支跟远程分支冲突太大以至于不得不得放弃本地修改。这时候就可以用远程分支直接覆盖本地分支。

.. code:: bash

    git fetch -all  # 拉取所有分支
    get reset --hard origin/master # master 为你想同步的分支名