← [Ashe wiki](../README.md)

---
MQ丢失的解决方案：

Producer Loss：（根据MQ的异步回调）开启重试或使用本地事务（task表维护MQ生命周期状态）/事务消息（RocketMQ本身提供的功能）保证消息发出。  
Broker Loss：开启持久化和多副本保证消息存储。  
Consumer Loss：使用手动 ACK 保证消息处理，消费失败/拒绝签收则消息重新入队，需要设计幂等逻辑（业务状态变更）break，以避免重复消费。  
