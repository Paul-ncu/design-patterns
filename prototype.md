# 原型模式

## 1. 模式作用

原型模式属于创建型模式，用一个已经创建的实例作为原型，通过复制该原型对象来创建一个和原型相同或者相似的新对象。在这里，原型实例指定了要创建的对象的种类，这是一种效率非常高的做法，该模式一般会和工厂模式结合使用，在工厂创建对象的具体实现中使用了原型模式，可以提高创建对象的效率。

## 2. 代码试例

通过对每个类实现原型接口，对 ``copy()`` 方法进行重写，完成对对象的复制，在一些环境下可以简化对象的创建过程。

定义一个原型接口，其中提供了复制原型的方法

```java
public interface Prototype {

  Object copy();

}
```

三个实现了原型接口的抽象类

```java
public abstract class Beast implements Prototype {

  public Beast() {
  }

  public Beast(Beast source) {
  }

  @Override
  public abstract Beast copy();

  @Override
  public boolean equals(Object obj) {
    if (this == obj) {
      return true;
    }
    if (obj == null) {
      return false;
    }
    return getClass() == obj.getClass();
  }

}

public abstract class Mage implements Prototype {

  public Mage() {
  }

  public Mage(Mage source) {
  }

  @Override
  public abstract Mage copy();

  @Override
  public boolean equals(Object obj) {
    if (this == obj) {
      return true;
    }
    if (obj == null) {
      return false;
    }
    return getClass() == obj.getClass();
  }

}

public abstract class Warlord implements Prototype {

  public Warlord() {
  }

  public Warlord(Warlord source) {
  }

  @Override
  public abstract Warlord copy();

  @Override
  public boolean equals(Object obj) {
    if (this == obj) {
      return true;
    }
    if (obj == null) {
      return false;
    }
    return getClass() == obj.getClass();
  }

}
```

抽象类的一些实现,每个具体实现都重写了原型接口的 ``copy()`` 可以实现对原型的复制

