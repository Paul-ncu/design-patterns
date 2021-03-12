# 简单工厂

## 1. 模式作用

简单工厂又称为静态工厂方法，它提供一种静态方法来创建不同的类，是为了隐藏具体的实现逻辑，让用户只关注于它的使用，而不是初始化和管理。

---

## 2. 代码试例

用户在使用过程中不必关注具体Car是如何实现的，只需要关注它的使用即可。

```java
public class App {
  
  private static final Logger LOGGER = LoggerFactory.getLogger(App.class);
  
  /**
   * Program main entry point.
   */
  public static void main(String[] args) {
    var car1 = CarsFactory.getCar(CarType.FORD);
    var car2 = CarsFactory.getCar(CarType.FERRARI);
    LOGGER.info(car1.getDescription());
    LOGGER.info(car2.getDescription());
  }
}
```

接下来将会由CarsFactory来帮助我们完成Car的具体实现，通过调用 ``getCar()`` 获取我们具体所需要的Car的实现类

```java
public class CarsFactory {
  
  /**
   * 静态方法通过输入的枚举类型来创建用户所需要的具体实现。
   */
  public static Car getCar(CarType type) {
    return type.getConstructor().get();
  }
}
```

使用一个枚举类来声明我们所需要创建的一些具体Car的实现类

```java
public enum CarType {
  
  /**
   * Enumeration for different types of cars.
   */
  FORD(Ford::new), 
  FERRARI(Ferrari::new);
  
  private final Supplier<Car> constructor; 
  
  CarType(Supplier<Car> constructor) {
    this.constructor = constructor;
  }
  
  public Supplier<Car> getConstructor() {
    return this.constructor;
  }
}
```

Car在代码中表示的是一类事物的抽象，我们用一个接口来体现，对不同的实现Car接口的类，可以实现不同的方法。例如在实际生活中，轿车和越野车有不同的描述，简单工厂就帮助我们，不需要知道他们的具体描述是如何，用户只需要知道自己需要的是哪种Car，将由工厂帮助我们来完成具体Car的实现。

```java
public interface Car {
  
  String getDescription();
  
}
```

Car的两个实现类Ferrari(法拉利)和Ford(福特)，它们对Car都有不同的实现

```java
public class Ferrari implements Car {
   
  static final String DESCRIPTION = "This is Ferrari.";

  @Override
  public String getDescription() {
    return DESCRIPTION;
  }
}

public class Ford implements Car {

  static final String DESCRIPTION = "This is Ford.";

  @Override
  public String getDescription() {
    return DESCRIPTION;
  }
}
```

---

## 3. 简单工厂模式的缺点

在简单工厂的使用过程中，如果我们还需要再加入一种Car的实现，接下来看看我们应该怎么做。

首先我们创建一个Car的实现类Audi

```java
public class Audi implements Car {
   
  static final String DESCRIPTION = "This is Audi.";

  @Override
  public String getDescription() {
    return DESCRIPTION;
  }
}
```

接下来如果我们需要通过过程来生成Audi，就必须再 ``CarType`` 中添加 ``AUDI(Audi::new)``，这不符合我们的 “ 开闭原则 ”，对扩展开放，对修改闭合。

所以接下来我们会学习工厂方法模式和抽象工厂模式。逐步完善简单工厂模式的不足之处。
