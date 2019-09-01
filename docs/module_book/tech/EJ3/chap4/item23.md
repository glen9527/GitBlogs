## Chapter 4. Classes and Interfaces（类和接口）

### Item 23: Prefer class hierarchies to tagged classes（类层次结构优于带标签的类）

Occasionally you may run across a class whose instances come in two or more flavors and contain a tag field indicating the flavor of the instance. For example, consider this class, which is capable of representing a circle or a rectangle:

有时候，你可能会遇到这样一个类，它的实例有两种或两种以上的样式，并且包含一个标签字段，指示实例的样式。例如，考虑这个类，它能够表示一个圆或一个矩形：

```
// Tagged class - vastly inferior to a class hierarchy!
class Figure {
    enum Shape {RECTANGLE, CIRCLE};

    // Tag field - the shape of this figure
    final Shape shape;

    // These fields are used only if shape is RECTANGLE
    double length;

    double width;

    // This field is used only if shape is CIRCLE
    double radius;

    // Constructor for circle
    Figure(double radius) {
        shape = Shape.CIRCLE;
        this.radius = radius;
    }

    // Constructor for rectangle
    Figure(double length, double width) {
        shape = Shape.RECTANGLE;
        this.length = length;
        this.width = width;
    }

    double area() {
        switch (shape) {
            case RECTANGLE:
                return length * width;
            case CIRCLE:
                return Math.PI * (radius * radius);
            default:
                throw new AssertionError(shape);
        }
    }
}
```

Such tagged classes have numerous（adj. 许多的，很多的） shortcomings. They are cluttered with boilerplate, including enum declarations, tag fields, and switch statements. Readability is further harmed because multiple implementations are jumbled together in a single class. Memory footprint is increased because instances are burdened with irrelevant fields belonging to other flavors. Fields can’t be made final unless constructors initialize irrelevant fields, resulting in more boilerplate. Constructors must set the tag field and initialize the right data fields with no help from the compiler: if you initialize the wrong fields, the program will fail at runtime. You can’t add a flavor to a tagged class unless you can modify its source file. If you do add a flavor, you must remember to add a case to every switch statement, or the class will fail at runtime. Finally, the data type of an instance gives no clue as to its flavor. In short, **tagged classes are verbose, error-prone, and inefficient.**

这样的标签类有许多缺点。它们充斥着样板文件，包括 enum 声明、标签字段和 switch 语句。可读性会进一步受损，因为多个实现在一个类中混杂在一起。内存占用增加了，因为实例被其他类型的不相关字段所累。除非构造函数初始化不相关的字段，导致更多的样板文件，否则字段不能成为 final。构造函数必须设置标签字段并初始化正确的数据字段，而不需要编译器的帮助：如果初始化了错误的字段，程序将在运行时失败。除非你能够修改它的源文件，否则你不能向标签类添加样式。如果你确实添加了一个样式，那么你必须记住为每个 switch 语句添加一个 case，否则类将在运行时失败。最后，实例的数据类型没有给出它任何关于风格的线索。简而言之，**标签类冗长、容易出错和低效。**

Luckily, object-oriented languages such as Java offer a far better alternative for defining a single data type capable of representing objects of multiple flavors: subtyping. **A tagged class is just a pallid（adj. 苍白的；暗淡的；无生气的） imitation of a class hierarchy.**

幸运的是，面向对象的语言（如 Java）提供了一个更好的选择来定义能够表示多种类型对象的单一数据类型：子类型。**标签的类只是类层次结构的（简单）的模仿。**

To transform a tagged class into a class hierarchy, first define an abstract class containing an abstract method for each method in the tagged class whose behavior depends on the tag value. In the Figure class, there is only one such method, which is area. This abstract class is the root of the class hierarchy. If there are any methods whose behavior does not depend on the value of the tag, put them in this class. Similarly, if there are any data fields used by all the flavors, put them in this class. There are no such flavor-independent methods or fields in the Figure class.

