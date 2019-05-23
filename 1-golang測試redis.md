# 使用 Golang測試 Redis

此測試是在執行 docker-compose.yml後, 用來測試 Redis是否有正常運作中. 
> 備註: Redis官方推薦的 Golang client端套件 https://redis.io/clients#go

## Golang程式碼
下列範例使用的 Golang套件: https://github.com/go-redis/redis
```Golang
package db

import (
	"strconv"
	"testing"

	"github.com/go-redis/redis"
)

const host = "localhost"
const port = 6379

var addr = host + ":" + strconv.Itoa(port)

func Test_ping(t *testing.T) {
	client := redis.NewClient(&redis.Options{
		Addr:     addr,
		Password: "", // no password set
		DB:       0,  // use default DB
	})
	defer client.Close()

	pong, err := client.Ping().Result()
	if err != nil {
		t.Fatal("Ping, err:", err)
	}
	if pong != "PONG" {
		t.Fatal("!= PONG, err:", pong)
	}
	t.Log(pong)
}

func Test_set_get(t *testing.T) {
	client := redis.NewClient(&redis.Options{
		Addr:     addr,
		Password: "", // no password set
		DB:       0,  // use default DB
	})
	defer client.Close()

	err := client.Set("key", "value", 0).Err()
	if err != nil {
		t.Fatal(err)
	}

	val, err := client.Get("key").Result()
	if err != nil {
		t.Fatal(err)
	}
	t.Log("key", val)

	val2, err := client.Get("key2").Result()
	if err == redis.Nil {
		t.Log("key2 does not exist")
	} else if err != nil {
		t.Fatal(err)
	} else {
		t.Log("key2", val2)
	}
}
```