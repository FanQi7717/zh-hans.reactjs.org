---
id: react-dom-client
title: ReactDOMClient
layout: docs
category: Reference
permalink: docs/react-dom-client.html
---

`react-dom/client`包提供了客户端专属方法用于初始化客户端应用程序，你的大部分组件都不需要这个模块。

```js
import * as ReactDOM from 'react-dom/client';
```

如果你使用 npm 和 ES5，你可以用：

```js
var ReactDOM = require('react-dom/client');
```

## 概览 {#overview}

以下方法可以在客户端环境使用：

- [`createRoot()`](#createroot)
- [`hydrateRoot()`](#hydrateroot)

### 浏览器支持 {#browser-support}

React支持所有现代浏览器，尽管对于旧版本来说，可能需要引入[相关的 polyfills 依赖](/docs/javascript-environment-requirements.html)。

> 注意：
>
> 我们不支持那些不兼容ES5方法或是微任务的旧版浏览器，例如IE。但如果你的应用包含了polyfills，例如[es5-shim and es5-sham](https://github.com/es-shims/es5-shim)，你可能会发现你的应用仍然可以在这些浏览器中正常运行。但是如果你选择这种方法，你便需要孤军奋战了。

## 参考 {#reference}

### `createRoot()` {#createroot}

```javascript
createRoot(container[, options]);
```
为提供的容器创建一个React根节点并将其返回。通过`render`方法，根节点被用来在DOM中渲染React元素。

```javascript
const root = createRoot(container);
root.render(element);
```

`createRoot` 接收两个参数:
- `onRecoverableError`: React从错误中自动恢复时调用的回调（可选参数）。
- `identifierPrefix`: React使用`React.useId`生成的id的前缀（可选参数）。当在同一页面内使用多个根节点时，可有效避免冲突，必须与服务器上使用的前缀相同。

根节点可以被`unmount`方法卸载：

```javascript
root.unmount();
```

> 注意：
>
> `createRoot()` 控制你传入容器节点的内容。当调用`render`时， 内部任何现有的DOM元素都将被替换，后续的调用使用`diff`算法高效更新。
>
> `createRoot()` 不会更改容器节点（只更改容器内部子节点）。 可以在不覆盖现有子节点的情况下，将组件插入到当前DOM节点。
>
> 不支持使用`createRoot()`复用填充（译者注：初次渲染时，复用已存在的DOM节点以减少开销，常指服务端渲染场景）服务端渲染的容器，应该使用 [`hydrateRoot()`](#hydrateroot) 实现.

* * *

### `hydrateRoot()` {#hydrateroot}

```javascript
hydrateRoot(container, element[, options])
```

与[`createRoot()`](#createroot)相同，但被用来复用填充容器，HTML内容被[`ReactDOMServer`](/docs/react-dom-server.html)渲染。React将尝试把事件监听器附加到现有标记上。

`hydrateRoot` 接收两个参数:
- `onRecoverableError`: React从错误中自动恢复时调用的回调（可选参数）.
- `identifierPrefix`: React使用`React.useId`生成的id的前缀（可选参数）。当在同一页面内使用多个根节点时，可有效避免冲突，必须与服务器上使用的前缀相同。

> 注意：
> 
> React期望服务器与客户端呈现内容相同。React可以修补文本内容的差异，但你应该将这种不匹配视为bug并修复。开发模式下，React会对复用填充过程中的不匹配发出警告。在不匹配的情况下，无法保证属性差异会被修补。这对于性能来说很重要，因为这种不匹配在大多数应用程序中非常少见，因此要验证所有标记将是非常昂贵的开销。
