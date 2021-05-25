### 爬虫基础

- 爬虫作用

  - 1.爬虫批量采集数据，可奖励人工成本，提高有效数据量，给与运营/销售的数据支撑，加快产品发展。
    | 命令 | 描述 |
    | ---- | ---- |
    | 表格 | xxx |

- 合法性

  - 爬虫利用程序进行批量爬取网页公开信息，前端显示的数据信息。

- 反爬虫
  爬虫很难完全制止，
  反爬虫的一些手段

  - 和法检测：请求校验；
  - 小黑屋: ip/用户限制请求频率，或者直接拦截；
  - 投毒： 反爬虫高境界可以不用拦截，拦截试一试的，投毒返回虚假数据，可以舞蹈竞品决策；

- 爬虫软件

  - 常用工具
    python、pycharm、浏览器（chrome、火狐）和 fiddler

- 爬取数据-urllib 库

  - response.read()
    read 方法读取文件全部内容，返回 bytes 类型

  - response.getcode()
    返回 HTTP 的响应码，成功返回 200，4 服务器页面出错，5 服务器问题

  - response.geturl()
    返回实际数据的实际 URL，防止重定向问题
  - response.info()
    返回服务器相应的 http 报头

  - 使用 request 对象

### 变量

- 变量类型

  - Number
  - 布尔类型
  - String
  - List
  - Tuple
  - Dictionary

- 关系运算
  | 符号 | 表达式 |
  | ---- | ---- |
  | > | a > b |
  | < | a < b |

- 逻辑运算
  | 符号 | 表达式 |
  | ---- | ---- |
  | and | a and b |
  | or | a or b |
  | not | a not b |

- 位运算
  | 符号 | 表达式 |
  | ---- | ---- |
  | and | a and b |
  | or | a or b |
  | not | a not b |

- 进制
  `bin()` `0b`
  `Oct()` `0o`
  `int()`
  `hex()` `0x`

- 切片：主要适用于 list 结构的数据类型
  `list`[n:m] 从第 `n` 位到 `m` 位

- 迭代
  使用 `for` 循环遍历 list 或 tuple，这种遍历称为迭代。
  使用 `for...in` 完成迭代，

  ```python
    for key in list:
      pass;
  ```

  判断一个数据是否可迭代

  ```python
    from collections import Iterable
    isinstance(data, Iterable)
  ```

  返回 `list` 的索引-元素对

  ```python
    for index, value in enumerate(list):
      print(index, value)
  ```

- 列表生成式
  即 `List Cmprehensions`, 是 `Python` 内置的非常简单却强大的可以用来创建 `list` 的生成式。

  例子：生成一个 `n` 到 `m` 的 `list` 结构

  ```python
    list(range(n, m))
  ```

  例子：生成一个 `[1x1, 2x2, 3x3, ..., 10x10]` 的结构

  ```python
    L = [];
    for x in range(1, 11):
      L.append(x*x)

    #改进 使用列表生成式
    [ x * x for x in range(1, 11)]
  ```

  例子：实现全排列 'ABC' 和 'XYZ'

  ```python
    [ m + n for m in 'ABC' for n in 'XYZ' ]
  ```

- 生成器
  一边循环一边计算的机制，称为生成器

  - 创建一个 `generator`
    把一个列表生成式的[]改成()，即可创建一个 `generator`
    使用 yield 关键字

- 迭代器 `Iterator`

- 高阶函数
  一个函数可以接收另外一个函数作为参数，这种函数就称之为高级函数

  - map/reduce
    `map()`函数接收两个参数，一个是函数，一个是`Iterable`，`map`将传入的函数依次作用到序列的每个元素，并把结果作为新的`Iterator`返回。
    `reduce`把一个函数作用在一个序列[x1, x2, x3, ...]上，这个函数必须接收两个参数，reduce 把结果继续和序列的下一个元素做累积计算

  - filter
    接收一个函数和一个序列,把传入的函数依次作用于每个元素，然后根据返回值是 True 还是 False 决定保留还是丢弃该元素
