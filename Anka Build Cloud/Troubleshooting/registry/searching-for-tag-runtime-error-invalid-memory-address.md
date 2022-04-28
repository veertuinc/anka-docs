---
title: "Searching for tag, runtime error: invalid memory address or nil pointer dereference"
linkTitle: "Searching for tag, runtime error: invalid memory address or nil pointer dereference"
weight: 1
---

## Scenario

Starting the registry is fine, but when you go to pull a template the following error is seen:

```
anka-registry      | I0425 16:22:44.191788       1 reg_logic.go:230] Searching for tag os-deps:uni-apps:app-deps:ensureit:6cores:8G-RAM
anka-registry      | 2022/04/25 16:22:44 http: panic serving 10.254.55.5:43736: runtime error: invalid memory address or nil pointer dereference
anka-registry      | goroutine 86299 [running]:
anka-registry      | net/http.(*conn).serve.func1()
anka-registry      |    /usr/local/go/src/net/http/server.go:1801 +0xb9
anka-registry      | panic({0x10d51c0, 0x1928040})
anka-registry      |    /usr/local/go/src/runtime/panic.go:1047 +0x266
anka-registry      | veertu.com/veertu/registry/backend/dao.(*Version).LastUpdated(...)
anka-registry      |    /anka/veertu/registry/backend/dao/vm.go:181
anka-registry      | veertu.com/veertu/registry/backend/dao.(*VM).LastUpdated(0x10cd7c0)
anka-registry      |    /anka/veertu/registry/backend/dao/vm.go:83 +0x45
anka-registry      | veertu.com/veertu/registry/backend/dao.(*VMCacheManager).SetList(0xc00032f4d0, {0xc0003ccc00, 0x13, 0x8}, {0x30, 0xc0000634e0, 0x19a6120})
anka-registry      |    /anka/veertu/registry/backend/dao/cache.go:30 +0x16e
anka-registry      | veertu.com/veertu/registry/backend/dao.(*VMBackend).List(0xc00032f4a0)
anka-registry      |    /anka/veertu/registry/backend/dao/vmbackend.go:33 +0x1f8
anka-registry      | veertu.com/veertu/registry/registry_logic.ListVms()
anka-registry      |    /anka/veertu/registry/registry_logic/reg_logic.go:23 +0x4e
anka-registry      | veertu.com/veertu/registry/http_interface.handleList({0x1331a00, 0xc000e6b340}, 0x11e7229)
anka-registry      |    /anka/veertu/registry/http_interface/http_hanlder.go:219 +0x145
anka-registry      | veertu.com/veertu/registry/http_interface.HandleGetRegistryVM({0x1331a00, 0xc000e6b340}, 0xc000c88600)
anka-registry      |    /anka/veertu/registry/http_interface/http_hanlder.go:237 +0xf1
anka-registry      | net/http.HandlerFunc.ServeHTTP(0x1155880, {0x1331a00, 0xc000e6b340}, 0xc)
anka-registry      |    /usr/local/go/src/net/http/server.go:2046 +0x2f
anka-registry      | veertu.com/veertu/registry/http_common.JsonHeader.func1({0x1331a00, 0xc000e6b340}, 0x0)
anka-registry      |    /anka/veertu/registry/http_common/utils.go:62 +0xf4
anka-registry      | net/http.HandlerFunc.ServeHTTP(0x108e520, {0x1331a00, 0xc000e6b340}, 0x1b)
anka-registry      |    /usr/local/go/src/net/http/server.go:2046 +0x2f
anka-registry      | veertu.com/veertu/registry/http_common.CorsHandler.func1({0x1331a00, 0xc000e6b340}, 0xc000ebe600)
anka-registry      |    /anka/veertu/registry/http_common/handlers.go:93 +0x11a
anka-registry      | net/http.HandlerFunc.ServeHTTP(0xc000c88500, {0x1331a00, 0xc000e6b340}, 0xc0000639f8)
anka-registry      |    /usr/local/go/src/net/http/server.go:2046 +0x2f
anka-registry      | github.com/gorilla/mux.(*Router).ServeHTTP(0xc000255e00, {0x1331a00, 0xc000e6b340}, 0xc000c88300)
anka-registry      |    /anka/vendor/github.com/gorilla/mux/mux.go:210 +0x1cf
anka-registry      | net/http.serverHandler.ServeHTTP({0xc000ebe480}, {0x1331a00, 0xc000e6b340}, 0xc000c88300)
anka-registry      |    /usr/local/go/src/net/http/server.go:2878 +0x43b
anka-registry      | net/http.(*conn).serve(0xc0003c8d20, {0x1338958, 0xc0003e0390})
anka-registry      |    /usr/local/go/src/net/http/server.go:1929 +0xb08
anka-registry      | created by net/http.(*Server).Serve
anka-registry      |    /usr/local/go/src/net/http/server.go:3033 +0x4e8
anka-registry      | 2022/04/25 16:22:44 http: panic serving 10.254.55.5:43738: runtime error: invalid memory address or nil pointer dereference
anka-registry      | goroutine 86436 [running]:
anka-registry      | net/http.(*conn).serve.func1()
anka-registry      |    /usr/local/go/src/net/http/server.go:1801 +0xb9
anka-registry      | panic({0x10d51c0, 0x1928040})
anka-registry      |    /usr/local/go/src/runtime/panic.go:1047 +0x266
anka-registry      | veertu.com/veertu/registry/backend/dao.(*Version).LastUpdated(...)
anka-registry      |    /anka/veertu/registry/backend/dao/vm.go:181
anka-registry      | veertu.com/veertu/registry/backend/dao.(*VM).LastUpdated(0x10cd7c0)
anka-registry      |    /anka/veertu/registry/backend/dao/vm.go:83 +0x45
anka-registry      | veertu.com/veertu/registry/backend/dao.(*VMCacheManager).SetList(0xc00032f4d0, {0xc000388000, 0x13, 0x10}, {0x0, 0x0, 0x19a6120})
anka-registry      |    /anka/veertu/registry/backend/dao/cache.go:30 +0x16e
anka-registry      | veertu.com/veertu/registry/backend/dao.(*VMBackend).List(0xc00032f4a0)
anka-registry      |    /anka/veertu/registry/backend/dao/vmbackend.go:33 +0x1f8
anka-registry      | veertu.com/veertu/registry/registry_logic.ListVms()
anka-registry      |    /anka/veertu/registry/registry_logic/reg_logic.go:23 +0x4e
anka-registry      | veertu.com/veertu/registry/http_interface.handleList({0x1331a00, 0xc0001bf6c0}, 0x11e7229)
anka-registry      |    /anka/veertu/registry/http_interface/http_hanlder.go:219 +0x145
anka-registry      | veertu.com/veertu/registry/http_interface.HandleGetRegistryVM({0x1331a00, 0xc0001bf6c0}, 0xc00025cf00)
anka-registry      |    /anka/veertu/registry/http_interface/http_hanlder.go:237 +0xf1
anka-registry      | net/http.HandlerFunc.ServeHTTP(0x1155880, {0x1331a00, 0xc0001bf6c0}, 0xc)
anka-registry      |    /usr/local/go/src/net/http/server.go:2046 +0x2f
anka-registry      | veertu.com/veertu/registry/http_common.JsonHeader.func1({0x1331a00, 0xc0001bf6c0}, 0x0)
anka-registry      |    /anka/veertu/registry/http_common/utils.go:62 +0xf4
anka-registry      | net/http.HandlerFunc.ServeHTTP(0x108e520, {0x1331a00, 0xc0001bf6c0}, 0x1b)
anka-registry      |    /usr/local/go/src/net/http/server.go:2046 +0x2f
anka-registry      | veertu.com/veertu/registry/http_common.CorsHandler.func1({0x1331a00, 0xc0001bf6c0}, 0xc000f47410)
anka-registry      |    /anka/veertu/registry/http_common/handlers.go:93 +0x11a
anka-registry      | net/http.HandlerFunc.ServeHTTP(0xc00025ce00, {0x1331a00, 0xc0001bf6c0}, 0xc00027d9f8)
anka-registry      |    /usr/local/go/src/net/http/server.go:2046 +0x2f
anka-registry      | github.com/gorilla/mux.(*Router).ServeHTTP(0xc000255e00, {0x1331a00, 0xc0001bf6c0}, 0xc00025cd00)
anka-registry      |    /anka/vendor/github.com/gorilla/mux/mux.go:210 +0x1cf
anka-registry      | net/http.serverHandler.ServeHTTP({0xc000f47290}, {0x1331a00, 0xc0001bf6c0}, 0xc00025cd00)
anka-registry      |    /usr/local/go/src/net/http/server.go:2878 +0x43b
anka-registry      | net/http.(*conn).serve(0xc0000b0500, {0x1338958, 0xc0003e0390})
anka-registry      |    /usr/local/go/src/net/http/server.go:1929 +0xb08
anka-registry      | created by net/http.(*Server).Serve
anka-registry      |    /usr/local/go/src/net/http/server.go:3033 +0x4e8
anka-registry      | 2022/04/25 16:22:44 http: panic serving
```

## Common Causes

1. Inside of the registry storage directory, you need to look for the vm_dir folder. Inside of that is a folder for each VM Template. The registry will not be able to parse a Template folder if it is empty or missing a versions file.

## Solutions

1. Delete the empty or broken directory.

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)