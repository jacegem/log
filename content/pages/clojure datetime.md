---
tags:
- clojure
- datetime
- instant
- epoch
- zoned
- date
- time
- local
categories:
- clojure
- datetime
date: 2024-12-02
title: clojure datetime
lastMod: 2025-01-03
---








## ns

```clojure
(ns user	
  (:require
   [clojure.walk :as walk]
   [java-time :as jt])
  (:import
   [java.time ZoneId]))
```



## instant

```clojure
(jt/instant)
;=> #object[java.time.Instant 0x43bf5e21 "2024-12-02T11:39:05.568272Z"]
```

## epoch

```clojure
(-> (jt/instant)
    (jt/to-millis-from-epoch))
;=> 1733139595866
```

## ZonedDateTime

```clojure
  (-> (jt/instant)
      (jt/zoned-date-time (ZoneId/of "Asia/Seoul")))
;=> #object[java.time.ZonedDateTime 0x3d29ff04 "2024-12-02T20:40:30.689936+09:00[Asia/Seoul]"]
```

## LocalDateTime

```clojure
(-> (jt/instant)
    (jt/zoned-date-time (ZoneId/of "Asia/Seoul"))
    (jt/local-date-time))
;=> #object[java.time.LocalDateTime 0x64f89f56 "2024-12-02T20:42:53.123852"]
```



## local ➡️ zoned

```clojure
(-> (jt/local-date-time)
    (jt/zoned-date-time (ZoneId/of "Asia/Seoul")))
;=> #object[java.time.ZonedDateTime 0x3e36cfa "2024-12-02T11:42:16.804282+09:00[Asia/Seoul]"]
```



## instant ➡️ epoch

```clojure
(-> (jt/instant)
    (jt/to-millis-from-epoch))
;=> 1733140306257
```



## zoned ➡️ epoch

```clojure
(-> (jt/zoned-date-time 1970)
    (jt/to-millis-from-epoch))
;=> 0
```



## epoch ➡️ zoned

```clojure
(-> (jt/to-millis-from-epoch 2)
    (jt/zoned-date-time))
;=> #object[java.time.ZonedDateTime 0x91f93db "0002-01-01T00:00Z[UTC]"]
```





## 변경

```clojure
(defn local-date-time
  [data]
  (walk/postwalk (fn [m]
                   (if (= (class m) java.time.ZonedDateTime)
                     (jt/local-date-time m)
                     m))
                 data))

(defn zoned-date-time
  [data]
  (walk/postwalk (fn [m]
                   (if (= (class m) java.time.LocalDateTime)
                     (jt/zoned-date-time m (ZoneId/of "Asia/Seoul"))
                     m))
                 data))

```




