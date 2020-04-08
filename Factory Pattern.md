# 工厂模式
##  简介：
这种模式属于创建型模式，他的主要作用是用来创建对象，主要用于：在不同的情况下，我们要创造不同的实例
## 理解：
如果你需要奔驰，你可以直接去奔驰ssss店里提车，而不用管这个车是怎么来的，你就只需要知道，你去ssss店就可以把车提回来就可以。
## 优点：
如果你想要获取一个特定的实例化对象，你只需要有知道你想要一个什么类型的实例化就ok。
## 缺点：
每次增加一种类型的产品时，就得去增加一个接口和一个工厂
## 代码：
```
//首先呢，创建一个接口
public interface Shape {
   void draw();
}
```
```
//一个实体类，来实现这个接口
public class Rectangle implements Shape {
   @Override
   public void draw() {
      System.out.println("Inside Rectangle::draw() method.");
   }
}
```
```
//再来一个实体类，又实现了这个接口
public class Square implements Shape {
   @Override
   public void draw() {
      System.out.println("Inside Square::draw() method.");
   }
}
```
```
//又来了一个实体类，最后一个！
public class Circle implements Shape {
   @Override
   public void draw() {
      System.out.println("Inside Circle::draw() method.");
   }
}
```
```
//这个就是本设计模式的核心，创建出来一个工厂，我们需要的所有东西，都从这个工厂里面拿
public class ShapeFactory {
   //使用 getShape 方法获取形状类型的对象
   public Shape getShape(String shapeType){
      if(shapeType == null){
         return null;
      }        
      if(shapeType.equalsIgnoreCase("CIRCLE")){
         return new Circle();
      } else if(shapeType.equalsIgnoreCase("RECTANGLE")){
         return new Rectangle();
      } else if(shapeType.equalsIgnoreCase("SQUARE")){
         return new Square();
      }
      return null;
   }
}
```
```
//最后 我们使用这个工厂，然后根据参数传进去的类型，就可以拿到不同的实例了。
public class FactoryPatternDemo {
   public static void main(String[] args) {
      ShapeFactory shapeFactory = new ShapeFactory();
 
      //获取 Circle 的对象，并调用它的 draw 方法
      Shape shape1 = shapeFactory.getShape("CIRCLE");
 
      //调用 Circle 的 draw 方法
      shape1.draw();
 
      //获取 Rectangle 的对象，并调用它的 draw 方法
      Shape shape2 = shapeFactory.getShape("RECTANGLE");
 
      //调用 Rectangle 的 draw 方法
      shape2.draw();
 
      //获取 Square 的对象，并调用它的 draw 方法
      Shape shape3 = shapeFactory.getShape("SQUARE");
 
      //调用 Square 的 draw 方法
      shape3.draw();
   }
}
```
giao~！
