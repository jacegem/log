---
tags:
- logseq
- public
- plugin
- ditto
- sed
- git
categories:
- app
- logseq
title: Logseq Schrodinger 플러그인으로 배포하기
date: 2024-08-03
lastMod: 2024-08-04
---






## 플러그인 설치

https://github.com/sawhney17/logseq-schrodinger 설치

https://github.com/CharlesChiuGit/Logseq-Hugo-Template 템플릿 사용해서 저장소 생성



## 문서 작성

logseq에서 문서를 작성한다.

문서 속성에 `public:: true` 를 추가한다

필요에 따라 `tags::`, `categories::` 를 추가한다.

예시)

```typescript
public:: true
tags:: logseq, public, plugin, ditto, sed, git
categories:: app, logseq,
```



## 다운로드

`export-public-pages-to-hugo` 버튼을 눌러서 다운로드 받는다.

![export](/assets/golowgw.png#center)





## 배포

다운받은 `publicExport.zip` 압축을 풀면 아래 처럼 나온다.

```
.
├── assets
│   └── golowgw.png#center
└── pages
    ├── Logseq Schrodinger 플러그인으로 배포하기.md
    ├── public.md
    └── 카이젠 저니.md
```



저장소 경로를 설정하고 (`$LOGSEQ_HUGO`)

publicExport.zip 파일 압축 해제 후 커밋 & 푸시를 한다.

```shell
ditto -V -x -k --sequesterRsrc --rsrc ~/Downloads/publicExport.zip $LOGSEQ_HUGO/content
sed -ri '' 's/\(https:\/\/i.imgur.com/\(\/assets/g' $LOGSEQ_HUGO/content/pages/*.md
git -C $LOGSEQ_HUGO add .
git -C $LOGSEQ_HUGO commit -m 'publicExport'
git -C $LOGSEQ_HUGO push
```



> unzip 사용시 Illegal byte sequence 에러가 발생하여 ditto를 사용한다.

> 외부 이미지 링크 사용시 남아있는 https://i.imgur.com 을 제거한다.






