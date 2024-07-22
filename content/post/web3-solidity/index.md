---
title: "Web3 Solidity 代码实现彩票智能合约"
description: 
date: 2024-07-20T22:44:42+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---

# 前言

需求是编写第一个智能合约并发布到链上，只能合约就是使用solidity这个语言

智能合约就是一段代码，solidity就是一个类似javascript 语言，描述这段智能合约。

那么在哪里编辑这段代码呢，就是Remix

Remix 就是专门编写智能合约的 WebIDE，注意是web IDE，可以在浏览器环境中编写智能合约。  

以太坊智能合约 上链的过程：

编写智能合约代码： 使用 Solidity 或 Vyper 等语言编写智能合约代码。  
编译代码： 使用编译器将智能合约代码编译成 EVM 可执行的字节码。  
创建交易： 将字节码、合约部署参数以及其他必要信息打包成一个交易。  
广播交易： 将交易广播给以太坊网络中的多个节点。  
等待区块确认： 交易被包含在区块中，并被矿工验证。  
部署完成： 当交易被包含在区块中并被确认后，智能合约部署完成。  



# remix 编写代码

remix 官网： https://remix.ethereum.org/

![img.png](img.png)


思路：

## 彩票合约代码思路 - Markdown 版本

**概述:**

这是一个简单的彩票合约，允许用户参与彩票活动。合约拥有者可以设置活动持续时间，并最终结束彩票活动，选出中奖者并分配奖金。

**核心功能:**

1. **合约初始化:**
    - 设置合约拥有者为部署者。
    - 定义彩票活动持续时间。
    - 初始化彩票状态为“进行中”。

2. **参与彩票:**
    - 任何用户都可以参与。
    - 参与需要至少 0.1 ETH 的费用。
    - 检查彩票是否已经结束。
    - 如果彩票仍在进行，将参与者地址添加到参与者列表中。
    - 记录参与事件。

3. **结束彩票:**
    - 只有合约拥有者可以结束彩票活动。
    - 检查是否已达到活动结束时间。
    - 检查彩票是否仍在进行中。
    - 检查参与者数量是否足够。
    - 将彩票状态设置为“已结束”。
    - 随机选出三位中奖者。
    - 将奖金转账给中奖者。
    - 记录彩票结束事件。

**代码结构:**

- **构造函数:** 初始化合约状态。
- **参与函数:** 处理用户参与彩票活动。
- **结束函数:** 结束彩票活动并分配奖金。
- **事件:** 记录参与和结束事件，方便追踪彩票活动。

**关键点:**

- 合约拥有者拥有结束彩票活动的权限。
- 参与者需要支付一定费用才能参与。
- 中奖者通过随机抽取的方式选出。
- 合约使用事件记录彩票活动的关键状态变化。

**其他:**

- 可以考虑添加更多功能，例如查看参与者列表、查询中奖者等。
- 可以使用更安全的随机数生成方法。
- 可以使用更复杂的奖金分配机制。


具体代码：
```js
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// 定义一个彩票合约
contract Lottery {
    address public owner; // 合约的拥有者
    address payable[] public participants; // 参与彩票的用户数组
    uint256 public lotteryEndTime; // 彩票活动的结束时间
    bool public lotteryEnded; // 彩票活动是否已结束的标志

    // 定义一个事件，当用户参与彩票时触发
    event LotteryEnter(address indexed participant);
    // 定义一个事件，当彩票结束时触发，记录中奖者地址
    event LotteryEnd(address winner1, address winner2, address winner3);

    // 构造函数，设置彩票活动的持续时间
    constructor(uint _durationMinutes) {
        owner = msg.sender; // 设置合约拥有者为部署合约的用户
        lotteryEndTime = block.timestamp + (_durationMinutes * 1 minutes); // 计算彩票活动的结束时间
        lotteryEnded = false; // 初始化彩票活动未结束的标志
    }

    // 允许用户参与彩票的函数
    function enterLottery() public payable {
        require(msg.value == 0.1 ether, "Must send exactly 0.1 ETH"); // 要求用户支付0.1以太币
        require(block.timestamp <= lotteryEndTime, "Lottery has ended"); // 检查彩票活动是否已结束
        require(!lotteryEnded, "Lottery already ended"); // 检查彩票活动是否已标记为结束
        participants.push(payable(msg.sender)); // 将用户添加到参与者数组
        emit LotteryEnter(msg.sender); // 触发参与彩票的事件
    }

    // 结束彩票活动的函数，只能由合约拥有者调用
    function endLottery() public {
        require(msg.sender == owner, "Only owner can end the lottery"); // 只有合约拥有者可以结束彩票活动
        require(block.timestamp >= lotteryEndTime, "Lottery not yet ended"); // 检查彩票活动是否已到结束时间
        require(!lotteryEnded, "Lottery already ended"); // 检查彩票活动是否已标记为结束
        require(participants.length >= 3, "Not enough participants"); // 检查是否有足够的参与者

        lotteryEnded = true; // 标记彩票活动为已结束
        uint256 prize = address(this).balance / 3; // 计算每个中奖者的奖金
        // 为了避免重复中奖，我们需要一个方法来确保每个中奖者是唯一的
        // 以下是一个简单的解决方案，但请注意，这并不是一个安全的随机数生成方法
        // 在生产环境中，应该使用更安全的随机数生成方法，如Chainlink VRF
        address[3] memory winnersAddresses; // 创建一个固定大小为3的地址数组来存储中奖者地址
        uint256[] memory winners = new uint256[](3); // 创建一个大小为3的数组来存储中奖者的索引
        for (uint i = 0; i < 3; i++) {
            uint256 winnerIndex;
            bool isUnique;
            do {
                winnerIndex = random() % participants.length; // 随机选择一个参与者作为中奖者
                isUnique = true; // 假设选中的参与者是唯一的
                for (uint j = 0; j < i; j++) {
                    if (winners[j] == winnerIndex) {
                        isUnique = false; // 如果已经选中过，标记为不唯一
                        break;
                    }
                }
            } while (!isUnique); // 如果不唯一，重新选择
            winners[i] = winnerIndex; // 记录中奖者的索引
            winnersAddresses[i] = participants[winnerIndex]; // 记录中奖者的地址
            participants[winnerIndex].transfer(prize); // 将奖金转账给中奖者
        }
        // 循环结束，触发彩票结束的事件
        emit LotteryEnd(winnersAddresses[0], winnersAddresses[1], winnersAddresses[2]);

    }
    
    // 生成随机数的私有函数，用于选择中奖者
    function random() private view returns (uint) {
        // 使用区块难度、时间戳和参与者数组作为种子生成随机数
        return uint(keccak256(abi.encodePacked(block.prevrandao, block.timestamp, participants)));
    }
}


```
