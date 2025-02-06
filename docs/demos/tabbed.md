# Tabbing mit Docsify und Markdown

So sieht das Ganze aus:

<!-- tabs:start -->

#### ** Hello World in C **

```c
#include<stdio.h>

int main() {
	printf("Hello World\n");
	return 0;
}
```

#### ** Hello World in PHP **

```php
<html>
 <head>
  <title>PHP-Test</title>
 </head>
 <body>
 <?php echo '<p>Hallo Welt</p>'; ?>
 </body>
</html>
```

#### ** Hello World in Java **

```java
public class HelloWorld {

    public static void main(String[] args) {
        // Prints "Hello, World" to the terminal window.
        System.out.println("Hello, World");
    }

}
```

<!-- tabs:end -->

So wird es gemacht:

    ```markdown
    <!-- tabs:start -->

    #### ** Hello World in C **

    ```c
    #include<stdio.h>

    int main() {
        printf("Hello World\n");
        return 0;
    }
    ```

    #### ** Hello World in PHP **

    ```php
    <html>
    <head>
    <title>PHP-Test</title>
    </head>
    <body>
    <?php echo '<p>Hallo Welt</p>'; ?>
    </body>
    </html>
    ```

    #### ** Hello World in Java **

    ```java
    public class HelloWorld {

        public static void main(String[] args) {
            // Prints "Hello, World" to the terminal window.
            System.out.println("Hello, World");
        }

    }
    ```

    <!-- tabs:end -->
    ```