# 装饰器模式

## 1. 模式作用

## 2. 代码试例

欢迎各位英雄来到峡谷，今天我们的主角是巨魔之王，来看看巨魔之王的一些方法，``attack()``，``getAttackPower()``，``fleeBattle()``。 

```java
public interface Troll {

  void attack();

  int getAttackPower();

  void fleeBattle();

}
```

我们创建了一个普通的巨魔，它的攻击力是10点。并且实现了其他的方法。

```java
public class SimpleTroll implements Troll {

  private static final Logger LOGGER = LoggerFactory.getLogger(SimpleTroll.class);

  @Override
  public void attack() {
    LOGGER.info("The troll tries to grab you!");
  }

  @Override
  public int getAttackPower() {
    return 10;
  }

  @Override
  public void fleeBattle() {
    LOGGER.info("The troll shrieks in horror and runs away!");
  }
}
```

接下来是一直拿着棒子的巨魔，它同样实现了 ``Troll`` 接口，并且在它的构造器中我们需要传入一个 ``Troll`` 接口的实现类。  

```java
public class ClubbedTroll implements Troll {

  private static final Logger LOGGER = LoggerFactory.getLogger(ClubbedTroll.class);

  private final Troll decorated;

  public ClubbedTroll(Troll decorated) {
    this.decorated = decorated;
  }

  @Override
  public void attack() {
    decorated.attack();
    LOGGER.info("The troll swings at you with a club!");
  }

  @Override
  public int getAttackPower() {
    return decorated.getAttackPower() + 10;
  }

  @Override
  public void fleeBattle() {
    decorated.fleeBattle();
  }
}
```

首先我们创建了一只没有装备的巨魔 ``SimpleTroll``，会发现它只有10点攻击力，接着我们用 ``ClubbedTroll`` 去装饰它，会发现在 ``ClubbedTroll`` 的帮助下，它的攻击力上升了。

这就是装饰器模式的作用，``SimpleTroll`` 和 ``ClubbedTroll`` 都实现了相同的接口 ``Troll``，在 ``ClubbedTroll`` 的构造器中传入 ``SimpleTroll``，就能在 ``SimpleTroll`` 的基础上进行一些修改。

```java
public class App {

  private static final Logger LOGGER = LoggerFactory.getLogger(App.class);

  /**
   * Program entry point.
   *
   * @param args command line args
   */
  public static void main(String[] args) {

    // simple troll
    LOGGER.info("A simple looking troll approaches.");
    var troll = new SimpleTroll();
    troll.attack();
    troll.fleeBattle();
    LOGGER.info("Simple troll power {}.\n", troll.getAttackPower());

    // change the behavior of the simple troll by adding a decorator
    LOGGER.info("A troll with huge club surprises you.");
    var clubbedTroll = new ClubbedTroll(troll);
    clubbedTroll.attack();
    clubbedTroll.fleeBattle();
    LOGGER.info("Clubbed troll power {}.\n", clubbedTroll.getAttackPower());
  }
}
```
