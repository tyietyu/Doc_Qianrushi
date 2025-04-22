XIP（executed in place）本地执行。操作系统采用这种系统，可以不用将[内核](https://baike.baidu.com/item/内核)或执行代码拷贝到[内存](https://so.csdn.net/so/search?q=内存&spm=1001.2101.3001.7020)，而直接在代码的[存储空间](https://baike.baidu.com/item/存储空间)直接运行。

XIP是一种能够直接在闪速存储器中执行代码而无须装载到RAM中执行的机制。

 

来自 <[https://blog.csdn.net/yangzcc/article/details/96423819?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522170469599916800211525071%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=170469599916800211525071&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-4-96423819-null-null.142^v99^pc_search_result_base9&utm_term=XIP%E6%8A%80%E6%9C%AF&spm=1018.2226.3001.4187](https://blog.csdn.net/yangzcc/article/details/96423819?ops_request_misc=%7B%22request%5Fid%22%3A%22170469599916800211525071%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=170469599916800211525071&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-4-96423819-null-null.142^v99^pc_search_result_base9&utm_term=XIP技术&spm=1018.2226.3001.4187)> 

 

**程序执行过程可以参考如下：**

https://www.cnblogs.com/dy2903/p/8453660.html



Nor Flash芯片内执行（XIP）

 

来自 <[https://blog.csdn.net/qq_40390825/article/details/118893597?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522170469599916800182788687%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=170469599916800182788687&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-11-118893597-null-null.142^v99^pc_search_result_base9&utm_term=XIP%E6%8A%80%E6%9C%AF&spm=1018.2226.3001.4187](https://blog.csdn.net/qq_40390825/article/details/118893597?ops_request_misc=%7B%22request%5Fid%22%3A%22170469599916800182788687%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=170469599916800182788687&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-11-118893597-null-null.142^v99^pc_search_result_base9&utm_term=XIP技术&spm=1018.2226.3001.4187)> 