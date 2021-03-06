.. pypinyin documentation master file, created by
   sphinx-quickstart on Fri Sep 06 22:22:13 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


汉语拼音转换工具（Python 版）
=============================

|Build| |Coverage| |Pypi version| |Pypi downloads|


将汉语转为拼音。可以用于汉字注音、排序、检索。

基于 `hotoo/node-pinyin <https://github.com/hotoo/node-pinyin>`__ 开发。

* Documentation: http://pypinyin.rtfd.org
* GitHub: https://github.com/mozillazg/python-pinyin
* License: MIT license
* PyPI: https://pypi.python.org/pypi/pypinyin
* Python version: 2.6, 2.7, pypy, 3.3


特性
----

* 根据词组智能匹配最正确的拼音。
* 支持多音字。
* 简单的繁体支持。
* 支持多种不同拼音风格。


安装
----

.. code-block:: bash

    $ pip install pypinyin

为了更好的处理包含多音字及非中文字符的字符串，
推荐同时安装 `jieba <https://github.com/fxsjy/jieba>`__ 分词模块。


使用示例
--------

.. code-block:: python

    >>> from pypinyin import pinyin, lazy_pinyin
    >>> import pypinyin
    >>> pinyin(u'中心')
    [[u'zh\u014dng'], [u'x\u012bn']]
    >>> pinyin(u'中心', heteronym=True)  # 启用多音字模式
    [[u'zh\u014dng', u'zh\xf2ng'], [u'x\u012bn']]
    >>> pinyin(u'中心', style=pypinyin.INITIALS)  # 设置拼音风格
    [['zh'], ['x']]
    >>> lazy_pinyin(u'中心')
    ['zhong', 'xin']

命令行工具：

.. code-block:: console

    $ pypinyin 音乐
    yīn yuè
    $ pypinyin -h


分词处理
--------

如果安装了 `jieba <https://github.com/fxsjy/jieba>`__ 分词模块，程序会自动调用。

使用其他分词模块：

1. 安装分词模块，比如 ``pip install snownlp`` ；
2. 使用经过分词处理的字符串列表作参数：

   .. code-block:: python

       >> from pypinyin import lazy_pinyin, TONE2
       >> from snownlp import SnowNLP
       >> hans = u'音乐123'
       >> 
       >> lazy_pinyin(hans, style=TONE2)
       [u'yi1n', u'le4', u'1', u'2', u'3']
       >>
       >> hans_seg = SnowNLP(hans).words  # 分词处理
       >> hans_seg
       [u'\u97f3\u4e50', u'123']
       >> lazy_pinyin(hans_seg, style=TONE2)
       [u'yi1n', u'yue4', u'123']


自定义拼音库
------------

如果对结果不满意，可以通过自定义拼音库的方式修正结果：

.. code-block:: python

    >> from pypinyin import lazy_pinyin, load_phrases_dict, TONE2
    >> hans = u'桔子'
    >> 
    >> lazy_pinyin(hans, style=TONE2)
    [u'jie2', u'zi3']
    >> 
    >> load_phrases_dict({u'桔子': [[u'jú'], [u'zǐ']]})
    >> lazy_pinyin(hans, style=TONE2)
    [u'ju2', u'zi3']


贡献
----

* `New Issue <https://github.com/mozillazg/python-pinyin/issues/new>`__
* Pull Request:
    1. `Fork <https://github.com/mozillazg/python-pinyin/fork>`__
    2. ``git clone git@github.com:your-username/python-pinyin.git``
    3. ``git checkout develop``
    4. ``git checkout -b your-branch-name``
    5. ``git commit -am "commit"``
    6. ``git push origin your-branch-name``
    7. New pull request to **develop** branch




.. |Build| image:: https://api.travis-ci.org/mozillazg/python-pinyin.png?branch=master
   :target: https://travis-ci.org/mozillazg/python-pinyin
.. |Coverage| image:: https://coveralls.io/repos/mozillazg/python-pinyin/badge.png?branch=master
   :target: https://coveralls.io/r/mozillazg/python-pinyin
.. |PyPI version| image:: https://pypip.in/v/pypinyin/badge.png
   :target: https://crate.io/packages/pypinyin
