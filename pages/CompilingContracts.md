## 位置

我们的所有合约文件都位于项目的contracts/目录中。由于合约是用Solidity编写的，合约都将以.sol为文件扩展名。相关的Solidity库也是以.sol为文件扩展名。

通过truffle init创建的项目中都会有一个用于合约部署的Migrations.sol文件。如果我们使用的是truffle unbox，那么将会得到多个部署文件。

## 命令

要编译Truffle项目，需要切换到项目的根目录，然后在终端中键入以下内容：

> truffle compile

首次compile时，将编译所有合同。在后续compile中，Truffle__将仅编译自上次编译以来已更改的合同__。如果要覆盖此行为，请在上述的命令中加上--all选项。


## ARTIFACTS（编译产物）

编译生成的产物将放在build/contracts目录中（如果该目录不存在，将创建该目录）。这些产物是Truffle内部工作的组成部分，它们在成功部署应用程序中起着重要作用。您不应编辑这些文件，因为它们将被合同编译和部署所覆盖。

## 依赖

使用Solidity的import命令声明合约的依赖项。Truffle将以正确的顺序编译合同，并确保将所有依赖项发送给编译器。可以通过两种方式指定依赖关系：

1. 通过文件名导入依赖

要从单独的文件导入合约，请将以下代码添加到Solidity源文件中：

> import "./AnotherContract.sol";

这将使AnotherContract.sol中的所有合约变量都可用。这里，AnotherContract.sol的路径是相对于当前合约的路径。

2. 从外部包中导入依赖

Truffle支持从EthPM和NPM安装的包中导入依赖，使用语法：

> import "somepackage/SomeContract.sol";

这里，somepackage表示通过EthPM或NPM安装的包，SomeContract.sol表示该包提供的Solidity源文件。

需要注意的是，Truffle是先从EthPM中搜索已安装的软件包，然后再从NPM中搜索已安装的软件包。因此在极少数情况下会出现命名冲突，此时会使用通过EthPM安装的软件包。

