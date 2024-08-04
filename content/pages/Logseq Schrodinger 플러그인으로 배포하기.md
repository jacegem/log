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

![getBlocksInPage](getblocksinpage)

![caption](https://i.imgur.com/golowgw.png){:height 90, :width 500, :align center}

local image

![스크린샷 2024-08-03 오후 11.02.24.png](/assets/스크린샷_2024-08-03_오후_11.02.24_1722733021855_0.png)







## 배포

publicExport.zip 파일 압축 해제 후 커밋 & 푸시를 한다.

```shell
ditto -V -x -k --sequesterRsrc --rsrc ~/Downloads/publicExport.zip $LOGSEQ_HUGO/content
git -C $LOGSEQ_HUGO add .
git -C $LOGSEQ_HUGO commit -m 'publicExport'
git -C $LOGSEQ_HUGO push
sed -ri '' 's/\(https:\/\/i.imgur.com/\(\/assets/g' $LOGSEQ_HUGO/content/pages/*.md
sed -ri '' 's/\{\:.*\}//g' $LOGSEQ_HUGO/content/pages/*.md
```



unzip 사용시 Illegal byte sequence 에러가 발생하여 ditto를 사용한다.

외부 이미지 링크 사용시 남아있는 https://i.imgur.com 을 제거한다.

`{:width 100}`  속성을 제거한다.



## 참고

hugo/disablePathToLower


