# Concept of the Cache
A cache is a memory buffer used to temporarily store frequently accessed data. It improves performance since data does not have to be retrieved again from the original source. Caching is actually a concept that has been applied in various areas of the computer/networking industry for quite some time, so there are different ways of implementing cache dependening upon the use case. In fact, devices such as routers, switched, and PCs use caching to speed up memory access. Another very common cache, used on almost all PCs, is the web browser cache for storing request objects so that same data doesn't need to be retrieved multiple times. In a distributed JEE application, the client/server side cache plays a significant role in improving application performance. The client-side cache is used to temporarily store the static data transmitted over the network from the server to avoid unnecessarily calling to the server. On the other hand, the server-side cache is used to store data in memory fetched from the other resources.

Cache can be designed on single/multiple JVM or clustered environment. Different scalability scenarios where caching can be used to meet the nonfunctional requirment are as follows.

## Vertical Scalability


Source: [https://dzone.com/articles/introducing-amp-assimilating-caching-quick-read-fo](https://dzone.com/articles/introducing-amp-assimilating-caching-quick-read-fo)