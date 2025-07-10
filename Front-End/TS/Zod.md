# Zod

## Zod

- 스키마 기반 유효성 검사 라이브러리

## 스키마

- 데이터의 구조와 규칙을 정의한것, 데이터의 형식,조건을 명확하게 기술하는 설계도

```js
import { z } from "zod";

const UserSchema = z.object({
  name: z.string(),
  age: z.number().min(0).max(120),
  email: z.string().email(),
});
```

> name이 문자열, age는 0~120 사이의 숫자, email은 이메일 형식이어야하는 규칙을 선언

## 특징

- 런타입 검증 라이브러리( TypeScript의 컴파일단 타입 검사를 보완 )
- 정의한 스키마로 부터 자동으로 타입을 추론( z.inter<typeof UserSchema> )

## 주요기능

- 스키마 유추(Type Interface) : zod로 작성된 스키마를 z.infer 타입연산 유틸을 사용해 타입으로 변환

```js
type User = z.infer<typeof UserSchema>;
```

- 커스텀 밸리데이션(.refine(), .superRefine(), .transform()...) : zod에서 제공하는 타입 검사 이외에 사용자 정의 로직을 통한 조건 검사 가능

```js
const PasswordSchema = z
  .string()
  .min(8)
  .refine((val) => /[0-9]/.test(val), {
    message: "비밀번호에는 숫자가 포함되어야 합니다",
  });
```

- 중첩 스키마 및 배열 : z.object()안에 또 다른 z.object()또는 스키마를 넣을 수 있음

```js
const PostSchema = z.object({
  title: z.string(),
  tags: z.array(z.string()),
  author: UserSchema,
});
```

- safeParse/parse : 스키마 검증시 실패유형을 고를 수 있음

```js
schema.parse(data); // 실패 시 예외를 던집니다.(런타임 오류 발생)
schema.safeParse(data); // 실패 시 success: false 형태로 반환해 핸들링이 쉽습니다.
```

- 재귀스키마 : 재귀적 형태의 스키마 구성 가능

```js
const Category: z.ZodType<any> = z.lazy(() =>
  z.object({
    name: z.string(),
    children: z.array(Category).optional(),
  })
);
```
