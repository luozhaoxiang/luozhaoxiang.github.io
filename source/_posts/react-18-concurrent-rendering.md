---
title: React 18 并发渲染机制解析
date: 2026-08-01 10:00:00
tags: [React, 前端]
categories: [前端框架]
---

React 18 引入的并发渲染（Concurrent Rendering）是近年来 React 架构上最大的一次升级。它并非一个新的 API，而是一套让渲染过程变得「可中断、可恢复」的底层能力。在并发模式下，React 可以暂停当前的渲染任务，去处理更高优先级的更新，然后再回来继续未完成的工作，从而保证主线程始终能响应用户交互。

## 并发渲染解决了什么

在旧的同步渲染模式下，一旦开始渲染一棵大型组件树，整个过程无法中断。如果组件层级很深、计算量大，就容易造成主线程长时间被占用，导致输入、滚动等交互出现卡顿。并发渲染把渲染拆分成多个小单元（fiber），让 React 可以在单元之间让出主线程。

## useTransition：标记非紧急更新

`useTransition` 允许把某个状态更新标记为「过渡」，使其以低优先级执行。典型场景是搜索框输入触发的大列表过滤。

```tsx
import { useState, useTransition } from 'react';

export function SearchBox({ items }: { items: string[] }) {
  const [query, setQuery] = useState('');
  const [result, setResult] = useState(items);
  const [isPending, startTransition] = useTransition();

  function handleChange(e: React.ChangeEvent<HTMLInputElement>) {
    const value = e.target.value;
    // 输入框更新是高优先级，立即响应
    setQuery(value);
    // 列表过滤是低优先级，可被中断
    startTransition(() => {
      setResult(items.filter((it) => it.includes(value)));
    });
  }

  return (
    <>
      <input value={query} onChange={handleChange} placeholder="搜索…" />
      {isPending && <span>过滤中…</span>}
      <ul>
        {result.map((it) => (
          <li key={it}>{it}</li>
        ))}
      </ul>
    </>
  );
}
```

这样即便列表有上万条数据，用户输入依然流畅，因为过滤任务不会阻塞输入框的更新。

## useDeferredValue：延迟消费一个值

`useDeferredValue` 与 `useTransition` 解决的是同一类问题，但视角不同：它接收一个值，返回它的「延迟版本」。当源值快速变化时，延迟值会沿用旧值，直到有空闲时间才追上新值。

```tsx
import { useDeferredValue, useMemo } from 'react';

function Chart({ data }: { data: number[] }) {
  const deferred = useDeferredValue(data);
  const heavy = useMemo(() => deferred.map((n) => n * 2), [deferred]);
  return <pre>{JSON.stringify(heavy)}</pre>;
}
```

并发渲染并不是「多线程」，它依旧运行在主线程上，只是通过让出与恢复的策略，让交互优先于计算。理解这一点，才能合理地把 `useTransition` 与 `useDeferredValue` 用在真正昂贵的地方，而不是到处套用。
