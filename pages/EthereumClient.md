# 以太坊客户端

有许多以太坊客户可供选择。我们推荐根据您是在开发还是部署来推荐不同的客户端。

# 开发

## GANACHE

我们建议使用Ganache，这是在桌面上运行的以太坊开发的个人区块链。使用Ganache，您可以快速了解您的应用程序如何影响区块链，并能够看到一些详细信息，如您的帐户，余额，合同创建和Gas成本。您还可以微调Ganache的挖矿，以更好地满足您的需求。Ganache适用于Windows，Mac和Linux。

Ganache启动时会运行于http://127.0.0.1:7545。它将显示前10个帐户以及用于创建这些帐户的助记符。助记符将在Ganache重新启动时保留，您也可以自定义助记符。

##　TRUFFLE DEVELOP

我们也建议使用Truffle Develop，其将开发所用的区块链环境内置于Truffle中。 Truffle Develop可帮助您使用单个命令设置集成的区块链环境。其无需安装，只需在终端中键入：

> truffle develop

其将在http://127.0.0.1:9545地址上运行客户端。它将显示前10个帐户以及用于创建这些帐户的助记符。Truffle Develop每次都使用相同的助记符：

> candy maple cake sugar pudding cream honey rich smooth crumble sweet treat

一旦启动，Truffle Develop将为您提供一个控制台，您可以使用它来运行所有可用的Truffle命令。 通过省略truffle前缀来输入这些命令。例如，要编译智能合约，您只需要键入compile。

# 部署到以太坊网络

有许多官方和非官方的以太坊客户可供您使用。 以下是一个简短列表：

* Geth (go-ethereum): https://github.com/ethereum/go-ethereum

* WebThree (cpp-ethereum): https://github.com/ethereum/cpp-ethereum

* Parity: https://github.com/paritytech/parity


这些都是完整的以太坊网络的实现，包括挖矿，网络，区块和交易处理。在使用Ganache或Truffle Develop充分测试dapp之后，您可以使用这些客户端将合约部署到所需的以太坊网络。


# 部署到私有网络

私有网络使用与以太坊主网相同的技术，但具有不同的配置。因此，您可以在以太坊客户端上配置一个私有网络，并以完全相同的方式部署到它。
