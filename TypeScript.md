# TypeScript

## 环境搭建



## 基础语法

### 联合类型： number | string 



Falsy 值是在条件语句中被视为 false 的值。在 JavaScript 和 TypeScript 中，以下值被视为 falsy 值：

1. `false`: 布尔值 `false`。
2. `0`: 数字 0。
3. `-0`: 负零。
4. `0n`: BigInt 的零。
5. `""` 或 `''`: 空字符串。
6. `null`: 空值。
7. `undefined`: 未定义的值。
8. `NaN`: 不是一个数字。

这些值在条件判断时被视为 false，当它们作为条件表达式的计算结果时，会导致控制流跳转到 false 分支，或者执行条件表达式后面的代码。

除了以上 falsy 值之外，其他值都被视为 truthy 值，即它们在条件判断时被视为 true。

以下是一个示例：

```typescript
const value: number = 0;

if (value) {
  console.log("Value is truthy");
} else {
  console.log("Value is falsy");
}
```

在这个例子中，`value` 的值为 0，它是一个 falsy 值。因此，控制流会跳转到 `else` 分支，输出 "Value is falsy"。

### 交叉类型



![image-20230704135350055](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230704135350055.png)





**一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集**



### 在ts中怎么判断到底用=还是：

在 TypeScript 中，根据变量的使用情况和赋值的类型，可以通过以下几种方式来判断使用 `=` 还是 `:`：

1. 变量赋值：当你将一个值赋给一个变量时，使用 `=`。例如：
```typescript
let age = 25;
```

2. 函数参数和返回类型：在函数定义时，使用 `:` 来定义参数的类型和函数的返回类型。例如：
```typescript
function greet(name: string): string {
    return `Hello, ${name}!`;
}
```

3. 对象属性和方法的类型注解：在对象字面量中，或者在定义对象类型时，使用 `:` 来注解对象的属性和方法的类型。例如：
```typescript
let person: { name: string, age: number } = {
    name: 'Tom',
    age: 25
};
```

4. 函数类型注解：当你定义一个函数类型时，使用 `:` 来注解参数的类型和返回类型。例如：
```typescript
type GreetFunction = (name: string) => string;
```

需要注意的是，使用 `:` 进行类型注解时，通常是在变量声明或函数定义的上下文中，并且用于确定变量或函数的类型。而使用 `=` 进行变量赋值时，用于把一个值赋给一个变量。

希望以上解释可以帮到你理解在 TypeScript 中使用 `=` 和 `:` 的不同情况和用途。

