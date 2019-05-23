## 重點
- Redis(Remote DIctionary Server)是一種 key-value儲存系統.

- 數據結構有 5種( String、Hash、List、Set、Sorted Set )

- Redis線上測試網站 http://try.redis.io/

- Redis的操作都是原子性

- Redis支援 Publish/Subscribe

---

### 使用 redis-cli連線至 redis服務器
命令: `redis-cli -h <host> -p <port>`
### 參數:

    -h 服務器位置
    -p 服務器 Port
```shell
$ redis-cli -h 127.0.0.1 -p 6379
```

### 測試服務器是否運作中
命令: `PING`
```
127.0.0.1:6379> PING
PONG
```

### 切換資料庫
命令: `SELECT <index>`
```
127.0.0.1:6379> SELECT 1
OK
```
> 備註: 預設使用 0.

---

### 設定服務器密碼

命令: `CONFIG SET requirepass <password>`
```
127.0.0.1:6379> CONFIG SET requirepass "123456789"
OK
127.0.0.1:6379> CONFIG GET requirepass
1) "requirepass"
2) "123456789"
```
> 備註: 範例設定服務器密碼為 123456789. `CONFIG GET requirepass`命令可以用來確認當前設定的密碼.

連線至服務器
```shell
$ redis-cli -h 0.0.0.0 -p 6381 -a 123456789
```
### 參數:

    -a 服務器密碼

### 驗證密碼
命令: `AUTH`
```
0.0.0.0:6381> AUTH 123456789
OK
```
> 備註: 如果使用 redis-cli連線時沒有輸入密碼, 則需要使用此命令.

---

### 設定檔(redis.conf)取出內容
```
CONFIG GET *
```
> 備註: *是指全部設定, 可指定.

### 設定檔(redis.conf)修改內容
命令: `CONFIG SET CONFIG_SETTING_NAME NEW_CONFIG_VALUE`
```
CONFIG SET loglevel "notice"
```

---

### 刪除 KEY

命令: `DEL key`
```
127.0.0.1:6379> GET myKey
"abc"
127.0.0.1:6379> DEL myKey
(integer) 1
127.0.0.1:6379> GET myKey
(nil)
```
> 備註: 當 KEY不存在時如果執行 GET則會取得 (nil)

### 存入
命令: `SET key value`
```
127.0.0.1:6379> SET myKey abc
OK
```

### 取出
命令: `GET key`
```
127.0.0.1:6379> GET myKey
"abc"
```

### 檢查是否存在
命令: `EXISTS key [key ...]`
```
"OK"
127.0.0.1:6379> EXISTS key1
(integer) 1
127.0.0.1:6379> EXISTS nosuchkey
(integer) 0
127.0.0.1:6379> SET key2 "World"
"OK"
127.0.0.1:6379> EXISTS key1 key2 nosuchkey
(integer) 2
127.0.0.1:6379> 
```

---

## HASH類型

### 存入
命令: `HMSET key field value [field value ...]`
```
127.0.0.1:6379> HMSET myhash field1 "Hello" field2 "World"
"OK"
```
> 備註: 如果再次輸入 HMSET, 且 key已經存在, 則後來的 field如果存在則會覆蓋掉原本的 field的值. 如果 field不存在, 則會這組 key會多一個 field與他的值.

### 檢查總共有多少個 field
命令: `HLEN key`
```
redis> HSET myhash field1 "Hello"
(integer) 1
redis> HSET myhash field2 "World"
(integer) 1
redis> HLEN myhash
(integer) 2
```

### 檢查 field是否存在
命令: `HEXISTS key field`
```
127.0.0.1:6379> HSET myhash field1 "foo"
(integer) 1
127.0.0.1:6379> HEXISTS myhash field1
(integer) 1
127.0.0.1:6379> HEXISTS myhash field2
(integer) 0
```

### 取出
命令: `HMGET key`
```
127.0.0.1:6379> HMGET myhash field1 field2 nofield
1) "Hello"
2) "World"
3) (nil)
```

### 取出全部
命令: `HGETALL key`
```
127.0.0.1:6379> HGETALL myhash
1) "field1"
2) "Hello"
3) "field2"
4) "World
```

### 刪除
命令: `HDEL key field [field ...]`
```
127.0.0.1:6379> HSET myhash field1 "foo"
(integer) 1
127.0.0.1:6379> HDEL myhash field1
(integer) 1
127.0.0.1:6379> HDEL myhash field2
(integer) 0
127.0.0.1:6379> 
```

---

## 參考
- [Redis 教程](http://www.runoob.com/redis/redis-tutorial.html)
- [Redis](https://redis.io/commands)

