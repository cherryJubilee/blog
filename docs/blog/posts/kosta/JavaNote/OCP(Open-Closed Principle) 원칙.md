---
draft: false
authors:
    - hyewon
date: 2024-06-07
categories:
    - Java
---

# OCP(Open-Closed Principle) 원칙

> OCP(Open-Closed Principle) 원칙

<!-- more -->

## 📌 OCP(Open-Closed Principle) 원칙

1. `Open for extension` : 새로운 기능을 추가하거나 변경사항이 생겼을 때, 기존 코드는 확장할 수 있어야 한다.
2. `Closed for modification` : 기존의 코드는 수정되지 않아야 한다.  
   
즉 확장에는 열려있고, 변경에는 닫혀있다!! 기존 코드의 수정 없이 새로운 기능을 추가할 수 있다는 의미


### 🍀 결제시스템 예제