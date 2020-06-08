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
-------------------------------------------------------------------------------------------------------
2020.6.3：今天uzi宣布要退役了，从当初厂长退役 到 ig夺冠，再到现在uzi也走了，真的感觉到我们的青春一步步走远了。
昨天咱们说到，这样写的弊端，是违反了开放-封闭原则；接下来我修改了一版
```
public class Person {
	private String name;
	public Person(String name) {
		this.name = name;
	}
	private void show() {
		System.out.println("装扮的"+this.name);
	}
}
```
```
public abstract class Finery {
	public abstract void show();
}
```
```
public class BigTrouser extends Finery{
	@Override
	public void show() {
		System.out.println("大裤衩子");
	}
}
```
```
public class Tshirts extends Finery{
	@Override
	public void show() {
		System.out.println("大T");
	}
}
```
```
public class Client {
	public static void main(String[] args) {
		Finery tshirt = new Tshirts();
		Finery trouser = new BigTrouser();
		tshirt.show();
		trouser.show();
	}
}
```
这样的话，如果我们想再添加一种服饰，就只需要我们来增加一个子类就可以了，但是这样也有一个弊端，就相当于是你光着身子站在大家的面前，然后一件一件的穿衣服
这样的话我们就需要把所有的功能串联在一起进行控制，于是有了最后一版：
```
public class Person {
	private String name;
	public Person() {}
	public Person(String name) {
		this.name = name;
	}
	public void show() {
		System.out.println("装扮的"+name);
	}
}
```
```
public class Finery extends Person{
	protected Person person;
	public void Decorate(Person person) {
		this.person = person;
	}
	@Override
	public void show() {
		if(person!=null) {
			person.show();
		}
	}
}
```
```
public class BigTrouser extends Finery{
	@Override
	public void show() {
		System.out.println("大裤衩子");
	}
}
```
```
public class Tshirts extends Finery{
	@Override
	public void show() {
		System.out.println("大T");
	}
}
```
```
public class Client {
	public static void main(String[] args) {
		Person person = new Person("aaa");
		Tshirts t = new Tshirts();
		BigTrouser bt = new BigTrouser();
		bt.Decorate(person);
		t.Decorate(bt);
	}
}
```
## 总结:个人感觉装饰器模式就是就相当于是一个人到了上衣店，他给你穿了一件上衣，然后你有了上衣去裤子店，他给你穿了一个裤子，一系列的操作是串联的，并且是无序的。可以动态的给功能去添加更多功能的一种形式，而且这样的话，耦合程度也非常，可以去除类中的相关的重复装饰逻辑，他就是把一个装饰的对象放在了一个类中，然后又有其他的类来装饰这个类。
