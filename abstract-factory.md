# 抽象工厂模式

## 1. 模式作用

相比与工厂方法模式的每个子类工厂只能生产一种产品，如果需要创建新的产品必须添加新的工厂的缺点，我们对工厂方法进行改进，使得每个工厂可以生产多个产品。

## 2. 代码试例

定义一个父类抽象工厂，其中声明了创建产品的方法。

```java
public interface KingdomFactory {

  Castle createCastle();

  King createKing();

  Army createArmy();

}
```

具体的产品工厂，实现了抽象工厂的接口，负责创造具体的产品

```java
public class ElfKingdomFactory implements KingdomFactory {

  @Override
  public Castle createCastle() {
    return new ElfCastle();
  }

  @Override
  public King createKing() {
    return new ElfKing();
  }

  @Override
  public Army createArmy() {
    return new ElfArmy();
  }

}

public class OrcKingdomFactory implements KingdomFactory {

  @Override
  public Castle createCastle() {
    return new OrcCastle();
  }

  @Override
  public King createKing() {
    return new OrcKing();
  }

  @Override
  public Army createArmy() {
    return new OrcArmy();
  }
}
```

产品抽象类或者接口，定义了一类产品的接口。

```java
public interface Army {

  String getDescription();
}
public interface Castle {

  String getDescription();
}

public interface King {

  String getDescription();
}
```

具体产品的实现，定义一个即将被工厂创建的产品对象

接下来的三个产品是由 ``ElfKingdomFactory`` 工厂创建的产品

```java
public class ElfArmy implements Army {

  static final String DESCRIPTION = "This is the Elven Army!";

  @Override
  public String getDescription() {
    return DESCRIPTION;
  }
}
public class ElfCastle implements Castle {

  static final String DESCRIPTION = "This is the Elven castle!";

  @Override
  public String getDescription() {
    return DESCRIPTION;
  }
}
public class ElfKing implements King {

  static final String DESCRIPTION = "This is the Elven king!";

  @Override
  public String getDescription() {
    return DESCRIPTION;
  }
}
```

三个将由 ``OrcKingdomFactory`` 工厂创建的产品

```java
public class OrcArmy implements Army {

  static final String DESCRIPTION = "This is the Orc Army!";

  @Override
  public String getDescription() {
    return DESCRIPTION;
  }
}
public class OrcCastle implements Castle {

  static final String DESCRIPTION = "This is the Orc castle!";

  @Override
  public String getDescription() {
    return DESCRIPTION;
  }
}
public class OrcKing implements King {

  static final String DESCRIPTION = "This is the Orc king!";

  @Override
  public String getDescription() {
    return DESCRIPTION;
  }
}
```

## 3. 优缺点

增加新的子工厂非常简单，只需要实现 ``KingdomFactory`` 创建一个子类工厂，但是在子类工厂中增加新的创建产品接口很负责，例如我需要创建新的产品 ``Queen`` ，需要新的 ``Queen`` 接口和实现类，同时需要在子类工厂中添加新的方法用来创建 ``Queen`` , 不利于扩展。

---

## 4. 关于简单工厂模式、工厂方法模式、抽象工厂模式的一个总结

简单工厂模式：只有一个工厂类，一个产品类和不同的产品实现类，通过工厂类的传入参数来判断需要创建哪一个产品。

工厂方法模式：一个抽象工厂类，一个抽象产品类，多个抽象工厂的实现类，每个抽象工厂的实现类只会生产一种抽象产品类的实现类。

抽象工厂模式：一个抽象工厂类，多个产品抽象类，有多个抽象工厂的实现类，每个工厂抽象类的实现可以创建一组抽象产品类的实现。
