## 内置类型：any、unknown、never

### 顶级类型：any和unknown

两者区别： 

- any可以在声明后再次接受任意类型的值，同时可以被赋值给任意其它类型的变量
- 一个 unknown 类型的变量可以再次赋值为任意其它类型，但只能赋值给 any 与 unknown 类型的变量

### 底层类型：never

无了直接

## 类型断言：警告编译器不准报错

as newType

- 将 any / unknown 类型断言到一个具体的类型：
  ```typescript
  let unknownVar: unknown;
  
  (unknownVar as { foo: () => {} }).foo();
  ```

- as 到any 跳过所有检查

- 在联合类型中断言至某个具体的分支

**类型断言的正确使用方式是，在 TypeScript 类型分析不正确或不符合预期时，将其断言为此处的正确类型**

### 双重断言

断言类型跟原类型相差过大的时候，typescript会报错，这时候可以先断言到unknown类型，再断言到预期类型

```typescript
const str: string = "linbudu";

(str as unknown as { handler: () => {} }).handler();

// 使用尖括号断言
(<{ handler: () => {} }>(<unknown>str)).handler();
```

### 非空断言

非空断言其实是类型断言的简化，它使用 `!` 语法，即 `obj!.func()!.prop` 的形式标记前面的一个声明一定是非空的（实际上就是剔除了 null 和 undefined 类型,如下

```typescript
declare const foo: {
  func?: () => ({
    prop?: number | null;
  })
};

foo.func().prop.toFixed();
```

### 使用技巧：作为代码提示的辅助工具

```typescript
interface IStruct {
  foo: string;
  bar: {
    barPropA: string;
    barPropB: number;
    barMethod: () => void;
    baz: {
      handler: () => Promise<void>;
    };
  };
}
//想要不那么完整的实现这个接口，可能会这样

const obj:IStruct = {}


//这样会报错，这时候使用类型断言，可以在保留类型提示的前提下，不那么完整的实现这个结构

const obj = <IStruct>{
  bar: {
    baz: {},
  },
};
```

