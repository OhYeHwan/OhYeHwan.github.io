---
title: "[Clean Code] 의미 있는 이름"
description:
date: 2022-02-09
update: 2022-02-09
tags:
  - Clean Code
series: "클린 코드"
---

## 의미 있는 이름

> 소프트웨어에서 이름은 어디나 쓰인다.

### 의도를 분명히 밝혀라

```

- 의도가 분명한 이름은 정말로 중요하다.

- 좋은 이름을 지으려면 시간이 걸리지만 좋은 이름으로 절약하는 시간이 훨씬 더많다.

- 이름을 주의깊게 살펴 더 나은 이름이 떠오르면 개선해라.

- 존재 이유, 수행 기능, 사용 방법에 대한 주석이 따로 필요없게 이름을 지어라.

```

```typescript
function getThem(): number[] {
  const list1 = []
  for (let x of theList) {
    if (x[0] == 4) {
      list1.push(x)
    }
  }
  return list1
}
```

코드가 하는 일을 짐작하기 어렵다.

```typescript
function getFlaggedCells(): Cell[] {
  const flageedCells: Cell[] = []
  for (let cell of gameBoard) {
    if (cell.isFlagged()) {
      flageedCells.push(cell)
    }
  }
  return flageedCells
}
```

코드의 단순성은 변하지 않았지만 좋은 이름을 통해 함수가 하는 일을 이해하기 쉬워졌다.

### 그릇된 정보를 피하라

```

```

### 의미 있게 구분하라

### 발음하기 쉬운 이름을 사용하라

### 검색하기 쉬운 이름을 사용하라

### 인코딩을 피하라

### 자신의 기억력을 자랑하지 마라

### 클래스 이름

### 메서드 이름

### 기발한 이름은 피하라

### 한 개념에 한 단어를 사용하라

### 해법 영역에서 가져온 이름을 사용하라

### 문제 영역에서 가져온 이름을 사용하라

### 의미 있는 맥락을 추가하라

### 불필요한 맥락을 없애라
