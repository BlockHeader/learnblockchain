---
title: Solidity语法详解 - 类型介绍2  
date: 2017-12-06 15:25:59
categories: 以太坊
tags:
    - Solidity
author: Tiny熊
---

现在的中文Solidity文档，要么翻译的太烂，要么太旧。决定重新翻译下。

<!-- more -->

## 写在前面

Solidity是以太坊智能合约编程语言，阅读本文前，你应该对以太坊、智能合约有所了解，如果你还不知道这些概念欢迎订阅我的专栏系统学习[区块链技术](https://xiaozhuanlan.com/blockchaincore)

本文基于当前最新的Solidity0.4.20版本进行讲解，先介绍语法部分，这部分主要是对官方文档的翻译，然后是代码实例说明。

Solidity的介绍会是一系列文章，本文是第一篇介绍：Solidity的变量类型。

## 类型
Solidity是一种静态类型语言，意味着每个变量（本地或状态变量）需要在编译时指定变量的类型（或至少可以推倒出类型）。Solidity提供了一些基本类型可以用来组合成复杂类型。

Solidity类型分为两类
* 值类型(Value Type) - 变量在赋值或传参是，总是进行值拷贝。
* 引用类型(Reference Types)

## 值类型(Value Type)
**值类型**包含:
* 布尔类型(Booleans)
* 整型(Integers)
* 地址(Address)
* 定长字节数组(Fixed byte arrays)

* 有理数和整型(Rational and Integer Literals，String literals)
* 枚举类型(Enums)
* 函数(Function Types)

### 布尔类型(Booleans)
**布尔(bool)**:可能的取值为常量值**true**和**false**。

布尔类型支持的运算符有：
* ！逻辑非
* && 逻辑与
* || 逻辑或
* == 等于
* != 不等于

注意：运算符**&&**和**||**是短路运算符，如f(x)||g(y)，当f(x)为真时，则不会继续执行g(y)。

### 整型(Integers)
**int**/**uint**: 表示有符号和无符号不同位数整数。支持关键字**uint8** 到 **uint256** (以8步进)，
**uint** 和 **int** 默认对应的是 **uint256** 和 **int256**。

支持的运算符：
* 比较运算符： <=, < , ==, !=, >=, > (返回布尔值：true 或 false)
* 位操作符： &，|，^(异或)，~（位取反）
* 算术操作符：+，-，一元运算-，一元运算+，*，/, %(取余数), \***（幂）, << (左移位), >>(右移位)

说明：
1. 整数除法总是截断的，但如果运算符是字面量（字面量稍后讲)，则不会截断。
2. 整数除0会抛异常。
3. 移位运算的结果的正负取决于操作符左边的数。x << y 和 x * 2\***y 是相等， x >> y 和 x / 2\**y 是相等的。
4. 不能进行负移位，即操作符右边的数不可以为负数，否则会抛出运行时异常。


> 注意：Solidity中，右移位是和除等价的，因此右移位一个负数，向下取整时会为0，而不像其他语言里为无限负小数。


### 固定位浮点型（Fixed Point Numbers）

> 注意：固定位浮点型 Solidity（发文时）还不完全支持，它可以用来声明变量，但不可以用来赋值。
**fixed**/**ufixed**: 表示有符号和无符号的固定位浮点数。关键字为**ufixedMxN** 和 **ufixedMxN**。
**M**表示这个类型要占用的位数，以8步进，可为8到256位。
**N**表示小数点的个数，可为0到80之前

支持的运算符：
* 比较运算符： <=, < , ==, !=, >=, > (返回布尔值：true 或 false)
* 算术操作符：+，-，一元运算-，一元运算+，*，/, %(取余数)
> 注意：它和大多数语言的float和double不一样，**M**是表示整个数占用的固定位数，包含整数部分和小数部分。因此用一个小位数（M较小）来表示一个浮点数时，小数部分会几乎占用整个空间。

### 地址类型（Address）
**地址**： 20字节（一个以太坊地址的长度），地址类型也有成员，地址是所有合约的基础
支持的运算符：
* <=, <, ==, !=, >= 和 >
> 注意：从0.5.0开始，合约不再继承自地址类型，但仍然可以显式转换为地址。

#### 地址类型的成员
* balance 属性及transfer() 函数 
    balance用来查询账户余额，transfer()用来发送以太币（以wei为单位）。
    如：
    ```
    address x = 0x123;
    address myAddress = this;
    if (x.balance < 10 && myAddress.balance >= 10) x.transfer(10);
    ```
* send() 函数
    send 与transfer对应，但更底层。如果执行失败，transfer不会因异常停止，而send会返回false。
    > 警告：send() 执行有一些风险：如果调用栈的深度超过1024或gas耗光，交易都会失败。因此，为了保证安全，必须检查send的返回值，如果交易失败，会回退以太币。如果用transfer会更好。

* call(), callcode() 和 delegatecall() 函数
    为了和非ABI协议的合约进行交互，可以使用call() 函数, 它用来向另一个合约发送原始数据，支持任何类型任意数量的参数，每个参数会按规则(ABI协议)打包成32字节并一一拼接到一起。





## 参考文档
[](https://solidity.readthedocs.io/en/develop/types.html)