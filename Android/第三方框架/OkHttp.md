# okhttp
## 异步请求：
创建AsyncCall，然后加入到队列。如果running队列没有超过最大限制，加入到running中执行。否则加入到等待队列。

maxhost 5
max并发 64

## 责任链：实现方式index+1
* RetryAndFollowUpInterceptor：负责连接失败后重试和重定向。
* BridgeInterceptor：负责把用户构造的请求转换为发送到服务器的请求、把服务器返回的响应转换为对用户友好的响应。（gzip压缩，存储cookie）
* CacheInterceptor：缓存拦截器，负责读取缓存和更新缓存。（只用到DiskLruCache）
* ConnectInterceptor：连接拦截器，负责从客户端到服务器的连接工作。（连接池，减少三次握手）
* CallServerInterceptor：负责向服务器发送请求数据，并从服务器读取响应数据。（发送请求，读取response，依赖okio）

## 缓存：DiskLruCache
缓存策略：
首先获取到缓存的response，然后判断networkRequest，cacheResponse
cacheReponse==null，不用缓存，把之前缓存清掉
networkRequest==null，说明不用网络，有缓存就返回缓存，没缓存就返回错误
networkrequsest！=null 才请求网络，并加入缓存

## 连接复用：
会有一个连接池来缓存连接，如果池中没有就创建新的连接，缓存到池中。如果有就复用。按host地址查找

## 连接回收：
每个连接有一个引用计数，如果没有引用为0，按空闲最久的连接先回收。每次put一个新连接的时候，也会出发回收。

## ArrayDeque：
只删除尾部，只添加头部，没有中间操作，所以速度比LinkedList快


