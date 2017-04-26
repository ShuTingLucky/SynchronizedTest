# SynchronizedTest
Java语言中Synchronized的用法（同步锁）  

当没有明确的对象作为锁，只是想让一段代码同步时，可以创建一个特殊的instance变量（它得是一个对象）来充当锁：  
`private byte[] lock = new byte[0]; `  
  
注意：零长度的byte数组对象创建起来将比任何对象都经济――查看编译后的字节码：生成零长度的byte[]对象只需3条操作码,  
而 `Object lock= new Object()` 则需要7行操作码。  

***明确synchronized锁定的是哪个对象，能帮助我们设计更安全的多线程程序***
  
  
一些技巧可以让我们对共享资源的同步访问更加安全：
1. 定义private 的instance变量+它的 get方法，而不要定义public/protected的instance变量。  
*因为：如果将变量定义为public，对象在外界可以绕过同步方法的控制而直接取得它，并改动它。这也是JavaBean的标准实现方式之一。*  
2. 如果instance变量是一个对象，如数组或ArrayList什么的，那上述方法仍然不安全。  
*因为当外界对象通过get方法拿到这个instance对象的引用后，又将其指向另一个对象，那么这个private变量也就变了，很危险。这个时候就需要将get方法也加上synchronized同步，并且，只返回这个private对象的clone()――这样，调用端得到的就是对象副本的引用了*  

  
[参考](http://www.cnblogs.com/GnagWang/archive/2011/02/27/1966606.html)
