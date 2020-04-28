 # ==
  ==号对比的是两个对象的内存地址是否相同，换句话来说，就是对比是否是同一个对象
 # equals
  equals 对比的是这个对象中的值是否是相同的
 ```
 //String 的equals方法
 public boolean equals(Object anObject) {
        if (this == anObject) {
            return true;//如果是同一个对象 直接返回;
        }
        if (anObject instanceof String) {
            String anotherString = (String)anObject;
            int n = value.length;
            if (n == anotherString.value.length) {
                char v1[] = value;
                char v2[] = anotherString.value;
                int i = 0;
                while (n-- != 0) {
                  //循环比较String 中的每个值
                    if (v1[i] != v2[i])
                        return false;
                    i++;
                }
                return true;
            }
        }
        return false;
    }
 ```
 # 特别注意：
  String a="abc" String b = "abc" 这种方式声明的String 他的内存地址是一个，
  以String s="abcd";形式赋值在java中叫直接量,它是在常量池中而不是象new一样放在压缩堆中。 这种形式的字符串，在JVM内部发生字符串拘留，
  即当声明这样的一个字符串后，JVM会在常量池中先查找有有没有一个值为"abcd"的对象,如果有,就会把它赋给当前引用.
  即原来那个引用和现在这个引用指点向了同一对象, 如果没有,则在常量池中新创建一个"abcd",下一次如果有String s1 = "abcd";
  又会将s1指向"abcd"这个对象,即以这形式声明的字符串,只要值相等,任何多个引用都指向同一对象.
  这种方式使用== 也是相等的
# 注意：
hashcode相同他的equals 不一定相同，equals相同 hashcode也不一定相同，想想hashmap的hash碰撞，如果重写了equals方法 一定要重写hashcode方法
