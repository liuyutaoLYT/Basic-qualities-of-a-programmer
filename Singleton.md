# 单例模式：
## 单例模式的定义：
  -- 一个类只能有一个实例，并且这个类可以自行创建这个实例。
## 特点：
  -- 1.单例类只能有一个实例对象
  -- 2.该单例对象必须由该单例自己创建
  -- 3.单例类对外提供一个访问该单例的 全局 访问点
  
## 单例模式有两种实现方式
 -- 两种模式都是把构造方法变成私有，向外提供一个静态的公有方法来创建单实例，因为静态方法只能访问静态变量 所以单例模式中的所有变量，都必须是静态的（因为在类加载的时候是先加载静态类的）
  ### 1.懒汉模式：
        -- 所谓懒汉模式，就是他比较懒，你不用他，他不创建，
  ```
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
 ```
如果是多线程 这个 volatile 和 synchronized 不能去掉，他得保证线程的安全和同步；但是 你每次都需要去同步一下数据，就会影响性能，耗费更多的资源
（懒汉模式的缺点）
 ### 2.饿汉模式：
       -- 饿汉模式，就是这个人已经饿得不行了，不需要你去给他食物，他会自己去找，也就是说，在你还没调用的时候，只要这个类加载到了他就会去创建实例
       ```
          public class Singleton {  
              private static Singleton instance = new Singleton();  
              private Singleton (){}  
              public static Singleton getInstance() {  
                return instance;  
              }  
          }
        ```
      -- 饿汉模式，是线程安全的 因为只在加载类的时候创建了一次，这样的话 不会再去更改他（但是比较容易产生垃圾，因为可能都不用他他就创建了）
 ### 3.双检锁模式：
       -- 融合了懒汉模式和饿汉模式的优点
       
       ```
        public class Test {
            private volatile static Test instance;

            private Test() {

            }

            public static Test getInstance() {
                if (instance == null) {
                    synchronized (Test.class) {
                        if (instance == null) {
                            instance = new Test();
                        }
                    }
                }
                return instance;
            }
         }
       ```
        -- 这个模式相对于上面的两种模式，是一种比较好的写法，必须分析一波：getInstance方法中，
        -- 1.第一个if判断，可以去掉，没有逻辑上的错误，但是去掉之后每次进来了之后都回去获取锁，获取锁就是一个非常消耗资源的操作了，
        -- 2.加上synchronized 可以防止多个线程同时访问getInstance() 不加的话就会初始化出多个对象，不符合单例模式
        -- 3.第二个if判断，不能去掉，如果去掉，现在a获得了锁，执行了一遍，然后释放锁，然后b又获得了锁，再执行一遍，也不符合单例模式的条件
        -- 4.volatile 关键字，禁止指令重排序，
### 4.静态内部类：
    -- 相对于双检锁模式更加容易编写，同时也保证了线程安全
    ```
      public class SingleTon{
        private SingleTon(){}

        private static class SingleTonHoler{
           private static SingleTon INSTANCE = new SingleTon();
       }

        public static SingleTon getInstance(){
          return SingleTonHoler.INSTANCE;
        }
      }
    ```
    -- 接下来我们分析一波
    -- 外部类加载的时候 是不会触发内部类的加载的，这样就相当于是懒汉模式，只有调用了getInstance()方法，才会去实例化，在以后的文档中，会详细描述一下java类的实例化过程
 ### 5.枚举单例：
      ```
        public enum SingleTon{
          INSTANCE;
                public void method(){
                //TODO
             }
        }
       ```
       -- 枚举相当于是一个普通类，在任何情况下 他都是一个单例  如果我们想调用这个方法： SingTon.INSTANCE
      