```java
public class ElfBeast extends Beast {

  private final String helpType;

  public ElfBeast(String helpType) {
    this.helpType = helpType;
  }

  public ElfBeast(ElfBeast elfBeast) {
    super(elfBeast);
    this.helpType = elfBeast.helpType;
  }

  @Override
  public ElfBeast copy() {
    return new ElfBeast(this);
  }

  @Override
  public String toString() {
    return "Elven eagle helps in " + helpType;
  }

  @Override
  public boolean equals(Object obj) {
    if (this == obj) {
      return true;
    }
    if (!super.equals(obj)) {
      return false;
    }
    if (getClass() != obj.getClass()) {
      return false;
    }
    var other = (ElfBeast) obj;
    if (helpType == null) {
      return other.helpType == null;
    }
    return helpType.equals(other.helpType);
  }

}

public class ElfMage extends Mage {

  private final String helpType;

  public ElfMage(String helpType) {
    this.helpType = helpType;
  }

  public ElfMage(ElfMage elfMage) {
    super(elfMage);
    this.helpType = elfMage.helpType;
  }

  @Override
  public ElfMage copy() {
    return new ElfMage(this);
  }

  @Override
  public String toString() {
    return "Elven mage helps in " + helpType;
  }

  @Override
  public boolean equals(Object obj) {
    if (this == obj) {
      return true;
    }
    if (!super.equals(obj)) {
      return false;
    }
    if (getClass() != obj.getClass()) {
      return false;
    }
    var other = (ElfMage) obj;
    if (helpType == null) {
      return other.helpType == null;
    }
    return helpType.equals(other.helpType);
  }
}

public class ElfWarlord extends Warlord {

  private final String helpType;

  public ElfWarlord(String helpType) {
    this.helpType = helpType;
  }

  public ElfWarlord(ElfWarlord elfWarlord) {
    super(elfWarlord);
    this.helpType = elfWarlord.helpType;
  }

  @Override
  public ElfWarlord copy() {
    return new ElfWarlord(this);
  }

  @Override
  public String toString() {
    return "Elven warlord helps in " + helpType;
  }

  @Override
  public boolean equals(Object obj) {
    if (this == obj) {
      return true;
    }
    if (!super.equals(obj)) {
      return false;
    }
    if (getClass() != obj.getClass()) {
      return false;
    }
    var other = (ElfWarlord) obj;
    if (helpType == null) {
      return other.helpType == null;
    }
    return helpType.equals(other.helpType);
  }
}

public class OrcBeast extends Beast {

  private final String weapon;

  public OrcBeast(String weapon) {
    this.weapon = weapon;
  }

  public OrcBeast(OrcBeast orcBeast) {
    super(orcBeast);
    this.weapon = orcBeast.weapon;
  }

  @Override
  public OrcBeast copy() {
    return new OrcBeast(this);
  }

  @Override
  public String toString() {
    return "Orcish wolf attacks with " + weapon;
  }

  @Override
  public boolean equals(Object obj) {
    if (this == obj) {
      return true;
    }
    if (!super.equals(obj)) {
      return false;
    }
    if (getClass() != obj.getClass()) {
      return false;
    }
    var other = (OrcBeast) obj;
    if (weapon == null) {
      return other.weapon == null;
    }
    return weapon.equals(other.weapon);
  }
}

public class OrcMage extends Mage {

  private final String weapon;

  public OrcMage(String weapon) {
    this.weapon = weapon;
  }

  public OrcMage(OrcMage orcMage) {
    super(orcMage);
    this.weapon = orcMage.weapon;
  }

  @Override
  public OrcMage copy() {
    return new OrcMage(this);
  }

  @Override
  public String toString() {
    return "Orcish mage attacks with " + weapon;
  }

  @Override
  public boolean equals(Object obj) {
    if (this == obj) {
      return true;
    }
    if (!super.equals(obj)) {
      return false;
    }
    if (getClass() != obj.getClass()) {
      return false;
    }
    var other = (OrcMage) obj;
    if (weapon == null) {
      return other.weapon == null;
    }
    return weapon.equals(other.weapon);
  }
}

public class OrcWarlord extends Warlord {

  private final String weapon;

  public OrcWarlord(String weapon) {
    this.weapon = weapon;
  }

  public OrcWarlord(OrcWarlord orcWarlord) {
    super(orcWarlord);
    this.weapon = orcWarlord.weapon;
  }

  @Override
  public OrcWarlord copy() {
    return new OrcWarlord(this);
  }

  @Override
  public String toString() {
    return "Orcish warlord attacks with " + weapon;
  }

  @Override
  public boolean equals(Object obj) {
    if (this == obj) {
      return true;
    }
    if (!super.equals(obj)) {
      return false;
    }
    if (getClass() != obj.getClass()) {
      return false;
    }
    var other = (OrcWarlord) obj;
    if (weapon == null) {
      return other.weapon == null;
    }
    return weapon.equals(other.weapon);
  }
}
```

抽象工厂

```java
public interface HeroFactory {

  Mage createMage();

  Warlord createWarlord();

  Beast createBeast();

}
```

具体的工厂实现类

```java
public class HeroFactoryImpl implements HeroFactory {

  private final Mage mage;
  private final Warlord warlord;
  private final Beast beast;

  /**
   * Constructor.
   */
  public HeroFactoryImpl(Mage mage, Warlord warlord, Beast beast) {
    this.mage = mage;
    this.warlord = warlord;
    this.beast = beast;
  }

  /**
   * Create mage.
   */
  public Mage createMage() {
    return mage.copy();
  }

  /**
   * Create warlord.
   */
  public Warlord createWarlord() {
    return warlord.copy();
  }

  /**
   * Create beast.
   */
  public Beast createBeast() {
    return beast.copy();
  }

}

```

## 3. 模式优点

Java自带的原型模式，是基于内存二进制流的复制。
使用深拷贝可以将对象状态复制一份，简化了对象的创建流程，在需要回滚的时候可以起到辅助作用

## 4. 模式缺点

需要对每一个类都实现复制的方法，方法位于各个实现类的内部，需要修改内部代码，违背了 “开闭原则” ，并且深拷贝需要对拷贝对象的所有引用都进行复制，是一件复杂的事情。
