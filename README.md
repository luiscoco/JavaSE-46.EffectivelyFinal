# JavaSE-EffectivelyFinal

In Java, the concept of "effectively final" is related to lambda expressions and anonymous inner classes. 

When you use a local variable in a lambda expression or an anonymous inner class, the variable must be effectively final.

An effectively final variable is one whose value doesn't change after it's first assigned. 

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

