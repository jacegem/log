---
alias:
- react-hook-form
- rhf
link: https://react-hook-form.com
title: React hook Form
tags:
categories:
date: 2025-05-01
lastMod: 2025-05-01
---




폼 초기화

```javascript
const {register, handleSubmit, formState: {error}} = useForm({defaultValues})
```



useForm 함수의 주요 동작 옵션(인수 opts)

defaultValues

mode

onChange 입력 요소가 변경될 때마다 (성능 저하 가능성 있음)

onBlur 입력 요소에서 초점이 벗어났을 때

onSubmit 제출 시 (이후 onChange, 기본값)

onTouched 첫 번째 onBlur 타이밍 검증 (이후 onChange)

reValidateMode

criterialMode

shouldUseNativeValidation

shouldFocusError

delayError

resolver



useForm 함수의 반환값 (객체의 주요 멤버)

register 폼 요서에 연결해야 하는 이벤트 핸들러 등을 포함한 객체

formState 폼의 상태를 나타내는 객체

watch 지정된 요소 name의 값 가져오기 (변경사항 모니터링)

getValues 지정된 요소 name의 값을 가져옴

handleSubmit 제출 시 실행되는 핸들러를 설정

reset 폼 값을 지저된 값 '요소명:값,...'으로 재설정

resetField 지정된 요소 name을 재설정

setError 지정된 요소 name에 오류를 연결


