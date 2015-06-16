# 1.feature #
multicache4j用于为Java集成各种cache组件：
  * 方便集成各种remote cache
    * memcached  (支持组件spymemcached)
    * memcacheq  (支持组件spymemcached)
    * memcachedb (支持组件spymemcached)
    * ttserver   (支持组件spymemcached, ttserverclient)
    * redis      (支持组件jedis)
  * 方便集成各种local cache
    * ehcache
  * 基于对象池技术管理客户端连接对象，网络断开能够自动重连
  * 基于[Pattern Mapping](http://blog.csdn.net/liuzhongbing/archive/2011/01/18/6148866.aspx)进行哈希映射
  * 支持remote cache和local cache的混合缓存
  * 支持local cache的单独使用

# 2.usage #
  * multi cache (remote + local, or remote , or local)：混合使用远程与本地Cache
> 适用场景：单点应用+集群Cache
```
MultiCacheFactory.getInstance().set("foo", "bar");
MultiCacheFactory.getInstance().get("foo");
```
  * remote cache：单独使用远程Cache
> 适用场景：多点应用+集群Cache
```
RemoteCacheFactory.getInstance().set("foo", "bar");
RemoteCacheFactory.getInstance().get("foo");
RemoteCacheFactory.getInstance().del("foo");
```
  * local cache：单独使用本地Cache
> 适用场景：单点应用/多点应用
```
LocalCacheFactory.getInstance().set("foo", "bar");
LocalCacheFactory.getInstance().get("foo");
```

# 3.configuration #
配置文件multicache4j.xml如下：
```
<?xml version="1.0" encoding="UTF-8"?>
<root>
    <remotecache>
        <!-- memcached -->
        <datasource name="memd0" host="192.168.0.1" port="11210" timeout="200" maxActive="10" maxIdle="1" maxWait="-1" implementClass="com.multicache4j.remote.factory.SpyChannelFactoryImpl" />
        <datasource name="memd1" host="192.168.0.1" port="11211" timeout="200" maxActive="10" maxIdle="1" maxWait="-1" implementClass="com.multicache4j.remote.factory.SpyChannelFactoryImpl" />
        <datasource name="memd2" host="192.168.0.1" port="11212" timeout="200" maxActive="10" maxIdle="1" maxWait="-1" implementClass="com.multicache4j.remote.factory.SpyChannelFactoryImpl" />
        <datasource name="memd3" host="192.168.0.1" port="11213" timeout="200" maxActive="10" maxIdle="1" maxWait="-1" implementClass="com.multicache4j.remote.factory.SpyChannelFactoryImpl" />
        <datasource name="memd4" host="192.168.0.1" port="11214" timeout="200" maxActive="10" maxIdle="1" maxWait="-1" implementClass="com.multicache4j.remote.factory.SpyChannelFactoryImpl" />
        <datasource name="memd5" host="192.168.0.1" port="11215" timeout="200" maxActive="10" maxIdle="1" maxWait="-1" implementClass="com.multicache4j.remote.factory.SpyChannelFactoryImpl" />
        <!-- memcacheq -->
        <!-- memcachedb -->
        <!-- redis -->
        <datasource name="redis1" host="192.168.0.2" port="6379" timeout="200" maxActive="20" maxIdle="10" maxWait="1" implementClass="com.multicache4j.remote.factory.JedisChannelFactoryImpl" />
        <datasource name="redis2" host="192.168.0.2" port="6380" timeout="200" maxActive="20" maxIdle="10" maxWait="1" implementClass="com.multicache4j.remote.factory.JedisChannelFactoryImpl" />
        <datasource name="redis3" host="192.168.0.2" port="6381" timeout="200" maxActive="20" maxIdle="10" maxWait="1" implementClass="com.multicache4j.remote.factory.JedisChannelFactoryImpl" />
        <datasource name="redis4" host="192.168.0.2" port="6382" timeout="200" maxActive="20" maxIdle="10" maxWait="1" implementClass="com.multicache4j.remote.factory.JedisChannelFactoryImpl" />
        <datasource name="redis5" host="192.168.0.2" port="6383" timeout="200" maxActive="20" maxIdle="10" maxWait="1" implementClass="com.multicache4j.remote.factory.JedisChannelFactoryImpl" />
        <datasource name="redis6" host="192.168.0.2" port="6384" timeout="200" maxActive="20" maxIdle="10" maxWait="1" implementClass="com.multicache4j.remote.factory.JedisChannelFactoryImpl" />
        <!-- ttserver -->
        <datasource name="tt0" host="192.168.0.3" port="1978" timeout="200" maxActive="20" maxIdle="10" maxWait="1" implementClass="com.multicache4j.remote.factory.TTChannelFactoryImpl" />
        <datasource name="tt1" host="192.168.0.3" port="1978" timeout="200" maxActive="20" maxIdle="10" maxWait="1" implementClass="com.multicache4j.remote.factory.SpyChannelFactoryImpl" />
    </remotecache>
    
    <!-- mapping policy -->
    <key>
        <mapping pattern="default" remotecache="redis1" localcache=""/>
        <mapping pattern=".*[01234]{1}$" remotecache="memd0" localcache="" />
        <mapping pattern=".*[56789]{1}$" remotecache="memd1" localcache="" />
        <mapping pattern=".*[abcdABCD]{1}$" remotecache="redis1" localcache="foolist" />
        <mapping pattern=".*[efghEFGH]{1}$" remotecache="redis2" localcache="" />
        <mapping pattern=".*[ijklIJKL]{1}$" remotecache="redis3" localcache="" />
        <mapping pattern=".*[mnopMNOP]{1}$" remotecache="redis4" localcache="" />
        <mapping pattern=".*[qrstQRST]{1}$" remotecache="redis5" localcache="" />
        <mapping pattern=".*[uvwxUVWX]{1}$" remotecache="redis6" localcache="" />
        <mapping pattern=".*[yY]{1}$" remotecache="tt0" localcache="" />
        <mapping pattern=".*[zZ]{1}$" remotecache="tt1" localcache="" />
    </key>
</root>
```

本地缓存ehcache.xml配置：
```
<?xml version='1.0' encoding='GBK'?>
<ehcache>

    <diskStore path="/opt/cache/ehcache" />
    <defaultCache maxElementsInMemory="5000" eternal="true" timeToIdleSeconds="120" timeToLiveSeconds="120" 
        overflowToDisk="false" diskPersistent="false" diskExpiryThreadIntervalSeconds="120" memoryStoreEvictionPolicy="LRU" />

    <cache name="foolist" maxElementsInMemory="200" eternal="false" timeToIdleSeconds="600" timeToLiveSeconds="600" 
        overflowToDisk="false" diskPersistent="false" diskExpiryThreadIntervalSeconds="120" memoryStoreEvictionPolicy="LRU" />
</ehcache>
```

