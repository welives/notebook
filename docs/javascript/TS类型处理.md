## Record

定义一个对象的 key 和 value 类型

```ts
// 源码
type Record<K extends string | number | symbol, T> = { [P in K]: T }

// 例子
type NeverObj = Record<string, never> // 空对象
type AnyObj = Record<string, unknown> // 任意对象
type UserInfo = {
  name: string
  email: string
}
const user: Record<string, UserInfo> = {
  uid1: { name: 'john', email: 'john@gmail.com' },
}
```

---

## Partial

将某个类型里的所有属性变成可选，并返回新类型

```ts
// 源码
type Partial<T> = { [P in keyof T]?: T[P] }

// 例子
interface Todo {
  title: string
  desc: string
}
const todo: Partial<Todo> = { title: 'Learn TypeScript' }
```

---

## Required

将某个类型里的所有属性变成必选，并返回新类型

```ts
// 源码
type Required<T> = { [P in keyof T]-?: T[P] }

// 例子
interface Todo {
  title?: string
  desc?: string
}
const todo: Required<Todo> = { title: 'Learn TypeScript', desc: '学习类型处理' }
```

---

## Pick

从某个类型中取出部分属性，并返回新类型

```ts
// 源码
type Pick<T, K extends keyof T> = { [P in K]: T[P] }

// 例子
interface User {
  id: number
  readonly name: string
  age: number
  sex: 0 | 1
}
const user: Pick<User, 'id' | 'name'> = { id: 1, name: 'jandan' }
```

---

## Omit

从某个类型中省略部分属性，并返回新类型

```ts
// 源码
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>

// 例子
interface User {
  id: number
  readonly name: string
  age: number
  sex: 0 | 1
}
const user: Omit<User, 'age' | 'sex'> = { id: 1, name: 'jandan' }
```

---

## Exclude

类型排除，通常用于复杂类型。它接受两个类型参数，从`参数1`中排除掉`参数2`类型中的所有属性(`说人话就是过滤出参数1中独有的属性`)，并返回新类型

```ts
// 源码
type Exclude<T, U> = T extends U ? never : T

// 例子
type A = object | string | number | boolean
type B = number | boolean
type C = Exclude<A, B> // string | object
type D = Exclude<B, A> // never

type Shapes =
  | {
      kind: 'circle'
      radius: number
    }
  | {
      kind: 'square'
      x: number
    }
// 如果这里使用 Omit 的话会报错,因为无法处理复杂类型
type ExcludedShapes = Exclude<Shapes, { kind: 'circle' }>
```

---

## Extract

类型提取，与`Exclude`正好相反，通常用于复杂类型。它接受两个类型参数，从`参数1`中提取出`参数2`的相同部分(`说人话就是过滤出两个参数之间相同的属性`)，并返回新类型

```ts
// 源码
type Extract<T, U> = T extends U ? T : never

// 例子
type A = string | object | boolean
type B = number | boolean
type C = number
type D = Extract<A, B> // boolean
type E = Extract<A, C> // never

type Shapes =
  | {
      kind: 'circle'
      radius: number
    }
  | {
      kind: 'square'
      x: number
    }
// 如果这里使用 Pick 的话会报错,因为无法处理复杂类型
type ExtractedShapes = Extract<Shapes, { kind: 'circle' }>
```

---

## Parameters

以元组的方式获得函数的入参类型

```ts
// 源码
type Parameters<T extends (...args: any) => any> = T extends (
  ...args: infer P
) => any
  ? P
  : never

// 例子
function handle(name: string) {}
type HandleType = Parameters<typeof handle> // [string]
type ArrowFuncType = Parameters<(count: number) => any> // [number]
```

---

## ReturnType

获得函数返回值的类型

```ts
// 源码
type ReturnType<T extends (...args: any) => any> = T extends (
  ...args: any
) => infer R
  ? R
  : any

// 例子
const func = () => 'jandan'
const async_func = async () => 'jandan'

// 得到函数的类型
type FuncType = typeof func // () => string
// 得到函数的返回值类型
type FuncReturn = ReturnType<FuncType> // string
type FuncReturn = ReturnType<typeof func> // string
// 如果是异步函数的话
type PromiseReturn = ReturnType<typeof async_func> // Promise<string>
type AsyncReturn = Awaited<ReturnType<typeof async_func>> // string
```

---

## 函数重载

在 JavaScript 中，根据传入不同的参数调用同一个函数，返回不同类型的值是常见的情况。TypeScript 通过为同一个函数提供多个函数类型定义来实现这个功能

```ts
// 定义了两个重载,在函数实现部分同时处理两种情况
function reverse(x: number): number
function reverse(x: string): string
function reverse(x: number | string): number | string {
  if (typeof x === 'number') {
    return Number(x.toString().split('').reverse().join(''))
  }
  return x.split('').reverse().join('')
}

reverse(12345) // 54321
reverse('hello') // olleh
```

---

## 从现有对象中获取类型

```ts
const obj = {
  name: 'jandan',
  age: 30,
}
// 得到对象的类型
type Person = typeof obj
// 得到对象中有哪些属性
type Props = keyof Person
type Props = keyof typeof obj
// 根据传入对象,提示其有哪些属性
function handle<T extends object, K extends keyof T>(obj: T, prop: K) {}
```

## 疑难杂症

#### 社区里交叉(嵌套)类型查看不方便的解决方案

```ts
interface MainType {
  name: string
  age: number
}
type NestedType = MainType & {
  isDeveloper: boolean
}
// 上面的嵌套方式在查看属性时很不方便,需要一层一层的去查看

// 下面是社区的解决方案,可以直接得到嵌套后类型的所有属性
type Prettify<T> = {
  [P in keyof T]: T[P]
} & {}
type PrettifyNestedType = Prettify<NestedType>
```
