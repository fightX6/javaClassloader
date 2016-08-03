# javaClassloader
/**
Thread.currentThread().getContextClassLoader() 和 Class.getClassLoader()区别
查了一些资料也不是太明白两个的区别，但是前者是最安全的用法

 

打个简单的比方，你一个WEB程序，发布到Tomcat里面运行。
首先是执行Tomcat org.apache.catalina.startup.Bootstrap类，这时候的类加载器是ClassLoader.getSystemClassLoader()。
而我们后面的WEB程序，里面的jar、resources都是由Tomcat内部来加载的，所以你在代码中动态加载jar、资源文件的时候，首先应该是使用Thread.currentThread().getContextClassLoader()。如果你使用Test.class.getClassLoader()，可能会导致和当前线程所运行的类加载器不一致（因为Java天生的多线程）。
Test.class.getClassLoader()一般用在getResource，因为你想要获取某个资源文件的时候，这个资源文件的位置是相对固定的。

java的类加载机制（jvm规范）是委托模型，简单的说，如果一个类加载器想要加载一个类，首先它会委托给它的parent去加载，如果它的所有parent都没有成功的加载那么它才会自己亲自来，有点儿像儿子使唤老子的感觉。

 

如果你使用Test.class.getClassLoader()，可能会导致和当前线程所运行的类加载器不一致　：Class.getClassLoader() returns the class loader for the class. Some implementations may use null to represent the bootstrap class loader. This method will return null in such implementations if this class was loaded by the bootstrap class loader.
