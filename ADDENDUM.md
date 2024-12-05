# A primer on lambda expressions

Lambda expressions are a concise way to represent anonymous functions (i.e., functions without a name) in Java. They allow you to write clean, readable code, especially when passing behavior as an argument to a method (e.g., in functional programming or event handling).

A lambda expression consists of:

- Parameters: Enclosed in parentheses ().
- Arrow Token: The -> separates the parameters from the function body.
- Body: The function logic, which can be a single expression or a block of statements.

```java
(parameters) -> { body }
```

For example, this is a lambda with a single parameter that squares a number:

```java
x -> x * x
```

<br><br>

## Using lambda expressions with JavaFX event handlers
JavaFX provides a variety of event-handling interfaces, such as `EventHandler<ActionEvent>`. 

In [`Login.java`](src/basic/Login.java) we used this to detect a button click event:

```java
button.setOnAction(new EventHandler<ActionEvent>() {
    @Override
    public void handle(ActionEvent event) {
        System.out.println("Button clicked!");
    }
});
```

With lambdas, the handle method of the `EventHandler` interface can be replaced by a lambda, resulting in cleaner code.

```java
button.setOnAction(event -> System.out.println("Button clicked!"));
```

Here is what's happening:

- `event`: Represents the parameter of the handle method.
- `->`: Indicates this is a lambda.
- `System.out.println("Button clicked!")`: The body of the lambda, executed when the button is clicked.

### Warning: Unused parameter
Visual Studio Code might warn you that the `event` parameter is unused (underlined in yellow) in the body of the handler. 

We can use the `_` character in place of `event` to make it clear to the reader that the parameter is irrelevant for the logic within the lambda body. Using `_` signals, *“I don’t care about the parameter, and I don’t plan to use it.”*

Thus, we can rewrite the expression as:

```java
button.setOnAction(_ -> System.out.println("Button clicked!"));
```


### Example with code block
You can also put a block of code in the body of an lambda expression. For example, this code snippet monitors the status of a checkbox object (`someDataCheckbox`) and adds a data series (`someData`) to an existing chart (`myChart`):

```java
someDataCheckbox.setOnAction(new EventHandler<ActionEvent>() {
    @Override
    public void handle(ActionEvent event) {
        if (someDataCheckbox.isSelected()) {
            myChart.getData().add(someData);
        } else {
            myChart.getData().remove(someData);
        }
    }
});
```

Rewritten using a lambda expression, we have:

```java
someDataCheckbox.setOnAction(event -> {
    if (someDataCheckbox.isSelected()) {
        myChart.getData().add(someData);
    } else {
        myChart.getData().remove(someData);
    }
});
```

Deconstructing the above syntax, we have:

- The lambda `event -> { ... }` replaces the explicit implementation of the `EventHandler<ActionEvent>` interface.
- The handle method is directly expressed as the body of the lambda.
- `event`: Represents the `ActionEvent` passed when the checkbox is clicked. It’s unused in this case but can be used for more complex logic if needed.

Or, alternatively, with the cleaner `_` variable to deal with the unused parameter:

```java
someDataCheckbox.setOnAction(_ -> {
    if (someDataCheckbox.isSelected()) {
        myChart.getData().add(someData);
    } else {
        myChart.getData().remove(someData);
    }
});
```

The lambda directly toggles the `someData` series in the chart depending on the state of the `someDataCheckbox`.