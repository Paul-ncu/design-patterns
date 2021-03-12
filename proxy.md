# 代理模式

## 1. 模式作用

代理模式在我们的日常生活中是很长常见的，例如房产中介，当我们需要租房的时候，我们可能会不直接去联系房东，而是通过中介的帮助完成租房，这样做有什么好处呢？中介可以帮我们对自己需要的房子进行筛选，包括地区，配套，价格等等。这就很大的提高了我们的租房效率。在软件开发的过程中，同样很多的情况下我们会使用到代理模式。

## 2. 代码试例

代理类和被代理类共同实现的接口

```java
public interface WizardTower {

  void enter(Wizard wizard);
}
```

```java
public class IvoryTower implements WizardTower {

  private static final Logger LOGGER = LoggerFactory.getLogger(IvoryTower.class);

  public void enter(Wizard wizard) {
    LOGGER.info("{} enters the tower.", wizard);
  }

}
```

``WizardTowerProxy`` 作为 ``IvoryTower`` 的代理类，他们共同实现了 ``WizardTower`` 接口的方法。在 ``WizardTowerProxy`` 的内部对被代理类 ``IvoryTower`` 进行了进一步的封装

```java
public class WizardTowerProxy implements WizardTower {

  private static final Logger LOGGER = LoggerFactory.getLogger(WizardTowerProxy.class);

  private static final int NUM_WIZARDS_ALLOWED = 3;

  private int numWizards;

  private final WizardTower tower;

  public WizardTowerProxy(WizardTower tower) {
    this.tower = tower;
  }

  @Override
  public void enter(Wizard wizard) {
    if (numWizards < NUM_WIZARDS_ALLOWED) {
      tower.enter(wizard);
      numWizards++;
    } else {
      LOGGER.info("{} is not allowed to enter!", wizard);
    }
  }
}
```

 ``Wizard`` 是被代理对象的一个参数

```java
public class Wizard {

  private final String name;

  public Wizard(String name) {
    this.name = name;
  }

  @Override
  public String toString() {
    return name;
  }

}
```
