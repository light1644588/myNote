### 类 `class`

- 继承

  ```typescript
  class Animal {
      move(distanceInMeters: number = 0) {
          console.log(`Animal moved ${distanceInMeters}m.`);
      }
  }
  
  class Dog extends Animal {
      bark() {
          console.log('Woof! Woof!');
      }
  }
  
  const dog = new Dog();
  ```

  最基本的继承：类从基类中继承了属性和方法，`Dog`通过`extends`继承基类`Animal`，基类又被称为超类。

  ```typescript
  class Animal {
      name: string;
      constructor(theName: string) { this.name = theName; }
      move(distanceInMeters: number = 0) {
          console.log(`${this.name} moved ${distanceInMeters}m.`);
      }
  }
  
  class Snake extends Animal {
      constructor(name: string) { super(name); }
      move(distanceInMeters = 5) {
          console.log("Slithering...");
          super.move(distanceInMeters);
      }
  }
  
  class Horse extends Animal {
      constructor(name: string) { super(name); }
      move(distanceInMeters = 45) {
          console.log("Galloping...");
          super.move(distanceInMeters);
      }
  }
  
  let sam = new Snake("Sammy the Python");
  let tom: Animal = new Horse("Tommy the Palomino");
  
  sam.move();
  tom.move(34);
  ```

  `super`关键字指超类，`super()`会执行基类(例如：`Horse`是派生类，`Animal`是超类)的构造函数，且在访问构造函数内访问this的属性之前，我们必须调用`super()`。`ts`会强制执行

- 公共，私有域受保护的修饰符

  类中属性默认为公共属性 **`public`**

  ```typescript
  class Animal {
      public name: string;
      public constructor(theName: string) { this.name = theName; }
      public move(distanceInMeters: number) {
          console.log(`${this.name} moved ${distanceInMeters}m.`);
      }
  }
  ```

  ****

  **`private`**关键字修饰后的属性为私有属性，不能再声明它的类的外部访问。

  ```typescript
  class Animal {
      private name: string;
      constructor(theName: string) { this.name = theName; }
  }
  
  new Animal("Cat").name; // 错误: 'name' 是私有的.
  ```

  `TypeScript`使用的是结构性类型系统。 当我们比较两种不同的类型时，并不在乎它们从何处而来，如果所有成员的类型都是兼容的，我们就认为它们的类型是兼容的。

  然而，当我们比较带有 `private`或 `protected`成员的类型的时候，情况就不同了。 如果其中一个类型里包含一个 `private`成员，那么只有当另外一个类型中也存在这样一个 `private`成员， **并且它们都是来自同一处声明时**，我们才认为这两个类型是兼容的。 对于 `protected`成员也使用这个规则。

  ```typescript
  class Animal {
      private name: string;
      constructor(theName: string) { this.name = theName; }
  }
  
  class Rhino extends Animal {
      constructor() { super("Rhino"); }
  }
  
  class Employee {
      private name: string;
      constructor(theName: string) { this.name = theName; }
  }
  
  let animal = new Animal("Goat");
  let rhino = new Rhino();
  let employee = new Employee("Bob");
  
  animal = rhino;
  animal = employee; // 错误: Animal 与 Employee 不兼容.
  ```

  这个例子中有 `Animal`和 `Rhino`两个类， `Rhino`是 `Animal`类的子类。 还有一个 `Employee`类，其类型看上去与 `Animal`是相同的。 我们创建了几个这些类的实例，并相互赋值来看看会发生什么。 因为 `Animal`和 `Rhino`共享了来自 `Animal`里的私有成员定义 `private name: string`，因此它们是兼容的。 然而 `Employee`却不是这样。当把 `Employee`赋值给 `Animal`的时候，得到一个错误，说它们的类型不兼容。 尽管 `Employee`里也有一个私有成员 `name`，但它明显不是 `Animal`里面定义的那个。

  ****

  **`protected`**关键字和`private`类似，但被该关键字修饰后的属性可在派生类中访问

  ```typescript
  class Person {
      protected name: string;
      constructor(name: string) { this.name = name; }
  }
  
  class Employee extends Person {
      private department: string;
  
      constructor(name: string, department: string) {
          super(name)
          this.department = department;
      }
  
      public getElevatorPitch() {
          return `Hello, my name is ${this.name} and I work in ${this.department}.`;
      }
  }
  
  let howard = new Employee("Howard", "Sales");
  console.log(howard.getElevatorPitch());
  console.log(howard.name); // 错误
  ```

  注意，我们不能在 `Person`类外使用 `name`，但是我们仍然可以通过 `Employee`类的实例方法访问，因为 `Employee`是由 `Person`派生而来的。

  构造函数也可以被标记成 `protected`。 这意味着这个类不能在包含它的类外被实例化，但是能被继承。

  ```typescript
  class Person {
      protected name: string;
      protected constructor(theName: string) { this.name = theName; }
  }
  
  // Employee 能够继承 Person
  class Employee extends Person {
      private department: string;
  
      constructor(name: string, department: string) {
          super(name);
          this.department = department;
      }
  
      public getElevatorPitch() {
          return `Hello, my name is ${this.name} and I work in ${this.department}.`;
      }
  }
  
  let howard = new Employee("Howard", "Sales");
  let john = new Person("John"); // 错误: 'Person' 的构造函数是被保护的.
  ```

  ****

  **`readonly`**

  你可以使用 `readonly`关键字将属性设置为只读。 只读属性必须在声明时或构造函数里被初始化

  ```typescript
  class Octopus {
      readonly name: string;
      readonly numberOfLegs: number = 8;
      constructor (theName: string) {
          this.name = theName;
      }
  }
  let dad = new Octopus("Man with the 8 strong legs");
  dad.name = "Man with the 3-piece suit"; // 错误! name 是只读的.
  ```

  **存取器**

  `TypeScript`支持通过`getters`/`setters`来截取对对象成员的访问。 它能帮助你有效的控制对对象成员的访问。

  ```typescript
  let passcode = "secret passcode";
  
  class Employee {
      private _fullName: string;
  
      get fullName(): string {
          return this._fullName;
      }
  
      set fullName(newName: string) {
          if (passcode && passcode == "secret passcode") {
              this._fullName = newName;
          }
          else {
              console.log("Error: Unauthorized update of employee!");
          }
      }
  }
  
  let employee = new Employee();
  employee.fullName = "Bob Smith";
  if (employee.fullName) {
      alert(employee.fullName);
  }
  ```

  ###### 注意

  首先，存取器要求你将编译器设置为输出`ECMAScript 5`或更高。 不支持降级到`ECMAScript 3`。 其次，只带有 `get`不带有 `set`的存取器自动被推断为 `readonly`。 这在从代码生成 `.d.ts`文件时是有帮助的，因为利用这个属性的用户会看到不允许够改变它的值。

  **以上属性全部是类的实例属性，只有类被实例之后才会初始化的属性**。

  ****

