# 1. 介绍

```shell
#　定义
使用 Lambda 表达式可以替代只有一个抽象函数的接口实现，告别匿名内部类，代码看起来更简洁易懂。

#  特点
1：函数式编程
2：参数类型自动推断
3：代码量少，简洁

#  优势
1：更简洁的代码
2：更容易的并行

# 函数式接口
只有一个抽象方法（Object类中的方法除外）的接口是函数式接口
'也就是说默认方法、静态方法、object类中的方法都不是抽象方法'

```

# 2. 函数式接口

```java
//案例代码：对集合分组
Map<Integer, List<ShopCar>> map = shopCars.stream().collect(Collectors.groupingBy(ShopCar::getGoodsId))
```

```shell
# 以下是必须掌握的7个接口
1.Supplier（程序=供应商，提供输出内容） 代表一个输出
2.Consumer （程序=消费者，消费输入的内容）代表一个输入
3.BiConsumer 代表两个输入
4.Function 代表一个输入，一个输出（一般输入和输出是不同类型的）
5.UnaryOperator 代表一个输入，一个输出（输入和输出是相同类型的）
6.BiFunction 代表两个输入，一个输出（一般输入和输出是不同类型的）
7.BinaryOperator 代表两个输入，一个输出（输入和输出是相同类型的）
```

# 3. 语法

![1574080221660](image\1574080221660.png)