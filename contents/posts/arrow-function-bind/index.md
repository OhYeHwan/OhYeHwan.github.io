---
title: "[JavaScript] 화살표 함수와 영역"
description:
date: 2022-01-25
update: 2022-01-25
tags:
  - JavaScript
  - this
  - arrow
series: "러닝 리액트"
---

`러닝 리액트(Learning React) 2판을 복습하면서 이해되지 않았던 부분에 대해서 정리 합니다.`

---

## 화살표 함수와 영역

책에서는 다음과 같은 코드를 제시하며 **화살표 함수와 영역**에 대해서 설명을 하고 있습니다.

```js
const gangwon = {
  resorts: ["용평", "평창", "강촌", "강릉", "홍천"],
  print: function (delay = 1000) {
    setTimeout(function () {
      console.log(this.resorts.join(",")) // this = Window
    }, delay)
  },
}

gangwon.print() // Cannot read property 'join' of undefined 라는 오류 발생
```

위 코드는 "gangwon"이라는 객체의 print 프로퍼티에 메서드를 정의하고 이를 호출하는 과정에서 오류가 발생하였습니다.
이는 setTimeout 메서드 내부에 정의된 함수의 this를 "gangwon"으로 생각하고 작성한 코드이기 때문에 발생한 오류입니다. 실제 this의 값은 window 객체이며 window 객체 내부에는 "resorts"라는 배열이 존재하지 않기 때문에 "join"메서드 호출시 오류가 발생합니다.

<br>

```js
const gangwon = {
  resorts: ["용평", "평창", "강촌", "강릉", "홍천"],
  print: function (delay = 1000) {
    setTimeout(() => { # 수정된 부분
      console.log(this.resorts.join(","))
    }, delay)
  },
}

gangwon.print() // 용평, 평창, 강촌, 강릉, 춘천
```

책에서는 이 문제를 해결하기 위해서 setTimeout 함수 안에 정의된 함수를 화살표 함수로 재정의하는 것을 해결방안으로 제시하고 있습니다. 이렇게 하면 this의 영역이 "gangwon"으로 설정이됩니다. 하지만 아래와 같이 코드를 바꾼다면 어떻게 될까요?

<br>

```js
const gangwon = {
  resorts: ["용평", "평창", "강촌", "강릉", "홍천"],
  print: (delay = 1000) => { # 수정된부분
    setTimeout(() => {
      console.log(this.resorts.join(","))
    }, delay)
  },
}

gangwon.print() // Cannot read property 'join' of undefined 라는 오류 발생
```

print 프로퍼티를 화살표 함수로 바꾸면 내부의 this가 gangwon에서 window로 다시 한번 변경 됩니다. 이러한 this의 변화 과정을 이해하기 위해서는 세가지의 배경지식이 있어야합니다.

<br>

- 첫번째, 함수를 어떤 객체의 메서드로 호출하면 this의 값은 그 객체를 사용한다.
  <br>
- 두번째, setTimeout 함수에서 this는 항상 전역 객체(window)를 this 바인딩한다.
  <br>
- 세번째, 일반 함수는 해당 객체를 바인딩하여 this의 값을 변경하고 화살표 함수는 상위 객체의 this를 이어받아서 사용한다.
  <br>

<br>

![예제코드1](/this-example1.png)

###### <center>예제 코드</center>

<br>

![예제결과1](/this-result1.png)

###### <center>예제 코드 결과 </center>

<br>

위 코드에서 print 프로퍼티로 정의된 함수의 this는 첫번째 배경지식에 의해서 "gangwon" 객체를 바인딩하게 됩니다. 결과를 보시면 gangwon의 resorts가 콘솔 로그에 출력되어 있는 것을 확인할 수 있습니다. 그리고 그 아래에는 setTimeout 내부에서 두번째 배경지식에 의해서 this가 출력됩니다. 결과를 보시면 window 객체가 출력된 것을 확인할 수 있습니다. 이 두가지 배경지식과 마지막 세번째 배경지식을 이용하여 위의 코드를 재해석 해본다면 다음과 같습니다.

<br>

```js
const gangwon = {
  resorts: ["용평", "평창", "강촌", "강릉", "홍천"],
  print: (delay = 1000) => { #2
    setTimeout(() => { #1
      console.log(this.resorts.join(","))
    }, delay)
  },
}

gangwon.print() // Cannot read property 'join' of undefined 라는 오류 발생
```

1. 화살표 함수로 인해서 setTimeout 내부의 this가 상위 객체의 this(gangwon)를 이어받아서 사용한다.
2. print 프로퍼티의 메서드 정의도 화살표 함수이기 때문에 상위 객체의 this(Window)를 이어받아서 사용한다.
3. 따라서 this = Window 이므로 Cannot read property 'join' of undefined 라는 오류 발생된다.
