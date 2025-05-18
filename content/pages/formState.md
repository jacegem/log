---
categories: react, hook, form,
title: formState
tags:
date: 2025-05-02
lastMod: 2025-05-02
---




## formState 객체의 주요 멤버

  + [[isDirty]]: 사용자가 어떤 요소를 변경했는지

  + [[dirtyFields]]: 사용자가 변경한 필드 정보 ('필드명:값' 형식)

  + [[touchedFields]]: 사용자가 조작한 필드 정보 ('필드명: true/false' 형식)

  + [[defaultValues]]: useForm 함수에서 설정된 기본값

  + [[isSubmitted]]: 폼이 제출되었는지 확인

  + [[isSubmitSuccessful]]: 폼이 성공적으로 제출되었는지

  + [[isSubmitting]]: 폼이 제출 중인지

  + [[isLoading]]: 폼이 로딩 중인지 (비동기 기본값을 로드하고 있는지)

  + [[submitCount]]: 폼 제출 횟수

  + [[isValid]]: 폼에 입력한 값이 올바른지

  + [[isValidating]]: 폼이 검증 중인지

  + [[errors]]: 검증 시 발생한 오류 정보






