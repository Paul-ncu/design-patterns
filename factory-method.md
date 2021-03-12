# 工厂方法模式

## 1. 模式作用

工厂方法模式是一种创建型模式，通过工厂方法去创建一个类，而不是指明确切的类来实现

---

## 2. 代码试例

接下来的试例是，一个Blacksmith(铁匠)接口，在工厂方法模式中是一个父类工厂，它有一个方法 ``manufactureWeapon`` 用来生产 Weapon (武器)，Weapon是客户需要的事物的抽象。

```java
public class App {

  private static final Logger LOGGER = LoggerFactory.getLogger(App.class);

  // 
  private final Blacksmith blacksmith;
  
  public App(Blacksmith blacksmith) {
    this.blacksmith = blacksmith;
  }
  
  /**
   * Program entry point.
   * 
   * @param args command line args
   */
  public static void main(String[] args) {
    // Lets go to war with Orc weapons
    var app = new App(new OrcBlacksmith());
    app.manufactureWeapons();
    
    // Lets go to war with Elf weapons
    app = new App(new ElfBlacksmith());
    app.manufactureWeapons();
  }
  
  private void manufactureWeapons() {
    var weapon = blacksmith.manufactureWeapon(WeaponType.SPEAR);
    LOGGER.info(weapon.toString());
    weapon = blacksmith.manufactureWeapon(WeaponType.AXE);
    LOGGER.info(weapon.toString());
  }
}
```

Blacksmith在这里是一个子类工厂的接口，通过实现不同的子类工厂可以创建不同的Weapon

```java
public interface Blacksmith {

  Weapon manufactureWeapon(WeaponType weaponType);

}
```

接下来是两个具体子类工厂的实现

```java
public class ElfBlacksmith implements Blacksmith {

  private static final Map<WeaponType, ElfWeapon> ELFARSENAL;

  static {
    ELFARSENAL = new HashMap<>(WeaponType.values().length);
    Arrays.stream(WeaponType.values()).forEach(type -> ELFARSENAL.put(type, new ElfWeapon(type)));
  }

  @Override
  public Weapon manufactureWeapon(WeaponType weaponType) {
    return ELFARSENAL.get(weaponType);
  }

}

public class OrcBlacksmith implements Blacksmith {

  private static final Map<WeaponType, OrcWeapon> ORCARSENAL;

  static {
    ORCARSENAL = new HashMap<>(WeaponType.values().length);
    Arrays.stream(WeaponType.values()).forEach(type -> ORCARSENAL.put(type, new OrcWeapon(type)));
  }
  
  @Override
  public Weapon manufactureWeapon(WeaponType weaponType) {
    return ORCARSENAL.get(weaponType);
  }
}
```

Weapon是用户所需要实现类的抽象

```java
public interface Weapon {

  WeaponType getWeaponType();

}
```

Weapon的两个具体实现类

```java
public class ElfWeapon implements Weapon {

  private final WeaponType weaponType;

  public ElfWeapon(WeaponType weaponType) {
    this.weaponType = weaponType;
  }

  @Override
  public String toString() {
    return "Elven " + weaponType;
  }

  @Override
  public WeaponType getWeaponType() {
    return weaponType;
  }
}

public class OrcWeapon implements Weapon {

  private final WeaponType weaponType;

  public OrcWeapon(WeaponType weaponType) {
    this.weaponType = weaponType;
  }

  @Override
  public String toString() {
    return "Orcish " + weaponType;
  }

  @Override
  public WeaponType getWeaponType() {
    return weaponType;
  }
}
```

```java
public enum WeaponType {

  SHORT_SWORD("short sword"), SPEAR("spear"), AXE("axe"), UNDEFINED("");

  private final String title;

  WeaponType(String title) {
    this.title = title;
  }

  @Override
  public String toString() {
    return title;
  }
}
```

---

## 3. 工厂方法的优先

工厂方法可以通过实现新的子类工厂去创建不同的Weapon，这就符合了 “开闭原则” ，相对于无法扩展的简单工厂模式，我们可以利用工厂方法更容易的去扩展我们的代码。

## 4. 工厂方法的缺点

每一个工厂只是负责生产一种产品，所以每当我们需要增加新的产品时，就需要创建新的工厂。这样的结果是会造成我们系统中类的类的个数成倍的增加，增加了系统的复杂性。

所以接下来我们会学习抽象工厂模式。
