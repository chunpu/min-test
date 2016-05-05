设计一个最简单的测试框架

```js
var test = require('min-test')

test('should ok', function(t) {
	// t means assert and other
	t.ok(true)
	t.equal(1, 2, 'not equal')
})
```

mocha 不好的地方

- mocha 难以控制
- mocha 使用全局变量 describe 等，导致很难配合 webpack 等打包工具，自定义程序很困难
- 而且 before after 学起来也需要成本
- 执行需要用 `mocha xx.js`

另外两个比较火的框架

- substack 的 [tape](https://github.com/substack/tape)
- tj 推荐的 [ava](https://github.com/sindresorhus/ava)

参考 mocha, tape, tap, ava, t-man

都使用了类似的模式

由于是 min-test，因此不用 `test.before` `test.after` 等高端用法，import 出来直接就是 test function

callback 中唯一参数 t，t 可以代表 assert 的 t，也可以是 test 的 t

- `t.ok`
- `t.equal` use `===`
- `t.deepEqual`
- `t.plan`
- `t.end`

> ISSUE：tape 需要 `t.end` 否则就认为没结束，但看起来好像是不必要的

### 异步测试(TODO)

利用 `t.plan` `t.end` timeout 来完成异步测试


### 执行过程

最弱版本

1. `test(title, func)`
	1. 如果 title 是 function，就返回 `test(title.name || 'untitled', title)`
	1. 如果 func 是 function
		1. queue push 进 `{title: title, func: func}`
1. next tick
	1. 遍历 queue
		1. try catch 执行一遍 func
