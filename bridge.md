# 桥接模式

## 1. 模式作用

## 2. 代码试例

大家好，我是武器大师，我有着非常丰富的打造武器的经验。我可以打造各种各样的武器，并且我会使用魔法来加强武器。今天我会教我的徒弟如何打造武器。
首先我的武器是根据下面这些接口来实现，同时我的魔法也是根据魔法增强的接口来实现的。

```java
// 武器接口，声明了武器的一些方法
public interface Weapon {

  void wield();

  void swing();

  void unwield();

  Enchantment getEnchantment();
}

// 魔法增强接口
public interface Enchantment {

  void onActivate();

  void apply();

  void onDeactivate();
}
```

武器大师会很多厉害的魔法，今天教徒弟的是飞行附魔和噬魂附魔，当然以后我会教他更多的魔法，这只需要我们实现 ``Enchantment`` 接口，创建不同的实现类即可。

```java
public class FlyingEnchantment implements Enchantment {

  private static final Logger LOGGER = LoggerFactory.getLogger(FlyingEnchantment.class);

  @Override
  public void onActivate() {
    LOGGER.info("The item begins to glow faintly.");
  }

  @Override
  public void apply() {
    LOGGER.info("The item flies and strikes the enemies finally returning to owner's hand.");
  }

  @Override
  public void onDeactivate() {
    LOGGER.info("The item's glow fades.");
  }
}

public class SoulEatingEnchantment implements Enchantment {

  private static final Logger LOGGER = LoggerFactory.getLogger(SoulEatingEnchantment.class);

  @Override
  public void onActivate() {
    LOGGER.info("The item spreads bloodlust.");
  }

  @Override
  public void apply() {
    LOGGER.info("The item eats the soul of enemies.");
  }

  @Override
  public void onDeactivate() {
    LOGGER.info("Bloodlust slowly disappears.");
  }
}
```

打造的第一把武器是一把锤子，根据锤子的特点，通过 ``Weapon`` 接口来实现它，在锤子中有个 ``Enchantment`` 属性，需要我们在锤子的构造器中注入魔法，来为它赋值，至于具体是什么魔法，可以根据具体的需求来注入。所以我们在锤子的构造器中需要传入 ``Enchantment`` 接口的实现类来确定具体需要注入的魔法。

这个时候徒弟问武器大师，为什么不让锤子也实现 ``Enchantment`` 接口呢，这样也能给锤子附魔啊。武器大师告诉徒弟，你这么做是可以实现，但是你想过一个问题吗，如果我要你打造一把锤子，一把剑，一把斧子，同时给他们分别用飞行附魔和噬魂附魔，按照你这种方法，需要多少个实现类呢？

徒弟想了想，告诉武器大师，需要 2 * 3 = 6 个实现类。那如果我要打造更多的武器，和更多的附魔呢？徒弟突然觉得如果按照实现两个接口的话会很麻烦，需要更多的实现类。武器大师告诉徒弟，这里我采用了组合的方法，而不是继承，通过组合，我们可以用很少的类完成更多的事情。徒弟感到收获很大。

```java
public class Hammer implements Weapon {

  private static final Logger LOGGER = LoggerFactory.getLogger(Hammer.class);

  private final Enchantment enchantment;

  public Hammer(Enchantment enchantment) {
    this.enchantment = enchantment;
  }

  @Override
  public void wield() {
    LOGGER.info("The hammer is wielded.");
    enchantment.onActivate();
  }

  @Override
  public void swing() {
    LOGGER.info("The hammer is swinged.");
    enchantment.apply();
  }

  @Override
  public void unwield() {
    LOGGER.info("The hammer is unwielded.");
    enchantment.onDeactivate();
  }

  @Override
  public Enchantment getEnchantment() {
    return enchantment;
  }
}
```

同样的第二件武器是一把剑，我们按照打造锤子的方式，来构建它。

```java
public class Sword implements Weapon {

  private static final Logger LOGGER = LoggerFactory.getLogger(Sword.class);

  private final Enchantment enchantment;

  public Sword(Enchantment enchantment) {
    this.enchantment = enchantment;
  }

  @Override
  public void wield() {
    LOGGER.info("The sword is wielded.");
    enchantment.onActivate();
  }

  @Override
  public void swing() {
    LOGGER.info("The sword is swinged.");
    enchantment.apply();
  }

  @Override
  public void unwield() {
    LOGGER.info("The sword is unwielded.");
    enchantment.onDeactivate();
  }

  @Override
  public Enchantment getEnchantment() {
    return enchantment;
  }
}
```

武器大师要发动他的魔法了，他给剑注入了噬魂魔法，给锤子注入了飞行魔法。通过不同的魔法注入，他们会有不一样的效果。

```java
public class App {

  private static final Logger LOGGER = LoggerFactory.getLogger(App.class);

  /**
   * Program entry point.
   *
   * @param args command line args
   */
  public static void main(String[] args) {
    LOGGER.info("The knight receives an enchanted sword.");
    var enchantedSword = new Sword(new SoulEatingEnchantment());
    enchantedSword.wield();
    enchantedSword.swing();
    enchantedSword.unwield();

    LOGGER.info("The valkyrie receives an enchanted hammer.");
    var hammer = new Hammer(new FlyingEnchantment());
    hammer.wield();
    hammer.swing();
    hammer.unwield();
  }
}
```

## 3. 优缺点
