---
tags:
- flutter
- drift
- select
categories:
- flutter
- drift
title: drift/select
date: 2025-02-06
lastMod: 2025-02-07
---


## get

```dart
Future<List<ActivityData>> getAll() async {
  List<ActivityData> res = await db.select(db.activity).get();
  return res;
}
```



## get with order

```dart
Future<List<ActivityData>> getAll() async {
  var query = db.select(db.activity)
    ..orderBy([
      (tbl) => OrderingTerm(
            expression: tbl.order,
            mode: OrderingMode.asc,
          ),
      (tbl) => OrderingTerm(
            expression: tbl.updatedAt,
            mode: OrderingMode.desc,
          )
    ]);

  var res = await query.get();
  return res;
}
```



## get with where

```dart
final query = db.select(db.record)
  ..where(
    (table) => table.startedAt.isBiggerOrEqualValue(startDt) & table.startedAt.isSmallerThanValue(endDt),
  )
  ..orderBy([
    (table) => OrderingTerm(expression: table.startedAt, mode: OrderingMode.asc),
  ]);

final result = await query.get();
```


