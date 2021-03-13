# 适配器模式

## 1. 模式作用

将一个类的接口转换成客户指定的另一接口，使原本不兼容的两个接口一起工作。

## 2. 实现方式

hello大家好，我是一艘艘渔船，想要驾驶我出海远行，就必须要掌握 ``sail()`` 方法才能操作我，否则我就无法启动。

```java
final class FishingBoat {

  private static final Logger LOGGER = getLogger(FishingBoat.class);

  void sail() {
    LOGGER.info("The fishing boat is sailing");
  }

}
```

一名经验丰富的船长被委派操作我们的渔船，可是对这个系统他一无所知。船长苦思冥想，找来了一群经验丰富的造船工出谋划策，看看我们的造船工面对不了解的系统怎么解决这个问题。

```java
public final class Captain {

  private RowingBoat rowingBoat;

  public Captain() {
  }

  public Captain(final RowingBoat boat) {
    this.rowingBoat = boat;
  }

  void setRowingBoat(final RowingBoat boat) {
    this.rowingBoat = boat;
  }

  void row() {
    rowingBoat.row();
  }

}
```

不负所望，在几天几夜的工作下，造船工发明了渔船适配器，适配器根据船长熟悉运用的技能``RowingBoat``制作，能迅速准确的将船长的操作转换成对应渔船的操作。拥有它，船长就可以在不需要学习渔船驾驶技能的情况下成功开动渔船。

```java
public interface RowingBoat {

  void row();

}

public class FishingBoatAdapter implements RowingBoat {

  private final FishingBoat boat;

  public FishingBoatAdapter() {
    boat = new FishingBoat();
  }

  public final void row() {
    boat.sail();
  }
}

```

## 3. 模式的优点和缺点
