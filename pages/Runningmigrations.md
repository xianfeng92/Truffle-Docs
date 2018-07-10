# 部署

部署脚本一般为js格式，它们可将对应合约部署到以太坊网络。这些部署脚本可以暂存已经部署的合约，并且假定我们的部署需求会随着时间的推移而改变。 随着项目的推进，我们可以创建新的部署脚本来部署新的合约，以更新我们的项目功能。先前部署合约的历史记录通过__特殊的部署合约（Migration.sol）__记录在链上，详细信息如下所述。

## 命令

合约部署的命令：

> truffle migrate

这个命令将会运行migrations文件夹下的所有部署脚本。简单的说，migrations文件夹主要是管理一组合约的部署脚本。如果你之前成功部署过，那么Truffle将从__上次运行到的部署脚本__开始执行合约的部署,__只会运行新添加的部署脚本__。如果部署脚本没有更新，那么Truffle将不会执行任何操作。你可以使用　--reset　选项去运行所有的部署脚本。对于本地测试，确保在部署合约之前必须安装和运行GANACH等测试区块链。

## 部署脚本

一个简单的部署脚本是这样的：

> Filename: 4_example_migration.js

```
var MyContract = artifacts.require("MyContract");

module.exports = function(deployer) {
  // deployment steps
  deployer.deploy(MyContract);
};

```

需要注意，部署脚本名的前缀有一个数字，后缀是一个描述。使用数字编号是为了记录部署是否成功。后缀纯粹是便于我们的理解。

## ARTIFACTS.REQUIRE()

在开始部署的时候，告诉Truffle我们想通过artifacts.require（）方法与合约进行交互，这个方法类似于Node的require，但在我们的例子中，它返回了我们可以在其余部署脚本中使用的一个合约的抽象。指定的名称应与该源文件中的合约定义的名称相同。不要传递源文件的名称，因为一个源文件可以包含多个合约的定义。


考虑这个例子，在同一个源文件中定义了两个合约：

> Filename: ./contracts/Contracts.sol

```
contract ContractOne {
  // ...
}

contract ContractTwo {
  // ...
}

```

只使用 ContractTwo：

> var ContractTwo = artifacts.require("ContractTwo");


ContractOne和ContractTwo都使用：

> var ContractOne = artifacts.require("ContractOne");
> var ContractTwo = artifacts.require("ContractTwo");

## MODULE.EXPORTS

所有部署都必须通过MODULE.EXPORTS来导出函数。每个合约导出的函数应该接受部署器对象（deployer object）作为其第一个参数。这个对象在部署中既提供了一个清晰的语法来部署智能合约，也执行一些其它工作，例如保存部署的artifacts供以后使用。部署器对象是暂存部署任务的主要接口，其API在页面底部会有描述。

artifacts.require函数也可以接受其他参数。


## 初始化部署

Tunffle要求我们必须要有一个 Migrations 合约，以便使用 Migrations特性。这个合约必须包含一个特定的接口，但是你可以随意编辑它。对于大多数项目，该合约只会被部署一次。在使用Tunffle init创建新项目时即得到这个Migrations合约。

> Filename: contracts/Migrations.sol

```
pragma solidity ^0.4.8;

contract Migrations {
  address public owner;

  // A function with the signature `last_completed_migration()`, returning a uint, is required.
  uint public last_completed_migration;

  modifier restricted() {
    if (msg.sender == owner) _;
  }

  function Migrations() {
    owner = msg.sender;
  }

  // A function with the signature `setCompleted(uint)` is required.
  function setCompleted(uint completed) restricted {
    last_completed_migration = completed;
  }

  function upgrade(address new_address) restricted {
    Migrations upgraded = Migrations(new_address);
    upgraded.setCompleted(last_completed_migration);
  }
}
```
第一次部署时必须部署Migrations.sol，以便利用Migrations的特性。为此，创建以下部署脚本：

> Filename: migrations/1_initial_migration.js

```
var Migrations = artifacts.require("Migrations");

module.exports = function(deployer) {
  // Deploy the Migrations contract as our only task
  deployer.deploy(Migrations);
};

```

