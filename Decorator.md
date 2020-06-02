# 装饰模式
今天又回过头来看了看以前写的那些文章，看的我自己都要裂开了，那叫啥文章，感觉是时候得改变一下写作的方式了，分析一下原因，最主要的原因应该还是因为自己理解
的不到位，还有就是不用心，其实这样写的文章对自己并没有多大的提升，写文章还是得细细的品啊！
## 解释：装饰模式，动态的给一个对象添加一些额外的职责，就增加功能来说，装饰模式比生成子类更加灵活
可能这样说并不是特别好理解，接下来我用大话设计模式一书中的一个故事来讲述这一设计模式，比如我现在要设计一个可以给人搭配衣服的一个系统，我首先想到的可能
是如下这种结构方式:
```
public class Person(){
  private String name;
  public void wearTShirts() {
		System.out.println("大T");
	}
	public void wearBigTrouser() {
		System.out.println("裤衩子");
	}
	public void wearSneakers() {
		System.out.println("破鞋");
	}
	public void wearsuit() {
		System.out.println("西装");
	}
	public void wearTie() {
		System.out.println("领带");
	}
	public void wearLeatherShoes() {
		System.out.println("皮鞋");
	}
}
```
```
public class client{
  public static void main(String[] args) {
      Person xc = new Person("小菜");
      xc.WearTShirts();
      xc.wearSneakers();
      xc.wearBigTrouser();
      System.out.println("-----------------------装扮1");
      xc.wearsuit();
      xc.wearTie();
      xc.wearLeatherShoes();
      System.out.println("-----------------------装扮2");
	}
}
```
这样写的弊端，如果我要增加一种服饰，比如内裤，披风，这样我就需要去修改Person类，就违反了开放-封闭原则（软件的实体是可宽展的，不可修改的）
