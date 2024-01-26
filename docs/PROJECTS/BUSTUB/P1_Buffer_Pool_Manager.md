## 总体设计目标
TODO：
- thread safe
## Task #1 - LRU-K Replacement Policy
* 基本完成LRU-K的实现。
* TODO：用p0中学到的智能指针优化性能。
* LRU-K的定义：当多个帧具有back_k_distance为inf+时，选择出现最早的一个帧驱逐

```
std::unordered_map<frame_id_t, LRUKNode> node_store_; 
size_t current_timestamp_{0};  
size_t curr_size_{0};  
  

size_t replacer_size_;  
size_t k_;  
std::mutex latch_;
```
## Task#2 - Disk Scheduler
* Promise与future在线程之间同步与发送信息

## Task#3-Buffer Pool Manager
* 基于Task#1和Task#2所实现的东西，实现bufferPoolManager
* 注意bench的使用

```
const size_t pool_size_;  
std::atomic<page_id_t> next_page_id_ = 0;  
Page *pages_;  
std::unique_ptr<DiskScheduler> disk_scheduler_ __attribute__((__unused__));  
LogManager *log_manager_ __attribute__((__unused__));  
std::unordered_map<page_id_t, frame_id_t> page_table_;  
std::unique_ptr<LRUKReplacer> replacer_;  
std::list<frame_id_t> free_list_;  
std::mutex latch_;
```

bug:
- FetchPage和UnpinPage可能存在并发问题

## clion中调试多线程

* 