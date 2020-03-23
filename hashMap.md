# hashMap
## hashMap 底层结构
一个数组+链表的结构，在jdk1.8以后链表如果太长，会转变成红黑树，下面从源码中的关键方法进行解析
## 四种构造函数
```
public HashMap() {
    this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted,static final float DEFAULT_LOAD_FACTOR = 0.75f;
}
```
无参构造函数，默认的扩容因子是0.75
``` 
public HashMap(int initialCapacity, float loadFactor) {
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal initial capacity: " +
                                           initialCapacity);
    if (initialCapacity > MAXIMUM_CAPACITY)//  static final int MAXIMUM_CAPACITY = 1 << 30;
        initialCapacity = MAXIMUM_CAPACITY;
    if (loadFactor <= 0 || Float.isNaN(loadFactor))
        throw new IllegalArgumentException("Illegal load factor: " +
                                           loadFactor);
    this.loadFactor = loadFactor;
    this.threshold = tableSizeFor(initialCapacity);
}
```
两个参数，第一个参数是数组的长度，第二个参数是扩容因子，
```
public HashMap(int initialCapacity) {
    this(initialCapacity, DEFAULT_LOAD_FACTOR);
}
```
一个参数，初始数组的大小，然后方法体里面调用的还是第二种构造函数，扩容因子是默认扩容因子；
```
public HashMap(Map<? extends K, ? extends V> m) {
    this.loadFactor = DEFAULT_LOAD_FACTOR;
    putMapEntries(m, false);
}
```
参数是一个map ，方法体中将这个map放到新的map中
# get函数
```
public V get(Object key) {
    Node<K,V> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}
```
这个get方法里面主要还是getNode方法，下面我们看看getNode方法
```
final Node<K,V> getNode(int hash, Object key) {
    Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
    //判断数组是否为空，再判断这个地址下是否有第一个节点
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (first = tab[(n - 1) & hash]) != null) {//n 是之前获取的 table 的长度，n 的值总是 2 的次方(16/32/64/128...)，(n-1)转换成二进制低位全部是 1，和 hash 值&操作相当于对 n 取余。
        if (first.hash == hash && // always check first node 比较第一个节点的hash值是否是我们想要的
            ((k = first.key) == key || (key != null && key.equals(k))))
            return first;
        if ((e = first.next) != null) { //如果第一个节点不是，就要找下面的节点
            if (first instanceof TreeNode)//判断下面的节点是不是树形
                return ((TreeNode<K,V>)first).getTreeNode(hash, key);
            do {
                if (e.hash == hash &&//如果是链式结构，就一个一个往下找
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;
            } while ((e = e.next) != null);
        }
    }
    return null;
}
```
## put 方法
```
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);//如果寻址完成之后没有节点，需要新建一个节点
        else {
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;//p就是当前的这个节点
            else if (p instanceof TreeNode)//判断p是否是个树节点
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);//如果不是树节点就往下连
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st  static final int TREEIFY_THRESHOLD = 8;>=7的时候转化成树
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;//操作了多少次
        //插入新数据后判断是否需要扩容
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```
