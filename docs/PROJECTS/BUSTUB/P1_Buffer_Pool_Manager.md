## 总体设计目标
完成数据库的缓存管理模块

## Task #1 - LRU-K Replacement Policy
* 完成LRU-K的实现并保证实现是线程安全的
* LRU-K的定义：当多个帧具有back_k_distance为inf+时，选择出现最早的一个帧驱逐
* **坑**：但驱逐多个back_k_distance时，要驱逐最早出现的一个帧，并不是按照传统的LRU算法

## Task#2 - Disk Scheduler

* Promise与future是C++17的新特性，可以用于线程之间的PV操作或者发送信息
* **坑**：这个Task基本没有坑

## Task#3-Buffer Pool Manager
* 基于Task#1和Task#2所实现的东西，实现bufferPoolManager
* bpm_bench.cpp用来测试bpm的读取和写的并发性能，调试多线程是一大难点
* **坑**：主要来自于对于各个函数实现细节和要完成功能不明确造成的，这个要通过分析测例和仔细理解题目意思来解决

## 总结与收获

* 理解题目意思，做这种题目要连续做，一气呵成，断断续续做很浪费时间
* Promise和future的使用
* 体会到测例的重要性和多线程调试的不确定性