# EventBus：观察者、反射、粘性广播
## subscribe()
* 通过subscriptionByEventType得到该事件类型的所有订阅者，根据优先级将当前订阅者插入到订阅者队列subscriptionByEventType中
* 在typesBySubscriber中得到当前订阅者订阅的所有事件队列，将此事件保存到队列typesBySubscriber中，用于后续取消订阅
## 原理：
发送事件的时候，根据事件类型找到所有的订阅者，然后遍历订阅者的订阅方法找到对应的方法
通过类型找到类的时候，要调用方法，是用到反射（只是根据类去调用订阅方法的时候用）
![Class c = Class. forName-className-;](http://ob9iws2up.bkt.clouddn.com/Class c = Class. forName(className);.png)


