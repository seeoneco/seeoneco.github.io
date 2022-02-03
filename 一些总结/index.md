# 一些总结

> 本文旨在记录一些工作中常用的Java知识点，进行汇总，记录。

### 关于Java的并行流和Lambda表达式在循环中的利用（Java8新特性）
> 假设一下业务场景：在一个List对象列表`List<IssuesDto>`，想要从列表中拿出符合条件的`IssuesDto`对象中的某些属性值。

正常的想法是循环List，在每一个List中获取单个对象，拿到值，塞到另外一个新的List中。这无疑是一个冗长的for循环。但是在新特性中，对于集合类型的操作，有种新的方式可以快速，简洁的拿到新的List，一行代码直接搞定。

方式一：（带return的Lambda表达式 + stream流）

```java
List<String> listDtoString = listDto.stream().map(ele -> {return ele.getId();}).collect(Collectors.toList());
```

方式二：（Lambda表达式 + stream流）

```java
List<String> listDtoString = listDto.stream().map(ele -> ele.getId()).collect(Collectors.toList());
```

方式三：（方法引用 + stream流）

```java
List<String> listDtoString = listDto.stream().map(IssuesDto::getId).collect(Collectors.toList());
```





### JSON字符串转成对象

```java
JSONObject.parseObject(data,xxx.class)
```



### 字符串转变成数组

```java
List<String> myList = Arrays.asList("a1", "a2", "b1", "c2", "c1");
```


