---
title: java_8_lamda_expression
date: 2016-07-07 16:14:07
tags: java 8, lambda
---
Lambda 表达式是在Java 8中被引入的，而且是Java 8中最大的功能。Lambda表达式促进了函数式编程，同时简化了开发。

# 语法
Lambda 表达式具有以下几个语法特征：
  * 无需声明变量类型 - 编译器能够根据变量的值推断出变量类型。
  * 无需括号包含参数 - 不需要对单个参数使用括号，但是对于多个参数依然需要使用括号来包含它们。
  * 无需花括号 － 如果只有一个表达式，那么就不需要使用
  * 无需使用关键字return － 编译器会自动将只有一行的表达式作为返回值，如果需要使用return则必须要求用花括号将其包含。

以下是上述语法的一个具体例子


```java
public class Java8Tester {
   public static void main(String args[]){
      Java8Tester tester = new Java8Tester();
        
      //with type declaration
      MathOperation addition = (int a, int b) -> a + b;
        
      //with out type declaration
      MathOperation subtraction = (a, b) -> a - b;
        
      //with return statement along with curly braces
      MathOperation multiplication = (int a, int b) -> { return a * b; };
        
      //without return statement and without curly braces
      MathOperation division = (int a, int b) -> a / b;
        
      System.out.println("10 + 5 = " + tester.operate(10, 5, addition));
      System.out.println("10 - 5 = " + tester.operate(10, 5, subtraction));
      System.out.println("10 x 5 = " + tester.operate(10, 5, multiplication));
      System.out.println("10 / 5 = " + tester.operate(10, 5, division));
        
      //with parenthesis
      GreetingService greetService1 = message ->
      System.out.println("Hello " + message);
        
      //without parenthesis
      GreetingService greetService2 = (message) ->
      System.out.println("Hello " + message);
        
      greetService1.sayMessage("Mahesh");
      greetService2.sayMessage("Suresh");
   }
    
   interface MathOperation {
      int operation(int a, int b);
   }
    
   interface GreetingService {
      void sayMessage(String message);
   }
    
   private int operate(int a, int b, MathOperation mathOperation){
      return mathOperation.operation(a, b);
   }
}
```

验证结果

```
$javac Java8Tester.java
```

运行

```
$java Java8Tester
```

运行结果

```
10 + 5 = 15
10 - 5 = 5
10 x 5 = 50
10 / 5 = 2
Hello Mahesh
Hello Suresh
```

从上述的例子中我们需要学习到的点

1. Lambda 表达式主要用于定义一个函数接口的内部实现。例如，一个接口只有一个方法。在上述的例子中，我们使用各种个样的Lambda 表达式定义MathOperation 接口的操作。然后我们定义了GreetingService中的sayMessage实现。
2. 有了Lambda表达式之后我们就不再需要匿名类，赋予java强大的函数编程的能力。




















