---
tags:
- flutter
- record
- page
categories:
- flutter
- page
title: page/record_list_page.dart
date: 2025-01-24
lastMod: 2025-01-24
---




```dart
class _RecordListPageState extends ConsumerState<RecordListPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Record / 활동 기록'),
      ),
      body: ListView(
        reverse: true,
        children: [
          Gap(100),
          RecordByDaySection(),
          Text('Record Page'),
        ],
      ),
    );
  }
}
```
