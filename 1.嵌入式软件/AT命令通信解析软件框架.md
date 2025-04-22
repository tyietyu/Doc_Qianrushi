at_chat 模块(无OS)

at_chat 模块使用链式队列进行管理，包含2条链表，空闲链表和就绪链表。

**基本接口与描述**

• at_send_singlline, 发送单行命令，默认等待OK响应，超时3S

• at_send_multiline, 多行命令，默认等待OK响应，超时3S

• at_do_cmd，支持自定义发送格式与接收匹配串

• at_do_work，支持自定义发送与接收解析

 

 

at 模块(OS版本)

Git Source https://toscode.gitee.com/smtian/AT-Command

 

来自 <https://mp.weixin.qq.com/s/TUcfHy2x8Aa30SOBPLC-2w> 