.. |PyPI downloads| image:: https://pypip.in/d/pypinyin/badge.png
   :target: https://crate.io/packages/pypinyin



API
---

拼音风格:

+-----------------------+----------------------------------------------------------------------------------+
|pypinyin.NORMAL        |普通风格(0)，不带声调。如： ``pin yin``                                           |
+-----------------------+----------------------------------------------------------------------------------+
|pypinyin.TONE          |声调风格(1)，拼音声调在韵母第一个字母上（默认风格）。如： ``pīn yīn``             |
+-----------------------+----------------------------------------------------------------------------------+
|pypinyin.TONE2         |声调风格2(2)，即拼音声调在各个拼音之后，用数字 [0-4] 进行表示。如： ``pi1n yi1n`` |
+-----------------------+----------------------------------------------------------------------------------+
|pypinyin.INITIALS      |声母风格(3)，只返回各个拼音的声母部分。如： ``中国`` 的拼音 ``zh g``              |
+-----------------------+----------------------------------------------------------------------------------+
|pypinyin.FIRST_LETTER  |首字母风格(4)，只返回拼音的首字母部分。如： ``p y``                               |
+-----------------------+----------------------------------------------------------------------------------+
|pypinyin.FINALS        |韵母风格1(5)，只返回各个拼音的韵母部分，不带声调。如： ``ong uo``                 |
+-----------------------+----------------------------------------------------------------------------------+
|pypinyin.FINALS_TONE   |韵母风格2(6)，带声调，声调在韵母第一个字母上。如： ``ōng uó``                     |
+-----------------------+----------------------------------------------------------------------------------+
|pypinyin.FINALS_TONE2  |韵母风格2(7)，带声调，声调在各个拼音之后，用数字 [0-4] 进行表示。如： ``o1ng uo2``|
+-----------------------+----------------------------------------------------------------------------------+


.. autofunction:: pypinyin.pinyin

.. autofunction:: pypinyin.slug

.. autofunction:: pypinyin.lazy_pinyin

.. autofunction:: pypinyin.load_single_dict

.. autofunction:: pypinyin.load_phrases_dict


Changelog
---------

0.4.4 (2014-01-16)
++++++++++++++++++

* 清理命令行命令的输出结果，去除无关信息
* 修复“ImportError: No module named runner”


0.4.3 (2014-01-10)
++++++++++++++++++

* 修复命令行工具在 Python 3 下的兼容性问题


0.4.2 (2014-01-10)
++++++++++++++++++

* 去除拼音风格前的 ``STYLE_`` 前缀（兼容包含 ``STYLE_`` 前缀的拼音风格）
* 增加命令行工具，具体用法请见： ``pypinyin -h``


0.4.1 (2014-01-04)
++++++++++++++++++

* 支持自定义拼音库，方便用户修正程序结果


0.4.0 (2014-01-03)
++++++++++++++++++

* 将 ``jieba`` 模块改为可选安装，用户可以选择使用自己喜爱的分词模块对汉字进行分词处理
* 支持 Python 3


0.3.1 (2013-12-24)
++++++++++++++++++

* 增加 ``lazy_pinyin`` ::

    >>> lazy_pinyin(u'中心')
    ['zhong', 'xin']


0.3.0 (2013-09-26)
++++++++++++++++++

* 修复首字母风格无法正确处理只有韵母的汉字

* 新增三个拼音风格:
    * ``pypinyin.STYLE_FINALS`` ：       韵母风格1，只返回各个拼音的韵母部分，不带声调。如： ``ong uo``
    * ``pypinyin.STYLE_FINALS_TONE`` ：   韵母风格2，带声调，声调在韵母第一个字母上。如： ``ōng uó``
    * ``pypinyin.STYLE_FINALS_TONE2`` ：  韵母风格2，带声调，声调在各个拼音之后，用数字 [0-4] 进行表示。如： ``o1ng uo2``


0.2.0 (2013-09-22)
++++++++++++++++++

* 完善对中英文混合字符串的支持::

    >> pypinyin.pinyin(u'你好abc')
    [[u'n\u01d0'], [u'h\u01ceo'], [u'abc']]


0.1.0 (2013-09-21)
++++++++++++++++++

* Initial Release



Indices and tables
------------------

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

