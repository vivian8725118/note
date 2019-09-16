# RxJava：
## 线程切换原理
首先创建一个Observable给下一层，最后调用subscribe()的时候，回溯建立联系
第一层，是我们使用RxJava时进行的一系列链式调用，每次调用实际上都创建了一个新的Observable返回给下层，这一层的目的，就是创建一个个Observable，终点是subscribe()的调用，由此开始进入第二层。第一层按顺序执行完毕时，这些新建的Observable之间实际上还并没有建立联系。
第二层，是逆向顺序的回调层，以我们手动调用的subscribe()开始，逐级向上游回溯。每一级的subscribe()都会调用上级重写的call()，而中间每一级的call()又会调用subscribe()，直到调用到我们创建的原始Observable中重写的call()，开始执行我们的源业务代码，进入第三层。这一层的目的，是逐级建立订阅关系，我们的订阅业务代码被包含在原始Observer中，由我们手动调用subscribe()（黄色subscribe()）开始，向上逐级传递。这一层执行完毕以后，第一层创建的每个Observable才真正形成了一条链。
第三层，是正向顺序的回调层，从我们的源业务代码所在的call()开始，顺序执行，并由onNext()将结果传递至下一级。可以看到，我们在map()中写入的告诉RxJava具体如何进行转换的代码，同其他业务代码一样，也是在这一层执行的。

## 背压：
Flowable实现背压策略，MISSING，BUFFER，ERROR，DROP，LATEST

