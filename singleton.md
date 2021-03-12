# 单例模式

## 1.模式作用

单例模式可确保该类在每个Java类加载器实例中只能有一个现有实例，并提供对其的全局访问。在开发过程中，很多时候我们都需要保持某个对象的唯一性，比如线程池

## 2. 代码试例

### 2.1 饿汉式

实现单例模式的方法有很多，第一种就是饿汉式，静态初始化的实现类可以确保线程安全，保证全局实现类的唯一性。

```java
public final class IvoryTower {

  /**
   * 提供私有的构造器，所以无法实例化该实现类
   */
  private IvoryTower() {
  }

  /**
   * 实例化该类为静态类
   */
  private static final IvoryTower INSTANCE = new IvoryTower();

  /**
   * 用户调用可以获得该实现类
   *
   * @return 单例模式的实现类
   */
  public static IvoryTower getInstance() {
    return INSTANCE;
  }
}
```

### 2.2基于枚举

该方法是线程安全的

```java
public enum EnumIvoryTower {

  INSTANCE;

  @Override
  public String toString() {
    return getDeclaringClass().getCanonicalName() + "@" + hashCode();
  }
}
```

### 2.3懒汉式

懒汉式，是在用户第一次需要改实现类时才开始创建的，懒汉式的缺点是效率较低，因为获取实现类的方法时同步的。

```java
public final class ThreadSafeLazyLoadedIvoryTower {

  private static volatile ThreadSafeLazyLoadedIvoryTower instance;

  private ThreadSafeLazyLoadedIvoryTower() {
    // 防止通过反射再次实例化
    if (instance == null) {
      instance = this;
    } else {
      throw new IllegalStateException("Already initialized.");
    }
  }

  /**
   * The instance doesn't get created until the method is called for the first time.
   */
  public static synchronized ThreadSafeLazyLoadedIvoryTower getInstance() {
    if (instance == null) {
      synchronized (ThreadSafeLazyLoadedIvoryTower.class) {
        if (instance == null) {
          instance = new ThreadSafeLazyLoadedIvoryTower();
        }
      }
    }
    return instance;
  }
}
```

### 2.4双重检测

```java
public final class ThreadSafeDoubleCheckLocking {

  private static volatile ThreadSafeDoubleCheckLocking instance;

  private static boolean flag = true;

  /**
   * 私有化构造器，防止客户端实例化
   */
  private ThreadSafeDoubleCheckLocking() {
    // 防止通过反射实例化
    if (flag) {
      flag = false;
    } else {
      throw new IllegalStateException("Already initialized.");
    }
  }

  /**
   * 获取实例.
   *
   * @return an instance of the class.
   */
  public static ThreadSafeDoubleCheckLocking getInstance() {
    
    var result = instance;
    // 检查单例实例是否被实例化
    // 如果已经被实例化，我们可以返回改实例
    if (result == null) {
      // 当它没有被实例化
      // 但是我们不能确保在同一时间内，它没有被其他线程实例化
      // 我们需要加锁去确保只会创建一个实例
      synchronized (ThreadSafeDoubleCheckLocking.class) {

        // 再次将实例赋值给局部变量，检查它是否被其他线程实例化
        // 如果它被实例化，直接返回之前被创建的实例

        result = instance;
        if (result == null) {
          // 只有第一次需要创建实例的时候才会创建该实例
          instance = result = new ThreadSafeDoubleCheckLocking();
        }
      }
    }
    return result;
  }
}
```

### 2.5静态内部类

根据Java语言的特性实现的单例模式

```java
public final class InitializingOnDemandHolderIdiom {

  /**
   * Private constructor.
   */
  private InitializingOnDemandHolderIdiom() {
  }

  /**
   * Singleton instance.
   *
   * @return Singleton instance
   */
  public static InitializingOnDemandHolderIdiom getInstance() {
    return HelperHolder.INSTANCE;
  }

  /**
   * Provides the lazy-loaded Singleton instance.
   */
  private static class HelperHolder {
    private static final InitializingOnDemandHolderIdiom INSTANCE =
        new InitializingOnDemandHolderIdiom();
  }
}

```
