# Immix

> 一个极小的劫持方案，通过抽象 patch 用于 immutable 环境

### 原理

在 Proxy 劫持过程中产生 patches，在 react 等 immutable 环境中集中处理 patches

#### immix vs immer

immer 的方案是写时拷贝，在劫持的过程中进行对象的拷贝，将副作用转移到备胎对象上

它和 state 高度脱离，拿到的 draft 对象不再是普通的对象，而是 Proxy 实例

加上拷贝的性能问题，导致它不得不做一些优化手段（freeze tree）

immix 多了一层 patches 的抽象，无需关心对象是否可变，无需生成备胎，我们只要拿到 pathes ，在 immutable 环境中集中处理 patch 即可

### Use

```js
var obj = { name: 132, age: 20 }
var proxy = new Immed(obj)
var state = proxy.getState()
state.name = 'yse'
```

以上，我们成功劫持并修改了对象的值，我们不关心原始对象有没有变化（当然肯定没有变化）

我们只需要拿到 pathes ：

```js
var patches = proxy.getPatches()
console.log(patches) // patches [{op: "replace", path: "/name", value: "yse"}]
```

当我们拿到 patches 后，我们就可以对 state 进行处理了

```js
let state = { name: 132, age: 20 }
let patces = [{ op: 'replace', path: '/name', value: 'yse' }]
pathches.forEach({op,path,value} => {
  switch (op) {
    case 'replace':
      delve(state, path, value)
      break
  }
})
```

如果我们能够事先给组件绑定 path 的话，甚至可以做 path 的匹配命中工作
```js
let path = '/name'
patches.filter((item=>item.path===path)).forEach(/*……*/)
```

