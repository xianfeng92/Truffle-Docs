# 快速开始

下面是关于创建truffle项目和在区块链上部署智能合约的基础知识。

## 创建一个项目

大多数Truffle命令是要在现有的Truffle项目中才能使用，所以第一步是创建一个truffle项目：

使用　truffle init　可以创建一个全新的项目，但对于刚入门的人，我们可以使用Truffle Boxes，使用它可以创建一些官方提供的示例程序，这样我们可以快速熟悉truffle项目结构。下面我们就以MetaCoin为例，它创建一个可以在帐户之间转移的Token：

1. 新建一个目录，并进入到目录中：

> mkdir MetaCoin

> cd MetaCoin

2. 下载MetaCoin模板：

> truffle unbox metacoin

Note：使用truffle unbox <box-name>命令下载指定的Truffle Box。

完成此操作后，得到如下项目结构：

* contracts/: Solidity合约目录

* migrations/: 合约部署的脚本文件目录

* test/: 用于应用程序和合约的测试文件目录

* truffle.js: Truffle配置文件目录


## 项目轮廓

1. 在文本编辑器中打开contract/MetaCoin.sol文件。这是一个智能合约（用Solidity编写），用于创建MetaCoin Token。请注意，该合约中引用了同一目录中的另一个Solidity文件contract/ConvertLib.sol。

2. 打开contract/Migrations.sol文件。这是一个单独的Solidity文件，用于管理和更新已部署的智能合约的状态。此文件随每个Truffle项目一起提供，通常不需要修改其内容。

3. 打开migrations/1_initial_deployment.js文件。此文件是Migrations.sol合约的部署脚本。

4. 打开migrations/2_deploy_contracts.js文件。此文件是MetaCoin合约的部署脚本。（部署脚本按顺序运行，因此以2开头的文件将在以1开头的文件后运行）

5. 打开test/TestMetacoin.sol文件。这是一个用Solidity编写的测试文件，可确保您的合约按预期工作。

6. 打开test/metacoin.js文件。这是一个用JavaScript编写的测试文件，它执行与上面的Solidity测试类似的功能。

7. 打开truffle.js文件。这是Truffle配置文件，用于网络信息和其他与项目相关的设置。


## 测试

1. 运行Solidity测试：

> truffle test ./test/TestMetacoin.sol

会看到以下输出:

>   TestMetacoin
>    √ testInitialBalanceUsingDeployedContract (71ms)
>    √ testInitialBalanceWithNewMetaCoin (59ms)
>   2 passing (794ms)


2. 运行JavaScript测试：

> truffle test ./test/metacoin.js

您将看到以下输出:

> Contract: MetaCoin
>    √ should put 10000 MetaCoin in the first account
>    √ should call a function that depends on a linked library (40ms)
>    √ should send coin correctly (129ms)
> 3 passing (255ms)


## 编译

1. 编译智能合约：

> truffle compile

 会看到以下输出：

> Compiling .\contracts\ConvertLib.sol...
> Compiling .\contracts\MetaCoin.sol...
> Compiling .\contracts\Migrations.sol...
> Writing artifacts to .\build\contracts


2. 使用Truffle Develop 部署

当我们部署智能合约时，需要连接到以太坊的区块链网络。 __Truffle有一个内置的可用于测试的个人区块链网络__ 。此区块链网络只存在您系统的本地，不与主以太坊网络交互。


下面将使用Truffle Develop创建区块链网络并与其进行交互。

1. 运行Truffle Develop:

> truffle develop

将看到以下信息：

```
Truffle Develop started at http://127.0.0.1:9545/

Accounts:
(0) 0x627306090abab3a6e1400e9345bc60c78a8bef57
(1) 0xf17f52151ebef6c7334fad080c5704d77216b732
(2) 0xc5fdf4076b8f3a5357c5e395ab970b5b54098fef
(3) 0x821aea9a577a9b44299b9c15c88cf3087f3b5544
(4) 0x0d1d4e623d10f9fba5db95830f7d3839406c6af2
(5) 0x2932b7a2355d6fecc4b5c0b6bd44cc31df247a2e
(6) 0x2191ef87e392377ec08e7c08eb105ef5448eced5
(7) 0x0f4f2ac550a1b4e2280d04c21cea7ebd822934b5
(8) 0x6330a553fc93768f612722bb8c2ec78ac90b3bbc
(9) 0x5aeda56215b167893e80b4fe645ba6d5bab767de

Private Keys:
(0) c87509a1c067bbde78beb793e6fa76530b6382a4c0241e5e4a9ec0a0f44dc0d3
(1) ae6ae8e5ccbfb04590405997ee2d52d2b330726137b875053c36d94e974d162f
(2) 0dbbe8e4ae425a6d2687f1a7e3ba17bc98c673636790f1b8ad91193c05875ef1
(3) c88b703fb08cbea894b6aeff5a544fb92e78a18e19814cd85da83b71f772aa6c
(4) 388c684f0ba1ef5017716adb5d21a053ea8e90277d0868337519f97bede61418
(5) 659cbb0e2411a44db63778987b1e22153c086a95eb6b18bdf89de078917abc63
(6) 82d052c865f5763aad42add438569276c00d3d88a2d062d36b2bae914d58b8c8
(7) aa3680d5d48a8283413f7a108367c7299ca73f553735860a87b08f39395618b7
(8) 0f62d96d6675f32685bbdb8ac13cda7c23436f63efbb9d07700d8669ff12b7c4
(9) 8d5366123cb560bb606379f90a0bfd4769eecc0557f1b362dcae9012b548b1e5

Mnemonic: candy maple cake sugar pudding cream honey rich smooth crumble sweet treat

⚠️  Important ⚠️  : This mnemonic was created for you by Truffle. It is not secure.
Ensure you do not use it on production blockchains, or else you risk losing funds. 

truffle(development)>

```

