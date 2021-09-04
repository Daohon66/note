**简介：**

ThreadLocal关键字用于给线程绑定属于各自线程的变量。ThreadLocal类可以说是一个存放数据的盒子，盒子中可以存储每个线程的私有数据。

如果创建了一个ThreadLocal变量，那么访问这个变量的每个线程都会有这个变量的本地副本。需要使用get（）和set（）方法来获取默认值或将其值更改为当前线程所存的副本值，从而避免了线程安全问题。

**原理：**

ThreadLocal所创建的变量最后存储在了ThreadLocalMap中，可以说ThreadLocal是ThreadLocalMap的封装，ThreadLocal只是传递了变量的值。ThreadLocalMap是一个ThreadLocal定制的HashMap，默认情况下俩个值都为null，只有当线程调用ThreadLocal类的get（）或set（）方法时才会创建他们，实际上我们调用的是ThreadLocalMap的get（）或set（）方法。

每个Thread中都具备一个ThreadLocalMap，而ThreadLocalMap可以存储以ThreadLocal为key，Object对象为value的键值对。

**内存泄漏问题：**

ThreadLocalMap中使用的key为ThreadLocal的弱引用，而value是强引用。在ThreadLocal没有被外部强引用的情况下，进行垃圾回收时，会将key清理掉，而value不会。这样ThreadLocalMap就会出现key为null的Entry，如果不做任何措施的话，value永远都不会被gc回收，这个时候就可能产生内存泄漏问题。但ThreadLocalMap在实现过程中也考虑到了这种情况，在调用get（）、set（）、remove（）方法的时候，会清理掉key为null的记录。我们使用完ThreadLocal方法后最好手动调用remove（）方法。