#单例模式：
##单例模式的定义：
  一个类只能有一个实例，并且这个类可以自行创建这个实例。
##特点：
  1.单例类只能有一个实例对象
  2.该单例对象必须由该单例自己创建
  3.单例类对外提供一个访问该单例的 全局 访问点
  
##单例模式有两种实现方式
 两种模式都是把构造方法变成私有，向外提供一个静态的公有方法来创建单实例，因为静态方法只能访问静态变量 所以单例模式中的所有变量，都必须是静态的（因为在类
 加载的时候是先加载静态类的）
  ###1.懒汉模式：所谓懒汉模式，就是他比较懒，你不用他，他不创建，
  code:
      public class LazySingleton{
        private static volatile LazySingleton instance=null;    //保证 instance 在所有线程中同步
        private LazySingleton(){}    //private 避免类在外部被实例化
        public static synchronized LazySingleton getInstance()
        {
            //getInstance 方法前加同步
            if(instance==null)
            {
                instance=new LazySingleton();
            }
            return instance;
        }
    }
//如果是多线程 这个 volatile 和 synchronized 不能去掉，他得保证线程的安全和同步；但是 你每次都需要去同步一下数据，就会影响性能，耗费更多的资源
（懒汉模式的缺点）
## 饿汉模式
