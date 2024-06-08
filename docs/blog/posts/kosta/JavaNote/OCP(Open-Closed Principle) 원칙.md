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


### 🍀 결제시스템 예제 1

```java
public class SendMain {
     public static void main(String[] args) {
        Sender[] senders = {new EmailSender(), new SmsSender(), new FaceBookSender()};
        for (Sender sender : senders) {
            sender.sendMessage("환영합니다!"); 
            }
    } 
}
```


### 🍀 결제시스템 예제 2

- 조건
    - 새로운 결제수단을 추가했을 때 Pay를 사용하는 클라이언트 코드인 Payservice의 변경을 최소화하자



> PayMain1

1. PayService 객체 생성 
2. 3가지 결제 옵션 금액 결정
3. processPay() 메소드 호출


   
```java

public class PayMain1 {
    public static void main(String[] args) {
        PayService payService = new PayService();
        //kakao 결제
        String payOption1 = "kakao";
        int amount1 = 5000;
        payService.processPay(payOption1, amount1);
        //naver 결제
        String payOption2 = "naver";
        int amount2 = 10000;
        payService.processPay(payOption2, amount2);
        //잘못된 결제 수단 선택
        String payOption3 = "bad";
        int amount3 = 15000;
        payService.processPay(payOption3, amount3);
    }
}

```

> PayService

1. PayStore.findPay(option)을 호출하여 적절한 Pay 객체를 가져오기
2. 가져온 Pay 객체의 pay(int amount) 메소드를 호출하여 결제를 시도하고 결과를 반환 받습니다.
3. KakaoPay 클래스 결제 결과에 따라 성공 또는 실패 메시지를 출력

```java

public class PayService {

    // 변하지 않는 부분
    public void processPay(String option, int amount) {

        System.out.println("결제를 시작합니다: option=" + option + ", amount=" + amount);
        Pay pay = PayStore.findPay(option);
        boolean result = pay.pay(amount);

        if (result) {
            System.out.println("결제가 성공했습니다.");
        } else {
            System.out.println("결제가 실패했습니다.");
        } }

}

```


> PayService

1. 결제 옵션 문자열을 분석하여 해당하는 Pay 객체를 생성 및 반환
    -  kakao -> KakaoPay


```java

public abstract class PayStore {

    // 변하는 부분
    public static Pay findPay(String option) {
        if (option.equals("kakao")) {
            return new KakaoPay();
        } else if (option.equals("Naver")) {
            return new KakaoPay();
        } else {
            return new DefaultPay();
        }
    }
}

```

> Pay 인터페이스

```java
public interface Pay {
    boolean pay(int amount);
}

```

> NaverPay, DefaultPay, KakaoPay 클래스

1. 주어진 금액으로 결제 시도
2. 결제 성공 여부를 boolean으로 반환

```java

public class NaverPay implements Pay {
    @Override
    public boolean pay(int amount) {
        System.out.println("네이버페이 시스템과 연결합니다.");
        System.out.println( amount + "원 결제를 시도합니다.");
        return true;
    }
}

public class DefaultPay implements Pay {
    @Override
    public boolean pay(int amount) {
        System.out.println("결제 수단이 없습니다.");
        return false;
    }
}

public class KakaoPay implements Pay {
    @Override
    public boolean pay(int amount) {
        System.out.println("카카오페이 시스템과 연결합니다.");
        System.out.println( amount + "원 결제를 시도합니다.");
        return true;
    }
}
```


```bash

결제를 시작합니다: option=kakao, amount=5000 카카오페이 시스템과 연결합니다.
5000원 결제를 시도합니다.
결제가 성공했습니다.
결제를 시작합니다: option=naver, amount=10000 네이버페이 시스템과 연결합니다.
10000원 결제를 시도합니다.
결제가 성공했습니다.
결제를 시작합니다: option=bad, amount=15000 결제 수단이 없습니다.
결제가 실패했습니다.

```


### 🍀 결제시스템 예제 3 - 사용자 입력 받기

```bash

결제 수단을 입력하세요:kakao
결제 금액을 입력하세요:5000
결제를 시작합니다: option=kakao, amount=5000
카카오페이 시스템과 연결합니다.
5000원 결제를 시도합니다.
결제가 성공했습니다.
결제 수단을 입력하세요:exit
프로그램을 종료합니다.

```

```java

import java.util.Scanner;

public class PayMain2 {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        PayService payService = new PayService();

        while (true) {
            System.out.print("결제 수단을 입력하세요:"); 
            String payOption = scanner.nextLine(); 
            if (payOption.equals("exit")) {
                System.out.println("프로그램을 종료합니다.");
                return; 
            }
            System.out.print("결제 금액을 입력하세요:"); 
            int amount = scanner.nextInt(); 
            scanner.nextLine();
            
            payService.processPay(payOption, amount);
        }
    } 
}

```