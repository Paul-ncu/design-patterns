# 建造者模式

## 1. 模式作用

在介绍这个模式的时候，我会通过讲解一款小游戏的形式给大家介绍建造者模式的原理。相信大家可能会玩过赛博朋克，赛博朋克是一款近期出的比较热门的游戏。在游戏中玩家可以根据自己的兴趣选择不同的职业，同时也可以为自己的角色更换发型，脸部特征，穿着等的，它拥有非常多的选项。

如同我们在创建一个对象的时候，在构造器中可能会有很多的参数需要我们去传入，这是一个很麻烦的事情，而建造者模式可以帮助我们，把对象的创建和表示分离，通过相同的步骤我们可以创建不同的对象。

## 2. 代码试例

``Hero`` 相当于赛博朋克中我们需要构建的角色，我们要为他选择职业，姓名，发型，穿着等。

```java
public final class Hero {

  private final Profession profession;
  private final String name;
  private final HairType hairType;
  private final HairColor hairColor;
  private final Armor armor;
  private final Weapon weapon;

  private Hero(Builder builder) {
    this.profession = builder.profession;
    this.name = builder.name;
    this.hairColor = builder.hairColor;
    this.hairType = builder.hairType;
    this.weapon = builder.weapon;
    this.armor = builder.armor;
  }

  public Profession getProfession() {
    return profession;
  }

  public String getName() {
    return name;
  }

  public HairType getHairType() {
    return hairType;
  }

  public HairColor getHairColor() {
    return hairColor;
  }

  public Armor getArmor() {
    return armor;
  }

  public Weapon getWeapon() {
    return weapon;
  }

  @Override
  public String toString() {

    var sb = new StringBuilder();
    sb.append("This is a ")
        .append(profession)
        .append(" named ")
        .append(name);
    if (hairColor != null || hairType != null) {
      sb.append(" with ");
      if (hairColor != null) {
        sb.append(hairColor).append(' ');
      }
      if (hairType != null) {
        sb.append(hairType).append(' ');
      }
      sb.append(hairType != HairType.BALD ? "hair" : "head");
    }
    if (armor != null) {
      sb.append(" wearing ").append(armor);
    }
    if (weapon != null) {
      sb.append(" and wielding a ").append(weapon);
    }
    sb.append('.');
    return sb.toString();
  }

  /**
   * The builder class.
   */
  public static class Builder {

    private final Profession profession;
    private final String name;
    private HairType hairType;
    private HairColor hairColor;
    private Armor armor;
    private Weapon weapon;

    /**
     * Constructor.
     */
    public Builder(Profession profession, String name) {
      if (profession == null || name == null) {
        throw new IllegalArgumentException("profession and name can not be null");
      }
      this.profession = profession;
      this.name = name;
    }

    public Builder withHairType(HairType hairType) {
      this.hairType = hairType;
      return this;
    }

    public Builder withHairColor(HairColor hairColor) {
      this.hairColor = hairColor;
      return this;
    }

    public Builder withArmor(Armor armor) {
      this.armor = armor;
      return this;
    }

    public Builder withWeapon(Weapon weapon) {
      this.weapon = weapon;
      return this;
    }

    public Hero build() {
      return new Hero(this);
    }
  }
}
```

``Hero`` 中的内部类 ``Builder`` 是一个建造者，帮助我们来构建游戏中的角色的一些属性。

接下来的是各个属性值的枚举类，可在枚举类中选择可用的属性，然后交给建造者，让他去帮助我们完成角色的创建。

```java
/**
 * Armor enumeration.
 */
public enum Armor {

  CLOTHES("clothes"), LEATHER("leather"), CHAIN_MAIL("chain mail"), PLATE_MAIL("plate mail");

  private final String title;

  Armor(String title) {
    this.title = title;
  }

  @Override
  public String toString() {
    return title;
  }
}

/**
 * HairColor enumeration.
 */
public enum HairColor {

  WHITE, BLOND, RED, BROWN, BLACK;

  @Override
  public String toString() {
    return name().toLowerCase();
  }

}

/**
 * HairType enumeration.
 */
public enum HairType {

  BALD("bald"), SHORT("short"), CURLY("curly"), LONG_STRAIGHT("long straight"), LONG_CURLY(
      "long curly");

  private final String title;

  HairType(String title) {
    this.title = title;
  }

  @Override
  public String toString() {
    return title;
  }
}

/**
 * Profession enumeration.
 */
public enum Profession {

  WARRIOR, THIEF, MAGE, PRIEST;

  @Override
  public String toString() {
    return name().toLowerCase();
  }
}

/**
 * Weapon enumeration.
 */
public enum Weapon {

  DAGGER, SWORD, AXE, WARHAMMER, BOW;

  @Override
  public String toString() {
    return name().toLowerCase();
  }
}
```

通过使用建造者的方法，我们可以为我们的角色加上不同的装扮，最后通过 ``builde()`` 来完成我们角色的构建。

```java
public class App {

  private static final Logger LOGGER = LoggerFactory.getLogger(App.class);

  /**
   * Program entry point.
   *
   * @param args command line args
   */
  public static void main(String[] args) {

    var mage = new Hero.Builder(Profession.MAGE, "Riobard")
        .withHairColor(HairColor.BLACK)
        .withWeapon(Weapon.DAGGER)
        .build();
    LOGGER.info(mage.toString());

    var warrior = new Hero.Builder(Profession.WARRIOR, "Amberjill")
        .withHairColor(HairColor.BLOND)
        .withHairType(HairType.LONG_CURLY).withArmor(Armor.CHAIN_MAIL).withWeapon(Weapon.SWORD)
        .build();
    LOGGER.info(warrior.toString());

    var thief = new Hero.Builder(Profession.THIEF, "Desmond")
        .withHairType(HairType.BALD)
        .withWeapon(Weapon.BOW)
        .build();
    LOGGER.info(thief.toString());

  }
}
```

## 3. 模式优缺点

### 3.1 优点

在开发过程中，对一个对象的创建，如果一些参数之间存在依赖关系，我们可以通过建造者内部帮助我们处理这些依赖关系，我们不需要去关注具体的细节。

通过不同的建造者，用户可以创建不同的对象，同样用户也不用关注具体的实现。

### 3.2 缺点

如果角色的内部发生了变化，建造者同时也要变化，可能会需要很多不同的建造者去实现，会造成系统的庞大。