- 静态属性 `static`关键字

- 抽象类 `abstract`关键字

  抽象类做为其它派生类的基类使用。 它们一般不会直接被实例化。 不同于接口，抽象类可以包含成员的实现细节。 `abstract`关键字是用于定义抽象类和在抽象类内部定义抽象方法。

  ```typescript
  abstract class Animal {
      abstract makeSound(): void;
      move(): void {
          console.log('roaming the earch...');
      }
  }
  ```

  抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。 抽象方法的语法与接口方法相似。 两者都是定义方法签名但不包含方法体。 然而，抽象方法必须包含 `abstract`关键字并且可以包含访问修饰符。

  ```typescript
  abstract class Department {
  
      constructor(public name: string) {
      }
  
      printName(): void {
          console.log('Department name: ' + this.name);
      }
  
      abstract printMeeting(): void; // 必须在派生类中实现
  }
  
  class AccountingDepartment extends Department {
  
      constructor() {
          super('Accounting and Auditing'); // 在派生类的构造函数中必须调用 super()
      }
  
      printMeeting(): void {
          console.log('The Accounting Department meets each Monday at 10am.');
      }
  
      generateReports(): void {
          console.log('Generating accounting reports...');
      }
  }
  
  let department: Department; // 允许创建一个对抽象类型的引用
  department = new Department(); // 错误: 不能创建一个抽象类的实例
  department = new AccountingDepartment(); // 允许对一个抽象子类进行实例化和赋值
  department.printName();
  department.printMeeting();
  department.generateReports(); // 错误: 方法在声明的抽象类中不存在
  ```

****

### 函数

- 概念

  `TypeScript`函数可以创建有名字的函数和匿名函数。 你可以随意选择适合应用程序的方式，不论是定义一系列`API`函数还是只使用一次的函数。

