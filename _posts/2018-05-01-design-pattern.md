---
layout: post
title:  "7种结构型设计模式介绍"
date:   2018-05-01 23:34:40 +0800
categories: jekyll update
---

设计模式是开发者们对于一些常见问题场景而总结的最佳实践方案。通常设计模式也代表了开发者的设计思路，是开发者之间重要的沟通语言。

设计模式类型众多且不断发展，我们最为常见的主要有3类：创建型、结构型、行为型。

类型 | 解释 | 种类
--- | --- | ---
创建型 | 实例化对象的一种方法。不直接使用new操作符，屏蔽了实现细节，从而可以更为灵活地决定为应用场景创建什么对象。 | 5
结构型 | 关注于对象之间的关系。通过继承、接口和组合等方式来达到特定的功能。 | 7
行为型 | 主要关注对象之间如果沟通。 | 11

## 结构性设计模式

### decorator（装饰器）

装饰器模式可以在不修改原有对象的基础上为对象添加额外功能，无论是动态还是静态的。一般装饰器模式对于践行单一责任原则比较有用，因为它通常可以根据不同的功能域来分类数据类型。

#### 案例

附近的山上有一个愤怒的巨魔。通常它是徒手，但有时它有武器。为了让巨魔拥有武器没有必要创造一个新的巨魔，而是可以用适当的武器动态装饰它。

![decorator](https://raw.githubusercontent.com/richardmars/blog/master/res/design-pattern/decorator.svg?sanitize=true)

```java
public interface Troll {
  void attack();
  int getAttackPower();
  void fleeBattle();
}
```

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

```java
public class ClubbedTroll implements Troll {

  private static final Logger LOGGER = LoggerFactory.getLogger(ClubbedTroll.class);

  private Troll decorated;

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

```java
// simple troll
Troll troll = new SimpleTroll();
troll.attack(); // The troll tries to grab you!
troll.fleeBattle(); // The troll shrieks in horror and runs away!

// change the behavior of the simple troll by adding a decorator
troll = new ClubbedTroll(troll);
troll.attack(); // The troll tries to grab you! The troll swings at you with a club!
troll.fleeBattle(); // The troll shrieks in horror and runs away!
```

- 可以方便的在运行时动态修改对象能力，从而拓展和管理对象功能。
- 缺点是会创建一些具有相似功能的对象。
- 真实案例：java.io.InputStream, java.io.OutputStream, java.io.Reader and java.io.Writer
