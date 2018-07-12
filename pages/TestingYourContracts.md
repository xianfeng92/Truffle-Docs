# Testing your contracts

## Framework

Truffle标配了自动化的测试框架，简化了合约的测试。此框架允许我们以两种不同的方式编写简单且可管理的测试：

* In Javascript

* In Solidity

两种类型的测试都有其优点和缺点。有关每一部分的讨论，参阅接下面的两节。

## Location

所有测试文件都应位于./test目录中。 Truffle只会运行具有以下文件扩展名的测试文件：.js，.es，.es6和.jsx以及.sol。 所有其他文件都将被忽略。

## Command

要运行所有测试，只需运行：

> truffle test

或者，您可以运行在指定路径的特定文件，例如:

> truffle test ./path/to/test/file.js

## Clean-room environment

Truffle在运行测试文件时提供洁净室环境。 在针对Ganache或Truffle Develop运行测试时，Truffle将使用高级快照功能来确保您的测试文件不会彼此共享状态。 当与go-ethereum等其他Ethereum客户端运行时，Truffle将在开始每个文件的测试前重新部署　migrations，以确保测试合约的状态是最新。

## Speed and reliability considerations

在运行自动化测试时，Ganache和Truffle Develop都比其他客户快得多。 此外，它们还包含特殊功能，Truffle利用这些功能将测试运行时间提高了近90％。 作为一般工作流程，建议在正常开发和测试期间使用Ganache或Truffle Develop，然后在准备部署到实时或生产网络时，针对go-ethereum或其他官方Ethereum客户端再测试一次。

















