---
title: 现代 CSS 容器查询入门
date: 2026-07-05 09:15:00
tags: [CSS, 响应式]
categories: [前端基础]
---

做了多年响应式布局，我们一直围绕「视口（viewport）」写媒体查询。但真实场景里，组件的可用空间往往并不取决于视口，而取决于它在父容器里占了多宽。容器查询（Container Queries）补上了这块短板：它让样式能基于祖先容器的尺寸来响应，组件终于可以真正地「自适应」。

## @container 与 container-type

要使用容器查询，首先得声明一个「查询容器」。`container-type: inline-size` 把元素注册为容器，让子组件可以基于它的行内尺寸（通常是宽度）做响应。

```css
.card-wrap {
  container-type: inline-size;
  container-name: card;
}

@container card (min-width: 480px) {
  .card {
    display: grid;
    grid-template-columns: 120px 1fr;
    gap: 16px;
  }
}

@container card (max-width: 479px) {
  .card {
    display: flex;
    flex-direction: column;
  }
}
```

这样无论 `.card-wrap` 出现在侧边栏还是主内容区，只要宽度变化，`.card` 就会自动切换横向网格与纵向堆叠，完全不需要改 HTML。

## 简写与默认容器

如果只有一个容器层级，可以用 `container` 简写，并省略 `container-name`，直接用 `@container` 查询最近的祖先容器：

```css
.panel {
  container: panel / inline-size;
}

@container (min-width: 600px) {
  .panel__body {
    padding: 24px;
    font-size: 16px;
  }
}
```

## 容器查询单位

容器查询还带来一组新的单位：`cqw`、`cqh`、`cqi`、`cqb`、`cqmin`、`cqmax`，分别对应容器宽、高、行内尺寸、块尺寸及最小/最大值。它们让字号、间距随容器而非视口缩放：

```css
.hero__title {
  font-size: clamp(1.5rem, 5cqi, 3rem);
}
```

## 与媒体查询的分工

容器查询并不是要取代媒体查询。视口级的整体布局（导航栏切换、多栏栅格）仍适合用媒体查询；而组件内部的细节响应，交给容器查询更合适。两者协作，才能既照顾页面骨架，又照顾组件局部。

浏览器对容器查询的支持已经覆盖所有主流现代浏览器，是时候把它纳入日常的响应式工具箱了。
