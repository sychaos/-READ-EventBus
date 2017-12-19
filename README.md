## 手撕源码系列
    EventBus.getDefault常见单例
    EventBus.register 注册订阅者
    subscriber订阅者的类
    在 subscriberMethodFinder.findSubscriberMethods(subscriberClass)中遍历该类中的所有方法 利用eventBus回调方法名字的特征，封装成SubscriberMethod
    并循环调用 subscribe(subscriber, subscriberMethod);
    在 subscribe中会通过 CopyOnWriteArrayList<Subscription> subscriptions = subscriptionsByEventType.get(eventType) 获取 订阅eventType的订阅者list
    然后存至subscriptionsByEventType中
    typesBySubscriber是以订阅者类为key，以event事件类为value，在进行register或unregister操作的时候，会操作这个map  反正都是缓存用的！！！

    然后EventBus.post event维护一个postingState.eventQueue队列 一旦post EventBus会先把post锁住知道该post事件完全释放再打开
    然后会一直调用postSingleEvent 直到队列为空

    postSingleEvent中 根据设置不同 通过不同的方法获取clazz 然后在postSingleEventForEventType中循环调用 订阅的回调方法