### Hook

- `Hook 是一些可以让你在函数组件里“钩入” React state 及生命周期等特性的函数`。
- `Hook 就是 JavaScript 函数`

##### 使用规则

- 只能在**函数最外层**调用 Hook。不要在循环、条件判断或者子函数中调用
- 只能在 **React 的函数组件**中调用 Hook。不要在其他 JavaScript 函数中调用。
- 调用一个新的 effect 之前对前一个 effect 进行清理

