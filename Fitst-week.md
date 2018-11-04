##第一周周报:
###1. node assert 初步学习:
 - assert 模块提供了断言测试的函数，用于测试不变式, 其有 strict 与 legacy 两种模式, 一般使用strict模式.


- 常用api:
  - assert.AssertionError: Error 的一个子类，表明断言的失败。 使用new 创建实例: new assert.AssertionError(options)
  - assert.deepEqual() 等同于 assert.deepStrictEqua() 测试 两个参数是否深度相等。 深度相等意味着子对象中可枚举的自身属性也会按以下规则递归地比较。
  - assert.equal(actual, expected[, message]) : assert.strictEqual()的别名;
  - assert.notStrictEqual(actual, expected[, message]): 使用 SameValue比较法测试 actual 参数与 expected 参数是否不全等。
  - assert.notEqual(actual, expected[, message]) : assert.notStrictEqual 的别名;
  - assert.notDeepStrictEqual(actual, expected[, message]) 测试两个参数是否不深度全等。 与 assert.deepStrictEqual() 相反。
  - assert.ok(value[, message]): 测试 value 是否为真值。 相当于 assert.equal(!!value, true, message)。
  - assert.throws(fn[, error][, message]) : 抛出错误;
  - assert.fail([message]): 抛出 AssertionError，并带上提供的错误信息或默认的错误信息。


###2. mocha

- mocha是JavaScript的一种单元测试框架，既可以在浏览器环境下运行，也可以在Node.js环境下运行。使用mocha，我们就只需要专注于编写单元测试本身，然后，让mocha去自动运行所有的测试，并给出测试结果。
- mocha的特点主要有：
  - 既可以测试简单的JavaScript函数，又可以测试异步代码，因为异步是JavaScript的特性之一；
  - 可以自动运行所有测试，也可以只运行特定的测试；
  - 可以支持before、after、beforeEach和afterEach来编写初始化代码。
- mocha支持的断言模块:
- mocha支持任何可以抛出一个错误的断言模块。例如：should.js、better-assert、expect.js、unexpected、chai等。这些断言库各有各的特点，大家可以了解一下它们的特点，根据使用场景来选择断言库。
- 同步代码测试: 在测试同步代码的时候，用例函数执行完毕后，mocha就直接开始执行下一个用例函数了。
- 异步代码测试: 只需要在用例函数里边加一个done回调，异步代码执行完毕后调用一下done，就可以通知mocha;
- promise代码测试: 如果异步模块并不是使用callback，而是使用promise来返回结果的时候，可以让用例函数返回一个promise对象来进行正确性判断.
- 不建议使用箭头函数: mocha会把用例函数注册到自身的某个属性中，通过属性调用的使用，正常函数可以访问到mocha的其他属性，但是箭头函数不行.
- 钩子函数: mocha提供4种钩子函数：before()、after()、beforeEach()、afterEach()，这些钩子函数可以用来在用例集/用例函数开始执行之前/结束执行之后，进行一些环境准备或者环境清理的工.
- 钩子函数的描述参数: 定义钩子函数的时候，可以传第一个可选参数作为钩子函数的描述，可以帮助定位用例中的错误信息。若没有穿第一个参数，使用一个非匿名函数作为钩子函数，那么函数名称就将被作为钩子函数的描述。.
- 异步的钩子函数 : 钩子函数不仅能是同步函数，也可能是异步的函数，就像前边的异步测试用例函数一样。如果我们在开始之前，需要做一些异步的操作.
- 全局钩子:如果在用例集函数之外定义钩子函数，那么这个钩子函数将会对所有的mocha单元测试用例生效。若编写了多个用例集js文件，无论在哪一个用例集文件中，用例集函数之外定义钩子函数，都会对所有用例函数生效.

###3. should.js
- should.js是一个比较小而精的测试框架，他能够满足在开发过程中所需要的大部分测试场景，同时也支持自己编写扩展来强化它的功能。在设计上，这个框架使用了不少巧妙的方法，避免了一些复杂的链式调用与参数传递等问题，而且结构清晰
- asssertion.js为should.js中的类，负责对测试信息进行记录
- assertion-error.js为should.js定义了一个错误类，负责存储错误信息
- config.js中存储了一些should.js中的一些配置信息
- util.js中则定义了一些项目中常用的工具函数

```
心得体会: (多多琢磨)
- should.js中，通过将一些语义词添加为属性值并返回Assertion对象本身，因此有效解决了链式调用的问题。
- 通过Asseriton对象的属性来进行参数的传递，而不是通过函数参数，从而有效避免了函数调用时参数的传递问题以及多层调用时结构的复杂。
- should.js通过扩展的方式来添加其判断的函数，保证了良好的扩展性，避免了代码耦合在一起，通过也为其他人编写更多的扩展代码提供了接口。
- should.js通过extend方法，让should(obj)与obj.should两种方式达到了相同的效果。通过在defineProperty中定义should属性并且在回调函数中用should(obj)的方式来获取obj对象。
- 通过抛出错误而不是返回布尔值的方式来通知用户，能够更加明显的通知用户，也方便向上抛出异常进行传递。

```

###4. TDD 与 BDD
- TDD： Test-driven development （测试驱动开发）是一种使用自动化单元测试来推动软件设计并强制依赖关系解耦的技术。
  - 使用这种做法的结果是一套全面的单元测试，可随时运行，以提供软件可以正常工作的反馈。
  - TDD重点是培养整个研发过程的节奏感，就像跳踢踏舞一样，"ti-ta-ti"。在编写真正实现功能的代码之前先编写测试，每次测试之后，重构完成，然后再次执行相同或类似的测试。该过程根据需要重复多次，直到每个单元根据所需的规格运行。
- BDD:Behavior-Driven Development (行为驱动开发) BDD将TDD的一般技术和原理与领域驱动设计(DDD)的想法相结合。
  - BDD是一个设计活动，您可以根据预期行为逐步构建功能块。
  - BDD的重点是软件开发过程中使用的语言和交互。行为驱动的开发人员使用他们的母语与领域驱动设计的语言相结合来描述他们的代码的目的和好处。
  - 使用BDD的团队应该能够以用户故事的形式提供大量的"功能文档"，并增加可执行场景或示例。BDD通常有助于领域专家理解实现而不是暴露代码级别测试。它通常以GWT格式定义：GIVEN WHEN＆THEN。
  - BDD侧重于制定系统行为的场景。主要工作是通过协作和需求澄清，在项目干系人和交付团队之间建立共识。
###5 karma
- Karma的主要目标是为开发人员提供高效的测试环境。 这种环境不需要设置大量的配置，而是开发人员只需编写代码并从测试中立即获得结果.
- karma 支持三个命令。
````$xslt
start [<configFile>] [<options>] 启动 Karma 持续执行，也可以执行单次的测试，然后直接收集测试结果.
 init [<configFile>] 初始化配置文件.
 run [<options>] [ -- <clientArgs>] Trigger a test run.
````
 


