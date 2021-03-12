# 适配器模式

## 1. 模式作用

将一个类的接口转换成客户希望的另外一个接口，使得原本由于接口不兼容而不能一起工作的那些类能一起工作。

## 2. 实现方式

大家好，我是一艘艘渔船，要想驾驶我，你就必须要掌握我的 ``sail()`` 方法，否则你就不能开动我哦。

```java
final class FishingBoat {

  private static final Logger LOGGER = getLogger(FishingBoat.class);

  void sail() {
    LOGGER.info("The fishing boat is sailing");
  }

}
```

接下来我们的船长出现了，船长拥有非常丰富的驾驶经验，但是我们的船长可是从来都没有开过渔船的，这让船长很烦恼，该怎么办呢？这时船长找到了一很有经验的造船工，看看造船工会怎么帮助船长解决问题。

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

不负所望，在几天几夜的工作下，造船工发明了渔船适配器，拥有他，船长就能成功的驾驶渔船了，这个渔船适配器是根据船长熟悉的技能 ``RowingBoat`` 打造的，通过操作这个适配器，适配器会把船长熟悉的操作转换成对应渔船的操作，这样可以在不需要知道渔船怎么驾驶的情况下，船长也能成功开动渔船。

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
