##### 工作背景：企业级应用运维开发，客户公司服务器采用的weblogic, 在监控某项目的性能测试时，测试压到一定的量级，日志中会有[java.lang.OutOfMemoryError: GC overhead limit exceeded] 信息，误导性能测试结果；

<br>  

##### java.lang.OutOfMemoryError: GC overhead limit exceeded意义说明如下：  
GC overhead limt exceed检查是Hotspot VM 1.6定义的一个策略，通过统计GC时间来预测是否要OOM了，提前抛出异常，防止OOM发生。Sun 官方对此的定义是：“并行/并发回收器在GC回收时间过长时会抛出OutOfMemroyError。过长的定义是，超过98%的时间用来做GC并且回收了不到2%的堆内存。用来避免内存过小造成应用不能正常工作。“

Ref: https://blog.csdn.net/casablancaagnes_3sdf/article/details/52299100


##### 解决方法：
weblogic中部署的应用设置关闭该报错信息的参数：XX:-UseGCOverheadLimit，同时同时增加heap大小  

##### 常见的堆相关的名词意义：
Eden Space：堆内存池，大多数对象在这里分配内存空间。

Survivor Space：堆内存池，存储在Eden Space的gc中存活下来的对象。

Tenured Generation：堆内存池，存储Survivor Space中存活过几次gc的对象。

Permanent Generation：非堆空间，存储的是class和method对象。

Code Cache：非堆空间，JVM用来存储编译和存储native code。