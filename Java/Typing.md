# Preface

Applying consistent and easy to read code benefits not only the code owner, but for the other people working or viewing the code to understand. This scratch was made to set the general rules for **Nara**'s codebase based in **Java** programming language. Keeping the code readable and clean enables other developers to be able to adapt, collaborate, and maintain the code while typing uniformly. Consistency helps scalability and extensibility of the code.

Remember that these are just some general rules to keep the code consistent, there might be an exemption in some other cases.

## General Concepts

Most of the time, the phrase **readability over reusability** comes on the way of thinking. While this mostly is true, why not combining both? Abstraction may be needed for extendable methods, but not all instruction snippets should be extracted into its own methods, only if it's being reused or just very long.

## Maven project structure

This rules applies for **Maven** based structures. I have been using Maven more often than **Gradle** because its simplicity for small scale projects.

```
src
├── main
│   ├── database               Database sources
│   ├── java                   Application/Library sources
│   ├── resources              Application/Library resources
│   ├── filters                Resource filter files
│   └── webapp                 Web application sources
│   
└── test
	├── java                   Test sources
	├── resources              Test resources
	└── filters                Test resource filter files
```

## Naming conventions, whitespaces, indentations, restrictions

| Nr  | Naming Rule            | Rule                                    | Example            |
|:---:| ---------------------- | --------------------------------------- | ------------------ |
|  1  | Classes and Interfaces | Should be nouns, capitalized            | `Canal`            |
|  2  | Interfaces             | Adjectives ending with "-able"          | `Traversable`      |
|  3  | Variables              | `camelCase` style, short but meaningful | `isClosed`         |
|  4  | Constant Variables     | Capitalized, separated with underscores | `DEFAULT_WIDTH`    |
|  5  | Packages               | lowercase letters                       | `sample.naming`    |
|  6  | Variable Assignments   | Whitespaces around `=`                  | `this.id = id`     |
|  7  | Methods and Classes    | No whitespace after the name            | `Canal(String id)` |

Other than that, **indentations** are also important to separate scopes of codes. In most cases, 4 characters indent to tab out between scopes. I rarely use 2 or 8 characters indentations.

```
package sample.naming;  
  
interface Traversable {  
   void traverse();  
}  
  
public class Canal implements Traversable {  
  
   public static final int DEFAULT_DISTANCE = 40;  
   private String id;  
   protected int distance;  
   private boolean isClosed = false;  
  
   public Canal(String id) {  
      // Calls the other constructor  
      this(id, DEFAULT_DISTANCE);  
   }  
  
   public Canal(String id, int distance) {  
      this.id = id;  
      this.distance = distance;  
   }  
  
   public void traverse() {  
      System.out.println("Traversing: " + id + ", distance: " + distance);  
   }  
  
   public String getId() {  
      return id;  
   }
}
```

### Method declarations

A method should be short, understandable, and meaningful. Parameters should also be named according to the context. Some examples:

```
// Boolean methods may start with "has" or "is" following its context
public boolean hasGate() {  
   return gateId != null;  
}
```

```
// This boolean method uses "can" because the context is able or not able to.
public static boolean canConnectToDatabase() {  
   try {  
      getConnection();  
      return true;  
   } catch (SQLException e) {  
      return false;  
   }  
}
```

```
// Varargs may be used for repeating values (can be multiple single values)
public int addInts(int... values) {  
   int sum = 0;  
   for (int value : values) {  
      sum += value;  
   }  
   return sum;  
}
```

### Method parameter restriction

Parameters are necessary when working with methods. However, care should be taken to avoid using too many parameters in one method. Too many parameters may indicate that your method is addressing more than one concern and violates the single responsibility principle. 

Too many method parameters make your code less readable since keeping track of their types and meanings is difficult. To write clean Java code, you should: limit the number of method parameters and use objects or data structures instead of individual parameters or group-related parameters into objects.

Here is an example of a Java method with too many method parameters:

```
public void processOrder(String customerName, String shippingAddress, String billingAddress, String productName,   
                   int quantity, double price, boolean isExpressShipping) {  
   // Method implementation  
}
```

Refactoring the code by grouping it into one single class improves readability:

```
public class Order {
	private String customerName;
	private String shippingAddress;
	private String billingAddress;
	private String productName;
	private int quantity;
	private double price;
	private boolean isExpressShipping;
	
	// Constructors, getters, and setters
	
	// Other methods related to an order
}

public void processOrder(Order order) {
	// Method implementation
}
```

source: [digma.ai](https://digma.ai/blog/clean-code-java/)

### Code width restriction

The code must be inside the **Right Margin** or the **Visual Guides** line. This line is a visual guide that helps you keep your code within a specified width or column limit. It's a common practice to limit the line length in code to improve readability and make it easier to view side-by-side in various environments. If the width is too large, it will be hard to read the rest of the code in a side-by-side environments. Continue the statement in the next line.
## Documentation

### Javadoc

Java provides simple built-in documentation feature called Javadoc and comments. Every classes and methods should have a Javadoc above it, elaborating the context concisely. For methods, tags such as `@param` and `@return` may be used to describe the parameters instead of the main description. `@link` tag may also be used to reference Classes, its methods, and its attributes. Below is the table for a simple use of the tag:

| Tag use                      | Action                                     |
| ---------------------------- | ------------------------------------------ |
| `{@link HashMap}`            | References the class `HashMap`             |
| `{@link HashMap#values}`     | Access the `values` attribute of the class |
| `{@link HashMap#toString()}` | Calls the method `toString` of the class   | 

Example:

```
/**  
 * Mendapatkan data {@link Peminjam} dari hasil pertama {@link ResultSet} yang diberikan.  
 * 
 * @param rs {@link ResultSet} yang akan diambil datanya.  
 * @return {@link Peminjam} jika ada, {@code null} jika tidak ada.  
 * @throws {@link SQLException} exception yang kemungkinan dinaikan.
 * */
private static Peminjam getPeminjam(ResultSet rs) throws SQLException {
	// ...
}
```

### Comments

A comment briefly explains a snippet of code or set of instructions. It is used inside method scopes. Every line does not always need its own comment if unnecessary/obvious.

```
public static void printTest() {  
   HashMap<String, Peminjam> dataPeminjam = readAllDataPeminjam();  
   // Converts the map values into a list of Peminjam  
   var arr = List.copyOf(dataPeminjam.values());  
  
   // Prints all elements of Peminjam in arr  
   for (Peminjam p : arr) {  
      System.out.println(p);  
   }  
}
```

```
public static void main(String[] args) {
	/* 
	 * This is a multi-line comment. 
	 * It can span several lines. 
	 */
	System.out.println("Hello, World!"); 
}
```