我们可以通过增加__前缀的编号__创建新的部署脚本来部署其他合约并执行进一步的操作。

## 部署器

部署脚本将使用部署器(Deployer)来暂存部署任务。这样，我们可以同步地写入部署任务，并且将按照正确的顺序执行部署任务：

> Stage deploying A before B
> deployer.deploy(A);
> deployer.deploy(B);

或者，部署器上的每个函数都可以用作Promise，以顺序化部署任务的执行：

```
// Deploy A, then deploy B, passing in A's newly deployed address
deployer.deploy(A).then(function() {
  return deployer.deploy(B, A.address);
});

```

## 网络

我们可以依据网络（network）来进行有条件的部署，列如：

```
module.exports = function(deployer, network) {
  if (network == "live") {
    // Do something specific to the network named "live".
  } else {
    // Perform a different step otherwise.
  }
}

```

## AVAILABLE ACCOUNTS

我们可以向Migrations里面传递账户列表，以便在部署期间使用：

```
module.exports = function(deployer, network, accounts) {
  // Use the accounts within your migrations.
}

```

## DEPLOYER API

部署器中包含许多可用的函数来简化合约的部署。


* DEPLOYER.DEPLOY(CONTRACT, ARGS…, OPTIONS)

根据合约对象以及其构造器参数来部署合约，这对那些在Dapp中只存在一个实例的合约对象来说是很有用。在完成部署后会给合约一个地址，该地址将会覆盖先前已存储部署该合约得到的地址。

我们可以通过传递多个合约对象和构造器参数来部署多个合约。另外，最后一个参数是可选的对象，它可以包括名为　overwrite的key以及其他事务参数，如GAS和From。如果key overwrite　所对应的值为false,对于已经部署过的合约，部署器将不会重复部署（即覆盖已经部署的合约）。这对于某些合约地址是提供给外部做依赖的情况很有用。

注意，在调用部署之前，您需要首先部署和链接合约所依赖的库。


例如：

```
// Deploy a single contract without constructor arguments
deployer.deploy(A);

// Deploy a single contract with constructor arguments
deployer.deploy(A, arg1, arg2, ...);

// Don't deploy this contract if it has already been deployed
deployer.deploy(A, {overwrite: false});

// Set a maximum amount of gas and `from` address for the deployment
deployer.deploy(A, {gas: 4612388, from: "0x...."});

// Deploy multiple contracts, some with arguments and some without.
// This is quicker than writing three `deployer.deploy()` statements as the deployer
// can perform the deployment as a single batched request.
deployer.deploy([
  [A, arg1, arg2, ...],
  B,
  [C, arg1]
]);

// External dependency example:
//
// For this example, our dependency provides an address when we're deploying to the
// live network, but not for any other networks like testing and development.
// When we're deploying to the live network we want it to use that address, but in
// testing and development we need to deploy a version of our own. Instead of writing
// a bunch of conditionals, we can simply use the `overwrite` key.
deployer.deploy(SomeDependency, {overwrite: false});

```

## DEPLOYER.LINK(LIBRARY, DESTINATIONS)

将已部署的库链接到一个或多个合约。DESTINATIONS可以是单个或多个合约的数组。如果DESTINATIONS内的存在合约不依赖于链接的LIBRARY，该合约将被忽略。

```
// Deploy library LibA, then link LibA to contract B, then deploy B.
deployer.deploy(LibA);
deployer.link(LibA, B);
deployer.deploy(B);

// Link LibA to many contracts
deployer.link(LibA, [B, C, D]);

```

## DEPLOYER.THEN(FUNCTION() {…})

就像一个Promise，运行一个任意部署步骤。在部署的过程中使用此调用特定的合约函数来添加、编辑和重新组织合约数据。

例如：

```
var a, b;
deployer.then(function() {
  // Create a new version of A
  return A.new();
}).then(function(instance) {
  a = instance;
  // Get the deployed instance of B
  return B.deployed();
}).then(function(instance) {
  b = instance;
  // Set the new instance of A's address on B via B's setA() function.
  return b.setA(a.address);
});

```





















