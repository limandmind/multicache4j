# multicache4j-0.1.0 #

2011-01-15 feature list:
  * 方便集成各种remote cache
    * memcached  (支持组件spymemcached)
    * memcachedb (支持组件spymemcached)
    * ttserver   (支持组件spymemcached, ttserverclient)
    * redis      (支持组件jedis)
  * 方便集成各种local cache
    * ehcache
  * 基于对象池技术管理客户端连接对象，网络断开能够自动重连
  * 基于正则表达式配置缓存映射位置
  * 支持remote cache和local cache的混合缓存
  * 支持local cache的单独使用

# multicache4j-0.1.0 #

2011-01-18 feature list:
  * 增加del()接口