要将已标签的类转换为类层次结构，首先为标签类中的每个方法定义一个包含抽象方法的抽象类，其行为依赖于标签值。在 Figure 类中，只有一个这样的方法，即 area 方法。这个抽象类是类层次结构的根。如果有任何方法的行为不依赖于标签的值，请将它们放在这个类中。类似地，如果有任何数据字段被所有种类使用，将它们放在这个类中。在 Figure 类中没有这样的独立于味道的方法或字段。

Next, define a concrete subclass of the root class for each flavor of the original tagged class. In our example, there are two: circle and rectangle. Include in each subclass the data fields particular to its flavor. In our example, radius is particular to circle, and length and width are particular to rectangle. Also include in each subclass the appropriate implementation of each abstract method in the root class. Here is the class hierarchy corresponding to the original Figure class:

接下来，为原始标签类的每个类型定义根类的具体子类。在我们的例子中，有两个：圆形和矩形。在每个子类中包含特定于其风格的数据字段。在我们的例子中，半径是特定于圆的，长度和宽度是特定于矩形的。还应在每个子类中包含根类中每个抽象方法的适当实现。下面是原 Figure 类对应的类层次结构：

```
// Class hierarchy replacement for a tagged class
abstract class Figure {
    abstract double area();
}

class Circle extends Figure {
    final double radius;

    Circle(double radius) {
        this.radius = radius;
    }

    @Override
    double area() {
        return Math.PI * (radius * radius);
    }
}

class Rectangle extends Figure {
    final double length;
    final double width;

    Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    @Override
    double area() {
        return length * width;
    }
}
```

This class hierarchy（n. 层级；等级制度） corrects every shortcoming of tagged classes noted previously. The code is simple and clear, containing none of the boilerplate found in the original. The implementation of each flavor is allotted its own class, and none of these classes is encumbered by irrelevant data fields. All fields are final. The compiler ensures that each class’s constructor initializes its data fields and that each class has an implementation for every abstract method declared in the root class. This eliminates the possibility of a runtime failure due to a missing switch case. Multiple programmers can extend the hierarchy independently and interoperably without access to the source for the root class. There is a separate data type associated with each flavor, allowing programmers to indicate the flavor of a variable and to restrict variables and input parameters to a particular flavor.

这个类层次结构纠正了前面提到的标签类的所有缺点。代码简单明了，不包含原始代码中的样板文件。每种风格的实现都分配了自己的类，这些类中没有一个被不相关的数据字段拖累。所有字段为 final 字段。编译器确保每个类的构造函数初始化它的数据字段，并且每个类对于根类中声明的每个抽象方法都有一个实现。这消除了由于缺少开关情况而导致运行时失败的可能性。多个程序员可以独立地和互操作地扩展层次结构，而不需要访问根类的源。每种风格都有一个单独的数据类型，允许程序员指示变量的风格，并将变量和输入参数限制为特定的风格。

Another advantage of class hierarchies is that they can be made to reflect natural hierarchical relationships among types, allowing for increased flexibility and better compile-time type checking. Suppose the tagged class in the original example also allowed for squares. The class hierarchy could be made to reflect the fact that a square is a special kind of rectangle (assuming both are immutable):

类层次结构的另一个优点是，可以使它们反映类型之间的自然层次关系，从而提高灵活性和更好的编译时类型检查。假设原始示例中的标签类也允许使用正方形。类层次结构可以反映这样一个事实，即正方形是一种特殊的矩形（假设两者都是不可变的）：

```
class Square extends Rectangle {
  Square(double side) {
    super(side, side);
  }
}
```

Note that the fields in the above hierarchy are accessed directly rather than by accessor methods. This was done for brevity and would be a poor design if the hierarchy were public (Item 16).

注意，上面层次结构中的字段是直接访问的，而不是通过访问器方法访问的。这样做是为了简洁，如果层次结构是公共的，那么这将是一个糟糕的设计（Item-16）。

In summary, tagged classes are seldom appropriate. If you’re tempted to write a class with an explicit tag field, think about whether the tag could be eliminated and the class replaced by a hierarchy. When you encounter an existing class with a tag field, consider refactoring it into a hierarchy.

总之，标签类很少有合适的时候。如果你想用显式标签字段编写类，请考虑是否可以消除标签并用层次结构替换类。当你遇到带有标签字段的现有类时，请考虑将其重构为层次结构。