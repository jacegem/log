---
title: drift/table
tags:
categories:
date: 2025-02-05
lastMod: 2025-02-05
---


```dart
class Record extends Table with TableBase {
  IntColumn get activityId => integer().nullable()();
  IntColumn get durationSecond => integer().nullable()();
  DateTimeColumn get startedAt => dateTime().nullable()();
  DateTimeColumn get endedAt => dateTime().nullable()();
  BoolColumn get isDeleted => boolean().withDefault(Constant(false))();
}
```
