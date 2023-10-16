# JavaSE-EffectivelyFinal

In Java, the concept of "effectively final" is related to lambda expressions and anonymous inner classes. 

When you use a local variable in a lambda expression or an anonymous inner class, the variable must be effectively final.

An **effectively final** variable is one whose value doesn't change after it's first assigned. 

This allows the variable to be used in lambda expressions and anonymous inner classes without having to explicitly declare it as final. 

The compiler treats effectively final variables in such contexts as if they were declared with the final keyword.

```java
public class LambdaExample {
    public static void main(String[] args) {
        // Regular variable
        int regularVar = 10;
        regularVar = 20; // This would cause an error if used in a lambda expression

        // Effectively final variable
        int effectivelyFinalVar = 30;
        
        // Lambda expression using effectively final variable
        Runnable runnable = () -> {
            System.out.println("Value: " + effectivelyFinalVar);
            // Uncommenting the line below would cause a compilation error
            // effectivelyFinalVar = 40;
        };

        // Anonymous inner class using effectively final variable
        Runnable anonymousRunnable = new Runnable() {
            @Override
            public void run() {
                System.out.println("Value: " + effectivelyFinalVar);
                // Uncommenting the line below would cause a compilation error
                // effectivelyFinalVar = 40;
            }
        };

        // Rest of the code...
    }
}
```

In this example, effectivelyFinalVar is effectively final because its value is not changed after the initial assignment.

Therefore, it can be used in the lambda expression and the anonymous inner class without any issues.

If you try to modify the value of effectivelyFinalVar inside the lambda expression or the anonymous inner class, the compiler will generate an error.

I hope this clarifies the concept of "effectively final" in Java lambda expressions!

# More examples of effectively final variables in Java

## Example 1: Using Local Variables

```java
public class LambdaExample {
    public static void main(String[] args) {
        int x = 10;
        int y = 20;

        // Lambda expression using effectively final variables
        MathOperation add = () -> {
            System.out.println("Sum: " + (x + y));
            // Uncommenting the line below would cause a compilation error
            // x = 30;
        };

        add.operate(); // Output: Sum: 30
    }
}

interface MathOperation {
    void operate();
}
```

In this example, x and y are effectively final because their values are not changed after the lambda expression is defined.

## Example 2: Loop Variable in Lambda

```java
import java.util.ArrayList;
import java.util.List;

public class LambdaExample {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");
        names.add("Charlie");

        // Lambda expression using effectively final loop variable
        names.forEach(name -> System.out.println("Length: " + name.length()));
    }
}
```

The loop variable name is effectively final in the lambda expression because it is implicitly treated as if it's declared final. 

If you try to modify name inside the lambda expression, it will result in a compilation error.

## Example 3: Anonymous Inner Class

```java
public class LambdaExample {
    public static void main(String[] args) {
        int z = 5;

        // Anonymous inner class using effectively final variable
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                System.out.println("Value: " + z);
                // Uncommenting the line below would cause a compilation error
                // z = 10;
            }
        };

        runnable.run(); // Output: Value: 5
    }
}
```

Here, z is effectively final in the anonymous inner class.

These examples showcase scenarios where variables are effectively final, allowing them to be used in lambda expressions and anonymous inner classes. 

If you try to modify these variables after their initial assignment, the compiler will raise an error.

# More advance topics about effectively final 

While the concept of effectively final is relatively straightforward, there are some advanced scenarios and considerations worth exploring:

## 1. Capturing Mutable Objects

When dealing with mutable objects, even if the reference is effectively final, the state of the object can still be changed. 

This can lead to unexpected behavior. Consider the following example:

```java
import java.util.ArrayList;
import java.util.List;

public class LambdaExample {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");

        // This lambda captures the reference to the list (effectively final)
        // However, the state of the list can be modified
        Runnable runnable = () -> {
            names.add("Charlie");
            System.out.println(names);
        };

        runnable.run(); // Output: [Alice, Bob, Charlie]
        System.out.println(names); // Output: [Alice, Bob, Charlie]
    }
}
```

In this example, the reference to the `names` list is effectively final, but the contents of the list are mutable. The lambda modifies the list, affecting its state outside the lambda.

## 2. Lambda Expressions in a Loop

When using lambda expressions within a loop, each iteration creates a new lambda instance. 

While the loop variable itself must be effectively final, the lambda instances can capture different values of the variable in each iteration. Consider:

```java
public class LambdaExample {
    public static void main(String[] args) {
        for (int i = 0; i < 5; i++) {
            // This lambda captures the value of 'i' (effectively final for each iteration)
            Runnable runnable = () -> System.out.print(i + " ");
            runnable.run(); // Output: 0 1 2 3 4
        }
    }
}
```

In this case, each lambda instance captures a different value of `i` for each iteration. This behavior can be surprising if not understood correctly.

## 3. Effectively Final in Streams

When using streams, the lambda expressions operate in a different context. 

The variables used in lambdas within streams must be effectively final, but they are subject to different scoping rules. Consider:

```java
import java.util.stream.IntStream;

public class LambdaExample {
    public static void main(String[] args) {
        int baseValue = 5;

        // This lambda captures 'baseValue' (effectively final)
        IntStream.range(1, 5)
            .map(i -> i * baseValue)
            .forEach(System.out::println);
    }
}
```

Here, `baseValue` is effectively final, but it's not explicitly declared as such. The scoping rules in streams are more permissive than in regular loops.

Understanding these nuances is crucial for using effectively final variables in more complex scenarios. 

It's important to be aware of potential side effects, especially when dealing with mutable objects or when using lambda expressions in loops and streams.
