# Glide
## 三级缓存：
内存缓存（2个）、磁盘缓存、网络
## 内存缓存机制:
* ActiveResourceCache：缓存当前正在使用的资源（弱引用）
* LRUResourceCache：缓存最近试用过但是当前未使用的资源，LRU算法
* BitmapPool：缓存所有被释放的图片，内存复用，LRU算法
* 区别：
    * LruResourceCache和ActiveResourceCache设计是为了尽可能的资源复用
    * BitmapPool的设计目的是为了尽可能的内存复用

## 请求流程：
with()通过RequestManagerRetriever.get()获取得到RequestManager，然后RequestManager.get（），根据不同的Context来处理。
load()把不同资源类型封装成一个RequestBuilder。
into先build一个target，在build一个request。由request发起请求，通过EngineJob和DecodeJob实现真正的请求。

