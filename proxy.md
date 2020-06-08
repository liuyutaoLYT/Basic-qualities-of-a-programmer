# 代理模式：为其他对象提供一种代理以控制对这个对象的访问
## 故事奥：借用大话设计模式中的一段小故事奥，大学的生活是丰富多彩，多姿多味的，假如你要追求一位姑娘，你是不是得给人家礼物，这时候就简单了，
```
public class Pursuit {
	SchoolGirl mm;
	public Pursuit(SchoolGirl mm) {
		this.mm = mm;
	}
	public void GiveDolls() {
		System.out.println(mm.getName()+"送你一个大嘴巴");
		
	}
	public void GiveFlowers() {
		System.out.println(mm.getName()+"送你一个鲜花");
	}
	public void GiveChocolate() {
		System.out.println(mm.getName()+"送你一个德芙");
	}
	
}
```
```
public class SchoolGirl {
	private String name;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
	
}
```
```
public class Client {
	public static void main(String[] args) {
		SchoolGirl yingying = new SchoolGirl();
		yingying.setName("莹莹");
		Pursuit liuyutao = new Pursuit(yingying);
		liuyutao.GiveChocolate();
		liuyutao.GiveDolls();
		liuyutao.GiveFlowers();
	}
}
```
-- 这样的话我就把礼物送给人家了，但是呢，如果你刚开始的时候不认识这个姑娘咋整，这时候你是不是就得找一个认识人家姑娘的人去介绍介绍了
```
public class SchoolGirl {
	private String name;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
	
}
```
```
public interface GiveGift {
	void GiveDolls();
	void GiveFlowers();
	void GiveChocolate();
}
```
```
public class Pursuit implements GiveGift{
	SchoolGirl mm;
	public Pursuit(SchoolGirl mm) {
		this.mm = mm;
	}
	public void GiveDolls() {
		System.out.println(mm.getName()+"送你一个大嘴巴");
		
	}
	public void GiveFlowers() {
		System.out.println(mm.getName()+"送你一个鲜花");
	}
	public void GiveChocolate() {
		System.out.println(mm.getName()+"送你一个德芙");
	}
	
}
```
```
public class Proxy implements GiveGift{
	Pursuit gg;
	public Proxy(SchoolGirl mm) {
		gg = new Pursuit(mm);
	}
	public void GiveDolls() {
		gg.GiveDolls();
	}

	public void GiveFlowers() {
		gg.GiveFlowers();
	}

	public void GiveChocolate() {
		gg.GiveChocolate();
	}
	
}
```
```
public class Client {
	public static void main(String[] args) {
		SchoolGirl yy = new SchoolGirl();
		yy.setName("莹莹");
		Proxy gg = new Proxy(yy);
		gg.GiveChocolate();
		gg.GiveDolls();
		gg.GiveFlowers();
	}
}
```
这样的话就算你不认识这个小姑娘，经过这个proxy你也可以把礼物送过去，
## 代理的应用场合：
### 1.远程代理，也就是为一个对象在不同的地址空间提供局部代表，这样可以隐藏一个对象存在于不同局部空间的事实；2.虚拟代理：是根据需要创建开销很大的对象
通过他来存放实例化需要很长时间的真实对象；3.安全代理，用来控制真实对象访问时的权限；4.智能指引：是指调用真实对象时，代理处理另外一些事
