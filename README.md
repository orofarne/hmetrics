HMETRICS
========

Install
-------
```bash
go get 'github.com/orofarne/hmetrics'
```

API
---
https://gowalker.org/github.com/orofarne/hmetrics

Example
-------
```Go
package main

import (
        "github.com/orofarne/hmetrics"
        "time"
        "log"
        "expvar"
)

func test() {
        ms := hmetrics.NewMetricSet("test", 2*time.Second)

        timer := ms.NewTimer("time")
        τ := timer.NewObservation()
        defer τ.End()

        c := ms.NewCounter("count")
        c.Add(1)

        log.Print(expvar.Get("test"))
        time.Sleep(3*time.Second)
        log.Print(expvar.Get("test"))
}

func main() {
        test()
        time.Sleep(3*time.Second)
        log.Print(expvar.Get("test"))
}
```

```
% go run example.go
2014/04/15 12:01:16 {}
2014/04/15 12:01:19 {"count#rps":0.4999752489752871}
2014/04/15 12:01:22 {"count#rps":0,"time#rps":0,"time_avgtime#s":0}
```