- 函数类型

  我们可以给每个参数添加类型之后再为函数本身添加返回值类型。 `TypeScript`能够根据返回语句自动推断出返回值类型，因此我们通常省略它。

  为函数定义类型

  ```typescript
  let myAdd: (x: number, y: number) => number =
      function(x: number, y: number): number { return x + y; };
  ```

  函数类型包含两部分：参数类型和返回值类型。 当写出完整函数类型的时候，这两部分都是需要的。 我们以参数列表的形式写出参数类型，为每个参数指定一个名字和类型。 这个名字只是为了增加可读性。 

- 类型推断

  尝试这个例子的时候，你会发现如果你在赋值语句的一边指定了类型但是另一边没有类型的话，`TypeScript`编译器会自动识别出类型。这叫做“按上下文归类”，是类型推论的一种。

- 可选参数和默认参数

  `TypeScript`里的每个函数参数都是必须的。 这不是指不能传递 `null`或`undefined`作为参数，而是说编译器检查用户是否为每个参数都传入了值。 编译器还会假设只有这些参数会被传递进函数。 简短地说，传递给一个函数的参数个数必须与函数期望的参数个数一致。

  `JavaScript`里，每个参数都是可选的，可传可不传。 没传参的时候，它的值就是`undefined`。 在`TypeScript`里我们可以在参数名旁使用 `?`实现可选参数的功能。

  ```typescript
  function buildName(firstName: string, lastName?: string) {
      if (lastName)
          return firstName + " " + lastName;
      else
          return firstName;
  }
  
  let result1 = buildName("Bob");  // works correctly now
  let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
  let result3 = buildName("Bob", "Adams");  // ah, just right
  ```

  在所有必须参数后面的带默认初始化的参数都是可选的，与可选参数一样，在调用函数的时候可以省略。也就是说可选参数与末尾的默认参数共享参数类型。

- 剩余参数

  必要参数，默认参数和可选参数有个共同点：它们表示某一个参数。 有时，你想同时操作多个参数，或者你并不知道会有多少参数传递进来。 在JavaScript里，你可以使用 `arguments`来访问所有传入的参数。在`Typescript`中，你可以把所有参数收集到一个变量里：

  ```typescript
  function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
  }
  
  let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
  ```

  剩余参数会被当做个数不限的可选参数。 可以一个都没有，同样也可以有任意个。 编译器创建参数数组，名字是你在省略号（ `...`）后面给定的名字，你可以在函数体内使用这个数组。

  这个省略号也会在带有剩余参数的函数类型定义上使用到：

  ```typescript
  function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
  }
  
  let buildNameFun: (fname: string, ...rest: string[]) => string = buildName;
  ```

- 重载

  方法根据传入参数的不同，调用不同的函数

****

### 泛型

- 使用情景

  使用`any`类型会导致这个函数可以接收任何类型的`arg`参数，这样就丢失了一些信息：传入的类型与返回的类型应该是相同的。如果我们传入一个数字，我们只知道任何类型的值都有可能被返回。因此，我们需要一种方法使返回值的类型与传入参数的类型是相同的。 这里，我们使用了 *类型变量*，它是一种特殊的变量，只用于表示类型而不是值。

  ```typescript
  function identity<T>(arg: T): T {
      return arg;
  }
  ```

  我们给`identity`添加了类型变量`T`。 `T`帮助我们捕获用户传入的类型（比如：`number`），之后我们就可以使用这个类型。 之后我们再次使用了 `T`当做返回值类型。现在我们可以知道参数类型与返回值类型是相同的了。 这允许我们跟踪函数里使用的类型的信息。

  我们把这个版本的`identity`函数叫做泛型，因为它可以适用于多个类型。 不同于使用 `any`，它不会丢失信息，像第一个例子那像保持准确性，传入数值类型并返回数值类型。

- 使用泛型变量

  使用泛型创建像`identity`这样的泛型函数时，编译器要求你在函数体必须正确的使用这个通用的类型。 换句话说，你必须把这些参数当做是任意或所有类型

  ```typescript
  function loggingIdentity<T>(arg: Array<T>): Array<T> {
      console.log(arg.length);  // Array has a .length, so no more error
      return arg;
  }
  ```

