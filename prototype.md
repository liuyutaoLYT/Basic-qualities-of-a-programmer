# 原型模式：用原型实例指定创建对象的种类，并且通过拷贝这些对象创建新的对象
分享一个大话设计模式中的小故事：假如你现在要投简历你首先想到的代码实现方式是不是如下的这种：
```
public class Resume {
	private String name;
	private String sex;
	private String age;
	private String timeArea;
	private String company;
	public Resume(String name) {
		this.name = name;
	}
	public void setPersonalInfo(String sex,String age) {
		this.sex = sex;
		this.age = age;
	}
	public void setWorkExperience(String timeArea,String company) {
		this.timeArea = timeArea;
		this.company = company;
	}
	public void Display() {
		System.out.println(name +sex +age);
	}
	
	
}
```
```
public class Client {
	public static void main(String[] args) {
		Resume a = new Resume("a");
		a.setPersonalInfo("1", "11");
		a.setWorkExperience("2018", "A");
		Resume b = new Resume("B");
		b.setPersonalInfo("1", "11");
		b.setWorkExperience("2019","B");
	}
}
```
这样的话 如果我需要二十份简历，你就需要去实例化二十次
原型模式其实就是从一个对象再创建另外一个可定制的对象，而且不知道任何创建的细节
今天先写到这里 电脑卡的实在是顶不住了 明天继续
