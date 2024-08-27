---
draft: false
authors:
    - hyewon
date: 2024-06-09
categories:
    - Java
    - inflearn
---

# 예외처리

> 체크 예외, 언체크 예외(런타임 예외)

<!-- more -->

## 💡 check 예외

-   Exception을 상속받은 예외는 체크 예외가 된다.
-   체크예외는 잡아서 직접 처리하거나 밖으로 던지거나 둘 중에 하나를 개발자가 직접 명시적으로 처리 해야됨

### throws vs throw

-   `throws 예외`는 발생시킨 예외를 메서드 밖으로 던질 때 사용하는 키워드이다.
-   `throw 예외`라고 하면 새로운 예외를 발생시킬 수 있다. 예외도 객체이기 때문에 객체를 먼저 new로 생성하고 그 예외를 발생시켜야한다.

## 💡 uncheck 예외

-   RuntimeException을 상속받은 예외는 언체크 예외가 된다.
