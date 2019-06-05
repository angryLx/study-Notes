# 关于各种面试题

## `About React`
[Q1. react中，为什么要在列表组件中写key，其作用是什么？](#A1.原因及作用)








## `Answer About These Question`
A1.原因及作用

key帮助react识别哪些元素改变了，更快更准确。因此需要key在元素所在列表中是独一无二的字符串。（注：若列表项目的`顺序可能发生改变，不建议使用索引作为key值，`会导致diff性能变差，可能引起组件状态问题）

[`延伸：`](#https://zh-hans.reactjs.org/docs/reconciliation.html)key应该具有稳定、可预测，以及列表内唯一的特质。不稳定的 key（比如通过 Math.random() 生成的）会导致许多组件实例和 DOM 节点被不必要地重新创建，这可能导致性能下降和子组件中的状态丢失。