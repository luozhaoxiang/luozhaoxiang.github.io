---
title: TypeScript 类型体操实战
date: 2026-07-20 14:30:00
tags: [TypeScript, 类型系统]
categories: [前端基础]
---

「类型体操」听起来像炫技，但它的本质是让类型系统替我们捕获更多运行时错误。掌握条件类型、映射类型与 `infer` 这三件套后，很多原本需要手写运行时校验的逻辑，都可以前移到编译期。

## 条件类型：类型层面的 if

条件类型的语法是 `T extends U ? X : Y`。它让类型能根据输入「分支」，是类型体操的基石。下面这个 `IsNever` 判断一个类型是否为 `never`：

```ts
type IsNever<T> = [T] extends [never] ? true : false;

type A = IsNever<never>; // true
type B = IsNever<string>; // false
```

注意这里用 `[T] extends [never]` 而不是 `T extends never`。直接写 `never extends never` 会被 distributive 规则拆解为空联合，结果永远是 `never`，包一层元组可以绕过分布行为。

## 映射类型：遍历与改写键

映射类型用 `in` 遍历联合类型，对每个键做改写。结合 `as` 子句还能重命名键。下面把一个对象类型的所有键变成可选、并加上 `readonly`：

```ts
type DeepReadonly<T> = {
  readonly [K in keyof T]: T[K] extends object ? DeepReadonly<T[K]> : T[K];
};

interface Config {
  host: string;
  options: { timeout: number; retry: boolean };
}

type ReadonlyConfig = DeepReadonly<Config>;
// { readonly host: string; readonly options: { readonly timeout: number; readonly retry: boolean } }
```

## infer：在条件里捕获类型

`infer` 让我们在条件类型的 extends 子句中「声明」一个待推断的类型变量。最经典的例子是提取函数返回值类型：

```ts
type ReturnTypeOf<T> = T extends (...args: never[]) => infer R ? R : never;

type R1 = ReturnTypeOf<() => string>; // string
type R2 = ReturnTypeOf<(x: number) => boolean>; // boolean
```

`infer` 也能解构字符串、数组、Promise 等。比如取出 `Promise<T>` 内部的 `T`：

```ts
type Awaited<T> = T extends Promise<infer U> ? U : T;

type Inner = Awaited<Promise<number>>; // number
```

## 综合练习：PartialByKeys

把指定一组键变为可选，其余保持不变——这是 `Partial` 与 `Pick`/`Omit` 的组合应用：

```ts
type PartialByKeys<T, K extends keyof T = keyof T> = Omit<T, K> & Partial<Pick<T, K>>;

interface User {
  id: number;
  name: string;
  email: string;
}

type UserPatch = PartialByKeys<User, 'name' | 'email'>;
// { id: number; name?: string; email?: string }
```

类型体操的目标不是写出最短的表达式，而是让调用方在使用时获得精确的提示与保护。当你发现一个类型推导反复出错时，往往就是该补一段体操的地方。