- 泛型类型

  泛型函数的类型与非泛型函数的类型没什么不同，只是有一个类型参数在最前面，像函数声明一样：

  ```typescript
  function identity<T>(arg: T): T {
      return arg;
  }
  
  let myIdentity: <T>(arg: T) => T = identity;
  ```

  我们也可以使用不同的泛型参数名，只要在数量上和使用方式上能对应上就可以。

  ```typescript
  function identity<T>(arg: T): T {
      return arg;
  }
  
  let myIdentity: <U>(arg: U) => U = identity;
  ```

  我们还可以使用带有调用签名的对象字面量来定义泛型函数：

  ```typescript
  function identity<T>(arg: T): T {
      return arg;
  }
  
  let myIdentity: {<T>(arg: T): T} = identity;
  ```

  这引导我们去写第一个泛型接口了。 我们把上面例子里的对象字面量拿出来做为一个接口：

  ```typescript
  interface GenericIdentityFn {
      <T>(arg: T): T;
  }
  
  function identity<T>(arg: T): T {
      return arg;
  }
  
  let myIdentity: GenericIdentityFn = identity;
  ```

  我们可能想把泛型参数当作整个接口的一个参数。 这样我们就能清楚的知道使用的具体是哪个泛型类型（比如： `Dictionary<string>而不只是Dictionary`）。 这样接口里的其它成员也能知道这个参数的类型了。

  ```typescript
  interface GenericIdentityFn<T> {
      (arg: T): T;
  }
  
  function identity<T>(arg: T): T {
      return arg;
  }
  
  let myIdentity: GenericIdentityFn<number> = identity;
  ```

  注意，示例做了少许改动。 不再描述泛型函数，而是把非泛型函数签名作为泛型类型一部分。 当我们使用 `GenericIdentityFn`的时候，还得传入一个类型参数来指定泛型类型（这里是：`number`），锁定了之后代码里使用的类型。 对于描述哪部分类型属于泛型部分来说，理解何时把参数放在调用签名里和何时放在接口上是很有帮助的。

- 泛型类

  泛型类看上去与泛型接口差不多。 泛型类使用（ `<>`）括起泛型类型，跟在类名后面。

  ```typescript
  class GenericNumber<T> {
      zeroValue: T;
      add: (x: T, y: T) => T;
  }
  
  let myGenericNumber = new GenericNumber<number>();
  myGenericNumber.zeroValue = 0;
  myGenericNumber.add = function(x, y) { return x + y; };
  ```

  和接口一样，直接把泛型放在类后面，可以帮助确认类的所有属性都在使用相同的类型。

  类有两部分：静态部分和实例部分。 泛型类指的是实例部分的类型，所以类的静态属性不能使用这个泛型类型。

- 泛型约束

  你应该会记得之前的一个例子，我们有时候想操作某类型的一组值，并且我们知道这组值具有什么样的属性。 在 `loggingIdentity`例子中，我们想访问`arg`的`length`属性，但是编译器并不能证明每种类型都有`length`属性，所以就报错了。

  ```typescript
  function loggingIdentity<T>(arg: T): T {
      console.log(arg.length);  // Error: T doesn't have .length
      return arg;
  }
  ```

  相比于操作any所有类型，我们想要限制函数去处理任意带有`.length`属性的所有类型。 只要传入的类型有这个属性，我们就允许，就是说至少包含这一属性。 为此，我们需要列出对于T的约束要求。

  为此，我们定义一个接口来描述约束条件。 创建一个包含 `.length`属性的接口，使用这个接口和`extends`关键字来实现约束：

  ```typescript
  interface Lengthwise {
      length: number;
  }
  
  function loggingIdentity<T extends Lengthwise>(arg: T): T {
      console.log(arg.length);  // Now we know it has a .length property, so no more error
      return arg;
  }
  ```

- 在泛型约束中使用类型参数

  

#### 泛型

定义函数或类时，遇到类型不明确就可以使用泛型

```tsx
/**
 * T 表示任意类型 T表示泛型
 */
function func<T>(a: T): T{
    return a
}
```

```typescript
function identity<T>(arg: T): T {
    return arg
}
/**
 * T 表示任意类型 T表示泛型
 */
function func<T>(a: T): T{
    return a
}

// 直接调用具有泛型的函数
func(10) // 不指定泛型，但ts可自动对类型推断
func<String>('hello') // 指定泛型

function func1<T, K>(a: T, b:K): T {
    return a
}

func1<number, string>(123, 'hello');

// 限制泛型的范围
interface Inter {
    length: number
}
// T extends Inter 泛型T必须是Inter实现类（子类）
function fun3<T extends Inter>(a: T):number {
    return a.length
}

fun3({name: 'hello', length:0})

class MyClass<T> {
    name: T;
    constructor(name: T) {
        this.name = name
    }
}

const mc = new MyClass<string>('alice')
```

使用场景：当不知道使用函数时的参数类型的时候可以使用泛型

```typescript
function identity<T extends LengthWise>(arg: T): T {
     console.log(arg.length);    
     return arg
}
```

泛型既可以当作类型又可以当作实体类