上面的输出信息是系统分配的账户地址以及其所对应的私钥。


在Truffle Develop中，运行Truffle命令时可以省略truffle前缀。例如，要运行truffle migrate，只需要键入migrate，其可将已编译的合同部署到区块链上，因此在提示符下键入：

> migrate

将看到以下输出：

```

Using network 'develop'.

Running migration: 1_initial_migration.js
  Deploying Migrations...
  ... 0x63b393bd50251ec5aa3e159070609ee7c61da55531ff5dea5b869e762263cb90
  Migrations: 0x8cdaf0cd259887258bc13a92c0a6da92698644c0
Saving successful migration to network...
  ... 0xd7bc86d31bee32fa3988f1c1eabce403a1b5d570340a3a9cdba53a472ee8c956
Saving artifacts...
Running migration: 2_deploy_contracts.js
  Deploying ConvertLib...
  ... 0xa59221bc26a24f1a2ee7838c36abdf3231a2954b96d28dd7def7b98bbb8a7f35
  ConvertLib: 0x345ca3e014aaf5dca488057592ee47305d9b3e10
  Linking ConvertLib to MetaCoin
  Deploying MetaCoin...
  ... 0x1cd9e2a790f4795fa40205ef58dbb061065ca235bee8979a705814f1bc141fd5
  MetaCoin: 0xf25186b5081ff5ce73482ad761db0eb0d25abfbf
Saving successful migration to network...
  ... 0x059cf1bbc372b9348ce487de910358801bbbd1c89182853439bec0afaee6c7db
Saving artifacts...

```

上面的输出内容为交易ID和合约的部署地址。


## 替代方案：GANACHE

除了使用Truffle Develop这个一体化的个人区块链和控制台，我们还可以使用桌面应用程序Ganache来启动您的个人区块链。 对于那些刚接触以太坊和区块链的人来说，Ganache可以是一个更容易理解的工具，因为它可以预先显示更多关于区块链的信息。

使用Ganache，我们需要编辑Truffle的配置文件networks,使其指向Ganache实例。

1. 下载并安装Ganache

2. 在文本编辑器中打开truffle.js，添加以下内容：

```
module.exports = {
  networks: {
    development: {
      host: "127.0.0.1",
      port: 7545,
      network_id: "*"
    }
  }
};

```

Ganache的默认地址为：127.0.0.1:7545,修改truffle.js使其指向Ganache实例。

3. 保存并关闭该文件

4. 启动Ganache

5. 在终端上，将合同部署到Ganache所创建的区块链上：

> truffle migrate

将会看到以下输出:

```
Using network 'development'.

Running migration: 1_initial_migration.js
  Replacing Migrations...
  ... 0x63b393bd50251ec5aa3e159070609ee7c61da55531ff5dea5b869e762263cb90
  Migrations: 0xd6d1ea53b3a7dae2424a0525d6b1754045a0df9f
Saving successful migration to network...
  ... 0xe463b4cb6a3bbba06ab36ac4d7ce04e2a220abd186c8d2bde092c3d5b2217ed6
Saving artifacts...
Running migration: 2_deploy_contracts.js
  Replacing ConvertLib...
  ... 0xa59221bc26a24f1a2ee7838c36abdf3231a2954b96d28dd7def7b98bbb8a7f35
  ConvertLib: 0x33b217190208f7b8d2b14d7a30ec3de7bd722ac6
  Replacing MetaCoin...
  ... 0x5d51f5dc05e5d926323d580559354ad39035f16db268b91b6db5c7baddef5de5
  MetaCoin: 0xcd2c65cc0b498cb7a3835cfb1e283ccd25862086
Saving successful migration to network...
  ... 0xeca6515f3fb47a477df99c3389d3452a48dfe507980bfd29a3c57837d6ef55c5
Saving artifacts...

```

上面的内容只要是交易的id以及合约部署的地址。


6. 在Ganache中，单击“Transaction”按钮可以查看Transaction是否已处理。

7. 我们可以使用Truffle控制台与合约进行交互。Truffle控制台将会连接到由Ganache生成的区块链网络。

> truffle console

您将看到以下提示：

> truffle(development)>

## 与合约的交互

通过以下方式使用控制台与合约交互： 

* 检查合约帐户的metacoin余额：

> MetaCoin.deployed().then(function(instance){return instance.getBalance(web3.eth.accounts[0]);}).then(function(value){return value.toNumber()});

* 查看这个余额的价值是多少：

> MetaCoin.deployed().then(function(instance){return instance.getBalanceInEth(web3.eth.accounts[0]);}).then(function(value){return value.toNumber()});

* 将一些metacoin从一个帐户转移到另一个帐户：

> MetaCoin.deployed().then(function(instance){return instance.sendCoin(web3.eth.accounts[1], 500);});

* 检查metacoin接收者的帐户余额：

> MetaCoin.deployed().then(function(instance){return instance.getBalance(web3.eth.accounts[1]);}).then(function(value){return value.toNumber()});

* 检查metacoin发送者的帐户的余额：

> MetaCoin.deployed().then(function(instance){return instance.getBalance(web3.eth.accounts[0]);}).then(function(value){return value.toNumber()});



























