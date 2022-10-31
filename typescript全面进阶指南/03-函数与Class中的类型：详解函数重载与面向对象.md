## 函数

### 函数的类型签名

```typescript
//方式一
const foo = (name:string):number => {
	return name.length
}

//方式二
type FuncFoo = (name:string) => number

const foo:FuncFoo = (name)=> {
	return name.length
}
```

### void

没有return的语句返回void

```typescript
function bar():void {

}
```

有return但没有返回值的应当标注为undefined

```typescript
function bar():undefined {
 return;
}
```

### 可选参数与rest参数

函数参数有可选参数以及默认值

注意**可选参数必须位于必选参数之后**

#### rest参数

rest参数就是个数组，使用数组或者元组来标注类型

### 重载

TS的重载更多的是函数名的重载，使用的的是一个实现

## Class

类的主要结构：

- 构造函数
- 属性
- 方法
- 访问符

### 修饰符

访问信修饰符：

- public：类，派生类，实例
- provite：类内部
- protucted：类内部，派生类

操作性修饰符：

- readonly

### 静态成员

- static:在类的内部，静态成员无法通过this来访问，必须通过Foo.staticHandler

```typescript
class Foo {
  static staticHandler() { }

  public instanceHandler() { }
}

//编译之后
var Foo = /** @class */ (function () {
    function Foo() {
    }
    Foo.staticHandler = function () { };
    Foo.prototype.instanceHandler = function () { };
    return Foo;
}());
```

- 静态成员直接被挂载在函数体山，而实例成员挂载在原型上
- 静态成员不会被实例继承，他始终只属于当前定义的这个类及其子类
- 原型对象上的实例成员则会沿着原型链进行传递，被继承

### 继承、实现、抽象类

#### 继承

基类、派生类

- 派生类可以访问基类中的public以及protected修饰符的基类成员

- 基类中的方法可以在派生类中被覆盖
  ```typescript
  //覆盖基类中的方法时可以使用关键字override
  class Base {
    printWithLove() { }
  }
  
  class Derived extends Base {
    override print() {
      // ...
    }
  }
  
  //在这里 TS 将会给出错误，因为尝试覆盖的方法并未在基类中声明。通过这一关键字我们就能确保首先这个方法在基类中存在，同时标识这个方法在派生类中被覆盖了。
  ```

  

- 派生类可以通过super访问到基类中的方法

#### 抽象类

关键字：abstract

我们必须完全实现这个抽象类的每一个抽象成员。需要注意的是，在 TypeScript 中**无法声明静态的抽象成员**。

抽象类中的成员也需要使用 abstract 关键字才能被视为抽象类成员，如这里的抽象方法。

抽象类跟接口十分相似

```typescript
abstract class AbsFoo {
  abstract absProp: string;
  abstract get absGetter(): string;
  abstract absMethod(name: string): string
}

class Foo implements AbsFoo {
  absProp: string = "linbudu"

  get absGetter() {
    return "linbudu"
  }

  absMethod(name: string) {
    return name
  }
}

--------------------------------------------------

interface FooStruct {
  absProp: string;
  get absGetter(): string;
  absMethod(input: string): string
}

class Foo implements FooStruct {
  absProp: string = "linbudu"

  get absGetter() {
    return "linbudu"
  }

  absMethod(name: string) {
    return name
  }
}
```

