---
title: N维数组
description: 
published: true
date: 2023-03-14T02:14:34.568Z
tags: 
editor: markdown
dateCreated: 2023-03-14T02:14:32.645Z
---

# N维数组（`ndarray`）

一个 [`ndarray`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.html#numpy.ndarray)是具有相同类型和大小的项目的（通常是固定大小的）多维容器。 尺寸和数组中的项目的数量是由它的[`shape`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.shape.html#numpy.ndarray.shape)定义， 它是由N个非负整数组成的[`tuple`open in new window](https://docs.python.org/dev/library/stdtypes.html#tuple)（元组），用于指定每个维度的大小。 数组中项目的类型由单独的[`data-type object (dtype)`open in new window](https://numpy.org/doc/1.17/reference/arrays.dtypes.html#arrays-dtypes)指定， 其中一个与每个ndarray相关联。

与Python中的其他容器对象一样，可以通过对数组进行[索引或切片open in new window](https://numpy.org/doc/1.17/reference/arrays.indexing.html#arrays-indexing)（例如，使用N个整数）以及通过[`ndarray`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.html#numpy.ndarray)的方法和属性来访问和修改[`ndarray`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.html#numpy.ndarray)的内容。

不同的是,[`ndarrays`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.html#numpy.ndarray)可以共享相同的数据， 因此在一个[`ndarray`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.html#numpy.ndarray)中进行的更改可能在另一个中可见。 也就是说，ndarray可以是另一个ndarray 的 *“view”* ，它所指的数据由 *“base”* ndarray处理。 ndarrays也可以是Python拥有的内存[`strings`open in new window](https://docs.python.org/dev/library/stdtypes.html#str)或实现 `buffer` 或[数组接口](https://www.numpy.org.cn/reference/arrays/interface.html)的对象的视图。

**例子**：

尺寸为2 x 3的二维数组，由4个字节的整数元素组成：

```python
>>> x = np.array([[1, 2, 3], [4, 5, 6]], np.int32)
>>> type(x)
<type 'numpy.ndarray'>
>>> x.shape
(2, 3)
>>> x.dtype
dtype('int32')
```

可以使用类似Python容器的语法对数组进行索引：

```python
>>> # The element of x in the *second* row, *third* column, namely, 6.
>>> x[1, 2]
```

例如，[切片](https://www.numpy.org.cn/reference/arrays/indexing.html)可以生成数组的视图：

```python
>>> y = x[:,1]
>>> y
array([2, 5])
>>> y[0] = 9 # this also changes the corresponding element in x
>>> y
array([9, 5])
>>> x
array([[1, 9, 3],
       [4, 5, 6]])
```

## [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#构造数组)构造数组

可以使用[数组创建API](https://www.numpy.org.cn/reference/routines/array-creation.html)中详述的使用方法以及使用低级 [`ndarray`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.html#numpy.ndarray) 构造函数构建新数组：

| 方法                                                         | 描述                                     |
| ------------------------------------------------------------ | ---------------------------------------- |
| [ndarrayopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.html#numpy.ndarray)(shape[, dtype, buffer, offset, ...]) | 数组对象表示固定大小的项的多维同构数组。 |

## [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#索引数组)索引数组

可以使用扩展的Python切片语法对数组建立索引 `array[selection]`。 类似的语法也用于访问[结构化数据类型open in new window](https://numpy.org/devdocs/glossary.html#term-structured-data-type)中的字段。

另见

[数组索引](https://www.numpy.org.cn/reference/arrays/indexing.html)。

## [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#ndarray的内存布局)ndarray的内存布局

类的实例[`ndarray`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.html#numpy.ndarray) 由计算机内存的连续一维段（由数组拥有，或由某个其他对象拥有）组成， 并与将 *N个* 整数映射到块中项的位置的索引方案相结合。 索引可以变化的范围由[`shape`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.shape.html#numpy.ndarray.shape)数组的指定。 每个项目占用多少字节以及如何解释字节由与数组关联的[数据类型对象](https://www.numpy.org.cn/reference/arrays/dtypes.html)定义。

存储器段本质上是1维的，并且存在许多不同的方案用于在1维块中布置 *N* 维数组的项。NumPy非常灵活，[`ndarray`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.html#numpy.ndarray) 对象可以适应任何 *跨步索引方案* 。在跨步方案中，N维索引 ![(n_0, n_1, ..., n_{N-1})](https://www.numpy.org.cn/static/images/math/edb5f8b6064d0edc2bc57a1714249e0eae1a33e3.svg) 对应于偏移量（以字节为单位）：

![n_{\mathrm{offset}} = \sum_{k=0}^{N-1} s_k n_k](https://www.numpy.org.cn/static/images/math/1388948b609ce9a1d9ae0380d361628d6b385812.svg)

从与数组关联的内存块的开头。 这里是指定[`strides`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.strides.html#numpy.ndarray.strides)数组的整数。 的[column-majoropen in new window](https://numpy.org/devdocs/glossary.html#term-column-major)顺序（使用，例如，在Fortran语言和 *Matlab的* ）和 [row-majoropen in new window](https://numpy.org/devdocs/glossary.html#term-row-major)顺序方案（在C中使用）都只是特定种类的跨距方案的，并对应于可以被存储器 *寻址* 由步幅：

![n_{\mathrm{offset}} = \sum_{k=0}^{N-1} s_k n_k](https://www.numpy.org.cn/static/images/math/af328186eedd2e4200b34e0e6a31acae4dbc9d20.svg)

where ![d_j](https://www.numpy.org.cn/static/images/math/5e6cfb16a1d0565098e1a35072ef6fbfef092db3.svg) *= self.shape[j]*.

C和Fortran命令都是[连续的open in new window](https://docs.python.org/dev/glossary.html#term-contiguous)，*即* 单段内存布局， 其中内存块的每个部分都可以通过某些索引组合来访问。

虽然具有相应标志集的C风格和Fortran风格的连续数组可以通过上述步骤来解决，但实际的步幅可能不同。这可能发生在两种情况：

1. 如果 `self.shape[k] == 1`，则对于任何合法索引 `index[k] == 0`。这意味着在偏移量的公式中，因此和 *= self.strides[k]* 的值是任意的。
2. 如果数组没有元素 (`self.size == 0`) ，则没有合法索引，并且从不使用跨距。任何没有元素的数组都可以被认为是C样式和Fortran样式的连续数组。

点 1.表示`self`并且`self.squeeze()`始终具有相同的连续性和`aligned`标志值。这也意味着即使是高维数组也可能同时是C风格和Fortran风格的连续。

如果所有元素的内存偏移量和基本偏移量本身是 *self.itemsize* 的倍数，则认为数组是对齐的。 了解 *内存对齐* 可以在大多数硬件上实现更好的性能。

注意

默认情况下尚未应用点（1）和（2）。从NumPy 1.8.0开始，只有`NPY_RELAXED_STRIDES_CHECKING=1`在构建NumPy时定义了环境变量时才会一致地应用它们。最终这将成为默认值。

您可以通过查看 `np.ones((10,1), order='C').flags.f_contiguous` 的值来检查在构建NumPy时是否启用了此选项。如果这是 `True`，则您的NumPy已启用松弛步幅检查。

警告

它通常不认为对于C型连续数组，`self.strides[-1] == self.itemsize` 或对于Fortran样式连续数组，`self.strides[0] == self.itemsize` 为真。

除非另有说明，否则new [`ndarrays`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.html#numpy.ndarray) 中的数据采用[row-majoropen in new window](https://numpy.org/devdocs/glossary.html#term-row-major)(C) 顺序， 但是，例如，[基本数组切片](https://www.numpy.org.cn/reference/arrays/indexing.html)通常会 以不同的方案生成[视图open in new window](https://numpy.org/devdocs/glossary.html#term-view)。

注意

NumPy中的几种算法适用于任意跨步数组。但是，某些算法需要单段数组。当不规则跨越的数组传递给这样的算法时，自动进行复制。

## [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#数组属性)数组属性

数组属性反映了数组本身固有的信息。通常，通过其属性访问数组允许您获取并有时设置数组的内部属性，而无需创建新数组。公开的属性是数组的核心部分，只有一些属性可以有意义地重置而无需创建新数组。有关每个属性的信息如下。

### [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#内存布局)内存布局

以下属性包含有关数组内存布局的信息：

| 方法                                                         | 描述                                   |
| ------------------------------------------------------------ | -------------------------------------- |
| [ndarray.flagsopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.flags.html#numpy.ndarray.flags) | 有关数组内存布局的信息。               |
| [ndarray.shapeopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.shape.html#numpy.ndarray.shape) | 数组维度的元组。                       |
| [ndarray.stridesopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.strides.html#numpy.ndarray.strides) | 遍历数组时每个维度中的字节元组。       |
| [ndarray.ndimopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.ndim.html#numpy.ndarray.ndim) | 数组维数。                             |
| [ndarray.dataopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.data.html#numpy.ndarray.data) | Python缓冲区对象指向数组的数据的开头。 |
| [ndarray.sizeopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.size.html#numpy.ndarray.size) | 数组中的元素数。                       |
| [ndarray.itemsizeopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.itemsize.html#numpy.ndarray.itemsize) | 一个数组元素的长度，以字节为单位       |
| [ndarray.nbytesopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.nbytes.html#numpy.ndarray.nbytes) | 数组元素消耗的总字节数。               |
| [ndarray.baseopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.base.html#numpy.ndarray.base) | 如果内存来自其他对象，则为基础对象。   |

### [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#数据类型)数据类型

另见

[数据类型对象](https://www.numpy.org.cn/reference/arrays/dtypes.html)

可以在[`dtype`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.dtype.html#numpy.ndarray.dtype)属性中找到与该数组关联的数据类型对象 ：

| 方法                                                         | 描述                 |
| ------------------------------------------------------------ | -------------------- |
| [ndarray.dtypeopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.dtype.html#numpy.ndarray.dtype) | 数组元素的数据类型。 |

### [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#其他属性)其他属性

| 方法                                                         | 描述                                 |
| ------------------------------------------------------------ | ------------------------------------ |
| [ndarray.Topen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.T.html#numpy.ndarray.T) | 转置数组。                           |
| [ndarray.realopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.real.html#numpy.ndarray.real) | 数组的真实部分。                     |
| [ndarray.imagopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.imag.html#numpy.ndarray.imag) | 数组的虚部。                         |
| [ndarray.flatopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.flat.html#numpy.ndarray.flat) | 数组上的一维迭代器。                 |
| [ndarray.ctypesopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.ctypes.html#numpy.ndarray.ctypes) | 一个简化数组与ctypes模块交互的对象。 |

### [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#数组接口)数组接口

另见

[数组接口](https://www.numpy.org.cn/reference/arrays/interface.html)。

| 方法                                                         | 描述                        |
| ------------------------------------------------------------ | --------------------------- |
| [__array_interface__](https://www.numpy.org.cn/reference/arrays/interface.html#__array_interface__) | 数组接口的Python端          |
| __array_struct__                                             | 数组接口的C语言端（C-side） |

### [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#ctypes外部函数接口)`ctypes`外部函数接口

| 方法                                                         | 描述                                 |
| ------------------------------------------------------------ | ------------------------------------ |
| [ndarray.ctypesopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.ctypes.html#numpy.ndarray.ctypes) | 一个简化数组与ctypes模块交互的对象。 |

## [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#数组方法)数组方法

一个[`ndarray`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.html#numpy.ndarray)对象具有上或与以某种方式在数组，典型地返回一个数组结果操作的许多方法。下面简要说明这些方法。（每个方法的docstring都有更完整的描述。）

对于下面的方法在那里也相应的功能

[`numpy`](https://www.numpy.org.cn/reference/arrays/index.html)：[`all`open in new window](https://numpy.org/devdocs/reference/generated/numpy.all.html#numpy.all)，[`any`open in new window](https://numpy.org/devdocs/reference/generated/numpy.any.html#numpy.any)，[`argmax`open in new window](https://numpy.org/devdocs/reference/generated/numpy.argmax.html#numpy.argmax)， [`argmin`open in new window](https://numpy.org/devdocs/reference/generated/numpy.argmin.html#numpy.argmin)，[`argpartition`open in new window](https://numpy.org/devdocs/reference/generated/numpy.argpartition.html#numpy.argpartition)，[`argsort`open in new window](https://numpy.org/devdocs/reference/generated/numpy.argsort.html#numpy.argsort)，[`choose`open in new window](https://numpy.org/devdocs/reference/generated/numpy.choose.html#numpy.choose)， [`clip`open in new window](https://numpy.org/devdocs/reference/generated/numpy.clip.html#numpy.clip)，[`compress`open in new window](https://numpy.org/devdocs/reference/generated/numpy.compress.html#numpy.compress)，[`copy`open in new window](https://numpy.org/devdocs/reference/generated/numpy.copy.html#numpy.copy)，[`cumprod`open in new window](https://numpy.org/devdocs/reference/generated/numpy.cumprod.html#numpy.cumprod)， [`cumsum`open in new window](https://numpy.org/devdocs/reference/generated/numpy.cumsum.html#numpy.cumsum)，[`diagonal`open in new window](https://numpy.org/devdocs/reference/generated/numpy.diagonal.html#numpy.diagonal)，[`imag`open in new window](https://numpy.org/devdocs/reference/generated/numpy.imag.html#numpy.imag)，[`max`open in new window](https://numpy.org/devdocs/reference/generated/numpy.amax.html#numpy.amax)， [`mean`open in new window](https://numpy.org/devdocs/reference/generated/numpy.mean.html#numpy.mean)，[`min`open in new window](https://numpy.org/devdocs/reference/generated/numpy.amin.html#numpy.amin)，[`nonzero`open in new window](https://numpy.org/devdocs/reference/generated/numpy.nonzero.html#numpy.nonzero)，[`partition`open in new window](https://numpy.org/devdocs/reference/generated/numpy.partition.html#numpy.partition)， [`prod`open in new window](https://numpy.org/devdocs/reference/generated/numpy.prod.html#numpy.prod)，[`ptp`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ptp.html#numpy.ptp)，[`put`open in new window](https://numpy.org/devdocs/reference/generated/numpy.put.html#numpy.put)，[`ravel`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ravel.html#numpy.ravel)，[`real`open in new window](https://numpy.org/devdocs/reference/generated/numpy.real.html#numpy.real)， [`repeat`open in new window](https://numpy.org/devdocs/reference/generated/numpy.repeat.html#numpy.repeat)，[`reshape`open in new window](https://numpy.org/devdocs/reference/generated/numpy.reshape.html#numpy.reshape)，[`round`open in new window](https://numpy.org/devdocs/reference/generated/numpy.around.html#numpy.around)， [`searchsorted`open in new window](https://numpy.org/devdocs/reference/generated/numpy.searchsorted.html#numpy.searchsorted)，[`sort`open in new window](https://numpy.org/devdocs/reference/generated/numpy.sort.html#numpy.sort)，[`squeeze`open in new window](https://numpy.org/devdocs/reference/generated/numpy.squeeze.html#numpy.squeeze)，[`std`open in new window](https://numpy.org/devdocs/reference/generated/numpy.std.html#numpy.std)， [`sum`open in new window](https://numpy.org/devdocs/reference/generated/numpy.sum.html#numpy.sum)，[`swapaxes`open in new window](https://numpy.org/devdocs/reference/generated/numpy.swapaxes.html#numpy.swapaxes)，[`take`open in new window](https://numpy.org/devdocs/reference/generated/numpy.take.html#numpy.take)，[`trace`open in new window](https://numpy.org/devdocs/reference/generated/numpy.trace.html#numpy.trace)， [`transpose`open in new window](https://numpy.org/devdocs/reference/generated/numpy.transpose.html#numpy.transpose)，[`var`open in new window](https://numpy.org/devdocs/reference/generated/numpy.var.html#numpy.var)。

### [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#数组转换)数组转换

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [ndarray.itemopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.item.html#numpy.ndarray.item)(*args) | 将数组元素复制到标准Python标量并返回它。                     |
| [ndarray.tolistopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.tolist.html#numpy.ndarray.tolist)() | 将数组作为a.ndim-levels深层嵌套的Python标量列表返回。        |
| [ndarray.itemsetopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.itemset.html#numpy.ndarray.itemset)(*args) | 将标量插入数组（如果可能，将标量转换为数组的dtype）          |
| [ndarray.tostringopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.tostring.html#numpy.ndarray.tostring)([order]) | 构造包含数组中原始数据字节的Python字节。                     |
| [ndarray.tobytesopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.tobytes.html#numpy.ndarray.tobytes)([order]) | 构造包含数组中原始数据字节的Python字节。                     |
| [ndarray.tofileopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.tofile.html#numpy.ndarray.tofile)(fid[, sep, format]) | 将数组作为文本或二进制写入文件（默认）。                     |
| [ndarray.dumpopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.dump.html#numpy.ndarray.dump)(file) | 将数组的pickle转储到指定的文件。                             |
| [ndarray.dumpsopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.dumps.html#numpy.ndarray.dumps)() | 以字符串形式返回数组的pickle。                               |
| [ndarray.astypeopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.astype.html#numpy.ndarray.astype)(dtype[, order, casting, …]) | 数组的副本，强制转换为指定的类型。                           |
| [ndarray.byteswapopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.byteswap.html#numpy.ndarray.byteswap)([inplace]) | 交换数组元素的字节                                           |
| [ndarray.copyopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.copy.html#numpy.ndarray.copy)([order]) | 返回数组的副本。                                             |
| [ndarray.viewopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.view.html#numpy.ndarray.view)([dtype, type]) | 具有相同数据的数组的新视图。                                 |
| [ndarray.getfieldopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.getfield.html#numpy.ndarray.getfield)(dtype[, offset]) | 返回给定数组的字段作为特定类型。                             |
| [ndarray.setflagsopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.setflags.html#numpy.ndarray.setflags)([write, align, uic]) | 分别设置数组标志WRITEABLE，ALIGNED，（WRITEBACKIFCOPY和UPDATEIFCOPY）。 |
| [ndarray.fillopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.fill.html#numpy.ndarray.fill)(value) | 使用标量值填充数组。                                         |

### [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#形状操作)形状操作

对于重新`n`整形，调整大小和转置，单个元组参数可以用将被解释为n元组的整数替换。

| 方法                                                         | 描述                                   |
| ------------------------------------------------------------ | -------------------------------------- |
| [ndarray.reshapeopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.reshape.html#numpy.ndarray.reshape)(shape[, order]) | 返回包含具有新形状的相同数据的数组。   |
| [ndarray.resizeopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.resize.html#numpy.ndarray.resize)(new_shape[, refcheck]) | 就地更改数组的形状和大小。             |
| [ndarray.transposeopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.transpose.html#numpy.ndarray.transpose)(*axes) | 返回轴转置的数组视图。                 |
| [ndarray.swapaxesopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.swapaxes.html#numpy.ndarray.swapaxes)(axis1, axis2) | 返回数组的视图，其中axis1和axis2互换。 |
| [ndarray.flattenopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.flatten.html#numpy.ndarray.flatten)([order]) | 将折叠的数组的副本返回到一个维度。     |
| [ndarray.ravelopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.ravel.html#numpy.ndarray.ravel)([order]) | 返回一个扁平的数组。                   |
| [ndarray.squeezeopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.squeeze.html#numpy.ndarray.squeeze)([axis]) | 从形状除去单维输入一个。               |

### [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#项目选择和操作)项目选择和操作

对于采用 *axis* 关键字的数组方法，默认为 `None`。 如果axis为 *None* ，则将数组视为1-D数组。 *轴的* 任何其他值表示操作应继续进行的维度。

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [ndarray.takeopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.take.html#numpy.ndarray.take)(indices[, axis, out, mode]) | 返回由给定索引处的a元素组成的数组。                          |
| [ndarray.putopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.put.html#numpy.ndarray.put)(indices, values[, mode]) | 为索引中的所有n设置。a.flat[n] = values[n]                   |
| [ndarray.repeatopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.repeat.html#numpy.ndarray.repeat)(repeats[, axis]) | 重复数组的元素。                                             |
| [ndarray.chooseopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.choose.html#numpy.ndarray.choose)(choices[, out, mode]) | 使用索引数组从一组选项中构造新数组。                         |
| [ndarray.sortopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.sort.html#numpy.ndarray.sort)([axis, kind, order]) | 对数组进行就地排序。                                         |
| [ndarray.argsortopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.argsort.html#numpy.ndarray.argsort)([axis, kind, order]) | 返回将对此数组进行排序的索引。                               |
| [ndarray.partitionopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.partition.html#numpy.ndarray.partition)(kth[, axis, kind, order]) | 重新排列数组中的元素，使得第k个位置的元素值位于排序数组中的位置。 |
| [ndarray.argpartitionopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.argpartition.html#numpy.ndarray.argpartition)(kth[, axis, kind, order]) | 返回将对此数组进行分区的索引。                               |
| [ndarray.searchsortedopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.searchsorted.html#numpy.ndarray.searchsorted)(v[, side, sorter]) | 查找应在其中插入v的元素以维护顺序的索引。                    |
| [ndarray.nonzeroopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.nonzero.html#numpy.ndarray.nonzero)() | 返回非零元素的索引。                                         |
| [ndarray.compressopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.compress.html#numpy.ndarray.compress)(condition[, axis, out]) | 沿给定轴返回此数组的选定切片。                               |
| [ndarray.diagonalopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.diagonal.html#numpy.ndarray.diagonal)([offset, axis1, axis2]) | 返回指定的对角线。                                           |

### [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#计算)计算

其中许多方法都采用名为 *axis* 的参数。在这种情况下，

- 如果 *axis* 为 *None* （默认值），则将数组视为1-D数组，并对整个数组执行操作。 如果self是0维数组或数组标量，则此行为也是默认行为。 （数组标量是类型/类float32，float64等的实例，而0维数组是包含恰好一个数组标量的ndarray实例。）
- 如果 *axis* 是整数，则操作在给定轴上完成（对于可沿给定轴创建的每个1-D子数组）。

尺寸为 3 x 3 x 3 的三维数组，在其三个轴中的每个轴上求和

```python
>>> x
array([[[ 0,  1,  2],
        [ 3,  4,  5],
        [ 6,  7,  8]],
       [[ 9, 10, 11],
        [12, 13, 14],
        [15, 16, 17]],
       [[18, 19, 20],
        [21, 22, 23],
        [24, 25, 26]]])
>>> x.sum(axis=0)
array([[27, 30, 33],
       [36, 39, 42],
       [45, 48, 51]])
>>> # for sum, axis is the first keyword, so we may omit it,
>>> # specifying only its value
>>> x.sum(0), x.sum(1), x.sum(2)
(array([[27, 30, 33],
        [36, 39, 42],
        [45, 48, 51]]),
 array([[ 9, 12, 15],
        [36, 39, 42],
        [63, 66, 69]]),
 array([[ 3, 12, 21],
        [30, 39, 48],
        [57, 66, 75]]))
```

参数 *dtype* 指定应该进行简化操作（如求和）的数据类型。 默认的reduce数据类型与 *self* 的数据类型相同。 为避免溢出，使用更大的数据类型执行缩减可能很有用。

对于多种方法，还可以提供可选的 *out* 参数，并将结果放入给定的输出数组中。 该 *out* 参数必须是[`ndarray`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.html#numpy.ndarray)与具有相同数目的元素。 它可以具有不同的数据类型，在这种情况下将执行转换。

| 方法                                                         | 描述                                      |
| ------------------------------------------------------------ | ----------------------------------------- |
| [ndarray.maxopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.max.html#numpy.ndarray.max)([axis，out，keepdims，initial，...]） | 沿给定轴返回最大值。                      |
| [ndarray.argmaxopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.argmax.html#numpy.ndarray.argmax)([axis, out]) | 返回给定轴上的最大值的索引。              |
| [ndarray.minopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.min.html#numpy.ndarray.min)([axis，out，keepdims，initial，...]) | 沿给定轴返回最小值。                      |
| [ndarray.argminopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.argmin.html#numpy.ndarray.argmin)([axis, out]) | 返回最小值的索引沿给定轴线一个。          |
| [ndarray.ptpopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.ptp.html#numpy.ndarray.ptp)([axis, out, keepdims]) | 沿给定轴的峰峰值（最大值 - 最小值）。     |
| [ndarray.clipopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.clip.html#numpy.ndarray.clip)([min，max，out]) | 返回值限制为的数组。[min, max]            |
| [ndarray.conjopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.conj.html#numpy.ndarray.conj)() | 复合共轭所有元素。                        |
| [ndarray.roundopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.round.html#numpy.ndarray.round)([decimals, out]) | 返回a，每个元素四舍五入到给定的小数位数。 |
| [ndarray.traceopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.trace.html#numpy.ndarray.trace)([offset, axis1, axis2, dtype, out]) | 返回数组对角线的总和。                    |
| [ndarray.sumopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.sum.html#numpy.ndarray.sum)([axis, dtype, out, keepdims, …]) | 返回给定轴上的数组元素的总和。            |
| [ndarray.cumsumopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.cumsum.html#numpy.ndarray.cumsum)([axis, dtype, out]) | 返回给定轴上元素的累积和。                |
| [ndarray.meanopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.mean.html#numpy.ndarray.mean)([axis, dtype, out, keepdims]) | 返回给定轴上数组元素的平均值。            |
| [ndarray.varopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.var.html#numpy.ndarray.var)([axis, dtype, out, ddof, keepdims]) | 返回给定轴的数组元素的方差。              |
| [ndarray.stdopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.std.html#numpy.ndarray.std)([axis, dtype, out, ddof, keepdims]) | 返回沿给定轴的数组元素的标准偏差。        |
| [ndarray.prodopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.prod.html#numpy.ndarray.prod)([axis, dtype, out, keepdims, …]) | 返回给定轴上的数组元素的乘积              |
| [ndarray.cumprodopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.cumprod.html#numpy.ndarray.cumprod)([axis, dtype, out]) | 返回沿给定轴的元素的累积乘积。            |
| [ndarray.allopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.all.html#numpy.ndarray.all)([axis, out, keepdims]) | 如果所有元素都计算为True，则返回True。    |
| [ndarray.anyopen in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.any.html#numpy.ndarray.any)([axis, out, keepdims]) | 如果任何元素，则返回true 一个评估为True。 |

## [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#算术、矩阵乘法和比较运算)算术、矩阵乘法和比较运算

算术和比较操作[`ndarrays`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.html#numpy.ndarray) 被定义为逐元素操作，并且通常将 [`ndarray`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.html#numpy.ndarray)对象作为结果产生。

每个算术运算（的`+`，`-`，`*`，`/`，`//`， `%`，`divmod()`，`**`或`pow()`，`<<`，`>>`，`&`， `^`，`|`，`~`）和比较（`==`，`<`，`>`， `<=`，`>=`，`!=`）等效于相应的通用功能（或[ufuncopen in new window](https://numpy.org/devdocs/glossary.html#term-ufunc)的简称）中NumPy的。有关更多信息，请参阅[通用功能](https://www.numpy.org.cn/reference/arrays/ufuncs.html#ufuncs)部分。

比较运算符：

| 方法                                                         | 描述              |
| ------------------------------------------------------------ | ----------------- |
| [ndarray._*lt*_open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__lt__.html#numpy.ndarray.__lt__)(self, value, /) | 返回 self<value.  |
| [ndarray._*le*_open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__le__.html#numpy.ndarray.__le__)(self, value, /) | 返回 self<=value. |
| [ndarray._*gt*_open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__gt__.html#numpy.ndarray.__gt__)(self, value, /) | 返回 self>value.  |
| [ndarray._*ge*_open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__ge__.html#numpy.ndarray.__ge__)(self, value, /) | 返回 self>=value. |
| [ndarray._*eq*_open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__eq__.html#numpy.ndarray.__eq__)(self, value, /) | 返回 self==value. |
| [ndarray._*ne*_open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__ne__.html#numpy.ndarray.__ne__)(self, value, /) | 返回 self!=value. |

array（`bool`）的真值：

| 方法                                                         | 描述      |
| ------------------------------------------------------------ | --------- |
| [ndarray.__bool__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__bool__.html#numpy.ndarray.__bool__)(self, /) | self != 0 |

注意

数组的真值测试会调用 [`ndarray.\_\_bool__`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__bool__.html#numpy.ndarray.__bool__)，如果数组中的元素数大于1，则会引发错误，因为此类数组的真值是不明确的。使用[`.any()`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.any.html#numpy.ndarray.any)而 [`.all()`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.all.html#numpy.ndarray.all)不是清楚这种情况下的含义。（如果元素数为0，则数组的计算结果为`False`。）

一元操作：

| 方法                                                         | 描述  |
| ------------------------------------------------------------ | ----- |
| [ndarray.__neg__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__neg__.html#numpy.ndarray.__neg__)(self, /) | -self |
| [ndarray.__pos__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__pos__.html#numpy.ndarray.__pos__)(self, /) | +self |
| [ndarray.__abs__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__abs__.html#numpy.ndarray.__abs__)(self) |       |
| [ndarray.__invert__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__invert__.html#numpy.ndarray.__invert__)(self, /) | ~self |

算术：

| 方法                                                         | 描述                        |
| ------------------------------------------------------------ | --------------------------- |
| [ndarray.__add__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__add__.html#numpy.ndarray.__add__)(self, value, /) | 返回 self+value.            |
| [ndarray.__sub__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__sub__.html#numpy.ndarray.__sub__)(self, value, /) | 返回 self-value.            |
| [ndarray.__mul__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__mul__.html#numpy.ndarray.__mul__)(self, value, /) | 返回 self*value.            |
| [ndarray.__truediv__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__truediv__.html#numpy.ndarray.__truediv__)(self, value, /) | 返回 self/value.            |
| [ndarray.__floordiv__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__floordiv__.html#numpy.ndarray.__floordiv__)(self, value, /) | 返回 self//value.           |
| [ndarray.__mod__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__mod__.html#numpy.ndarray.__mod__)(self, value, /) | 返回 self%value.            |
| [ndarray.__divmod__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__divmod__.html#numpy.ndarray.__divmod__)(self, value, /) | 返回 divmod(self, value).   |
| [ndarray.__pow__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__pow__.html#numpy.ndarray.__pow__)(self, value[, mod]) | 返回 pow(self, value, mod). |
| [ndarray.__lshift__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__lshift__.html#numpy.ndarray.__lshift__)(self, value, /) | 返回 self<<value.           |
| [ndarray.__rshift__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__rshift__.html#numpy.ndarray.__rshift__)(self, value, /) | 返回 self>>value.           |
| [ndarray.__and__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__and__.html#numpy.ndarray.__and__)(self, value, /) | 返回 self&value.            |
| [ndarray.__or__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__or__.html#numpy.ndarray.__or__)(self, value, /) | 返回 self                   |
| [ndarray.__xor__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__xor__.html#numpy.ndarray.__xor__)(self, value, /) | 返回 self^value.            |

注意

- [`pow`open in new window](https://docs.python.org/dev/library/functions.html#pow)默认忽略任何第三个参数， 因为底层[`ufunc`open in new window](https://numpy.org/devdocs/reference/generated/numpy.power.html#numpy.power)只接受两个参数。
- 三个划分算子都是定义的; `div`默认情况下`truediv`处于活动状态， 当[`__future__`open in new window](https://docs.python.org/dev/library/__future__.html#module-__future__)分割生效时处于活动状态。
- 因为[`ndarray`open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.html#numpy.ndarray)是内置类型（用C编写）， 所以 `__r{op}__` 不直接定义特殊方法。
- 可以使用调用为数组实现许多算术特殊方法的函数[`__array_ufunc__`](https://www.numpy.org.cn/reference/arrays/classes.html#numpy.class.__array_ufunc__)。

就地算术运算方法：

| 方法                                                         | 描述                |
| ------------------------------------------------------------ | ------------------- |
| [ndarray.__iadd__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__iadd__.html#numpy.ndarray.__iadd__)(self, value, /) | 返回 self+=value。  |
| [ndarray.__isub__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__isub__.html#numpy.ndarray.__isub__)(self, value, /) | 返回 self==value。  |
| [ndarray.__imul__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__imul__.html#numpy.ndarray.__imul__)(self, value, /) | 返回 self*=value。  |
| [ndarray.__itruediv__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__itruediv__.html#numpy.ndarray.__itruediv__)(self, value, /) | 返回 self/=value。  |
| [ndarray.__ifloordiv__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__ifloordiv__.html#numpy.ndarray.__ifloordiv__)(self, value, /) | 返回 self//=value。 |
| [ndarray.__imod__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__imod__.html#numpy.ndarray.__imod__)(self, value, /) | 返回 self％=value。 |
| [ndarray.__ipow__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__ipow__.html#numpy.ndarray.__ipow__)(self, value, /) | 返回 self**=value。 |
| [ndarray.__ilshift__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__ilshift__.html#numpy.ndarray.__ilshift__)(self, value, /) | 返回 self<<=value。 |
| [ndarray.__irshift__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__irshift__.html#numpy.ndarray.__irshift__)(self, value, /) | 返回 self>>=value。 |
| [ndarray.__iand__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__iand__.html#numpy.ndarray.__iand__)(self, value, /) | 返回 self&=value。  |
| [ndarray.__ior__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__ior__.html#numpy.ndarray.__ior__)(self, value, /) | 返回 self           |
| [ndarray.__ixor__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__ixor__.html#numpy.ndarray.__ixor__)(self, value, /) | 返回 self^=value。  |

警告

就地操作将使用由两个操作数的数据类型决定的精度来执行计算，但会悄悄地向下转换结果（如果需要）， 以便它可以重新适应数组。 因此，对于混合精度计算，`A {op} = B` 可以不同于 `A = A {op} B`。例如，假设 `a = ones(3，3)`。 然后，`a += 3j` 与 `a = a + 3j` 不同：当它们都执行相同的计算时，`a += 3` 将结果强制转换为适合 `a` ，而 `a = a+3j` 将名称 `a` 重新绑定到结果。

矩阵乘法：

| 方法                                                         | 描述                                   |
| ------------------------------------------------------------ | -------------------------------------- |
| [ndarray.__matmul__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__matmul__.html#numpy.ndarray.__matmul__)(self, value, /) | 返回 [self@value](mailto:self@value)。 |

注意

Matrix 运算符 `@` 和 `@=` 是在PEP465之后的Python 3.5中引入的。NumPy 1.10.0为测试目的初步实现了 `@`。 进一步的文档可以在 [`matmul`open in new window](https://numpy.org/devdocs/reference/generated/numpy.matmul.html#numpy.matmul) 文档中找到。

## [#](https://www.numpy.org.cn/reference/arrays/ndarray.html#特殊方法)特殊方法

对于标准库函数：

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [ndarray.__copy__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__copy__.html#numpy.ndarray.__copy__)() | 如果使用的[copy.copyopen in new window](https://docs.python.org/dev/library/copy.html#copy.copy)是所谓的数组上。 |
| [ndarray.__deepcopy__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__deepcopy__.html#numpy.ndarray.__deepcopy__)() | 如果使用的[copy.deepcopyopen in new window](https://docs.python.org/dev/library/copy.html#copy.deepcopy)是所谓的数组上。 |
| [ndarray.__reduce__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__reduce__.html#numpy.ndarray.__reduce__)() | 用于 pickling。                                              |
| [ndarray.__setstate__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__setstate__.html#numpy.ndarray.__setstate__)（州，/） | 用于unpickling。                                             |

基本定制：

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [ndarray.__new__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__new__.html#numpy.ndarray.__new__)(*args, **kwargs) | 创建并返回一个新对象。                                       |
| [ndarray.__array__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__array__.html#numpy.ndarray.__array__)() | 如果没有给出dtype，则返回对self的新引用;如果dtype与数组的当前dtype不同，则返回提供的数据类型的新数组。 |
| [ndarray.__array_wrap__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__array_wrap__.html#numpy.ndarray.__array_wrap__)() |                                                              |

容器定制:（参见[索引](https://www.numpy.org.cn/reference/arrays/indexing.html)）

| 方法                                                         | 描述                       |
| ------------------------------------------------------------ | -------------------------- |
| [ndarray.__len__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__len__.html#numpy.ndarray.__len__)(self, /) | 返回 len(self)。           |
| [ndarray.__getitem__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__getitem__.html#numpy.ndarray.__getitem__)(self, key, /) | 返回 self[key]。           |
| [ndarray.__setitem__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__setitem__.html#numpy.ndarray.__setitem__)(self, key, value, /) | 将 self[key] 设置为value。 |
| [ndarray.__contains__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__contains__.html#numpy.ndarray.__contains__)(self, key, /) | 返回 self 的 key。         |

转换; 操作`int`，`float` 和 `complex`。 它们仅适用于其中包含一个元素的数组，并返回相应的标量。

| 方法                                                         | 描述 |
| ------------------------------------------------------------ | ---- |
| [ndarray.__int__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__int__.html#numpy.ndarray.__int__)(self) | none |
| [ndarray.__float__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__float__.html#numpy.ndarray.__float__)(self) | none |
| [ndarray.__complex__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__complex__.html#numpy.ndarray.__complex__)() | none |

字符串表示：

| 方法                                                         | 描述              |
| ------------------------------------------------------------ | ----------------- |
| [ndarray.__str__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__str__.html#numpy.ndarray.__str__)(self, /) | 返回 str(self)。  |
| [ndarray.__repr__open in new window](https://numpy.org/devdocs/reference/generated/numpy.ndarray.__repr__.html#numpy.ndarray.__repr__)(self, /) | 返回 repr(self)。 |