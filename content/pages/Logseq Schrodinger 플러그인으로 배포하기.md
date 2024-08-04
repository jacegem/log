---
title: Logseq Schrodinger 플러그인으로 배포하기
tags:
categories:
date: 2024-08-03
lastMod: 2024-08-04
---


## 플러그인 설치

[public]({{< ref "/pages/public" >}})

UPPERCASE!

## 문서 작성

속성을 추가한다. `public:: true`



## 다운로드

export-public-pages-to-hugo 버튼을 눌러서 다운로드 받는다.

![](https://i.imgur.com/golowgw.png?align=center)



## 배포

publicExport.zip 파일 압축 해제 후 커밋 & 푸시를 한다.

```shell
ditto -V -x -k --sequesterRsrc --rsrc ~/Downloads/publicExport.zip $LOGSEQ_HUGO/content
git -C $LOGSEQ_HUGO add .
git -C $LOGSEQ_HUGO commit -m 'publicExport'
git -C $LOGSEQ_HUGO push
```



unzip 사용시 Illegal byte sequence 에러가 발생하여 ditto를 사용한다.





## 참고

hugo/disablePathToLower


