---
tags:
- logseq
- publish
date: 2024-08-03
title: logseq/publish
categories:
lastMod: 2025-01-03
---


# 로크시크 배포하기



## 그래프 내보내기

설정 버튼을 눌러서 > 그래프 내보내기 선택 > 공개 페이지 내보내기 선택

![](/assets/akhtapz.png)

내보내기 위치를 선택하면 다음 폴더들이 생성된다.

```shell
├── 404.html
├── assets
│   └── your_assets.png
├── index.html
└── static
    ├── css
    ├── fonts
    ├── icons
    ├── img
    └── js
```

#단점

페이지들이 모두 index.html 파일에 들어 있다.

문서의 수가 많아지면 데이터 전송량이 많아 문제가 될 수 있다.



## logseq-export 사용해서 배포하기

#단점

루트 링크는 연결이 되는데, 서브링크는 연결이 안된다.



## logseq schrodinger 플러그인으로 배포하기

#단점

페이지 제목을 변경하고 싶은데, 파일 경로를 따라간다. ➡️ 수정 방법을 찾아야 겠다.




