---
draft: false
authors:
  - hyewon
date: 2024-05-14
categories:
  - study
  - spring
---
# Spring 면접 준비
> spring 면접 리스트
<!-- more -->
### 스프링 프레임 워크
- 초기 서버사이드처리는 직접 쓰레드, 소켓연결 등 개발자들이 직접 처리했었다. 개발자마자 구현하는 방법이 달라서 협업이 불편했었다.

- 이런 개발 표준을 잡을 것이 Java Enterprise Edition( 예시 → Jsp/servlet)

- 근데 이 구현 방법 또한 여러가지 방법으로 나뉘에 되었고, 개발자들은 정형화, 표준화 된 방법을 찾았다. → framework
  
- 많은 종류의 프레임워크(EJB)가 나왔는데 그중에서도 경량화된 스프링이 많이 사용됨.

### 객체주입 방식 3가지

1. `@Autowired`
2. `setter 주입`
3. `생성자`


### 스프링  vs 스프링 부트 차이점

> 스프링 부트가 제공하는 편리

1. 내장서버
    
    스프링 부트 없을 때:  WAS 설치 → 개발한 웹   애플리케이션 코드를 WAR로 빌드 → 빌드한 WAR파일을 WAR 폴더 하위에 넣기 → WAS 실행      
    
    스프링부트는 톰캣을 라이브러리도 포함하는 내장톰캣 기능을 제공한다!
    
2. 편리한 의존성 & 권장 버전 관리
    
    스프링 부트 아닐 때:  어떤 라이브러리 사용할지 고르기, 버전도 고르기  
    
    스프링 dependency-management(라이브버전관리 플러그인) → 버전생략 가능  
    
    그리고 기본 라이브러리 묶음 제공(springboot-starter-web)   
    
3. 자동 구성(Configuration)
    - 일반적으로 자주 사용하는 수많은 빈들을 자동으로 등록해주는 기능
    
    - 스프링은 프로젝트 초기에 다양한 환경설정을 해야 하지만, 부트는 설정의 많은 부분을 자동화하여 사용자가 편하게 스프링을 활용할 수 있도록 한다.
    - spring boot starter dependency만 추가해주면 설정은 끝나고, 내장된 톰캣을 제공해 서버를 바로 실행할 수 있습니다.
  

### VO와 BO, DAO, DTO에 대해 설명해주세요.

  - `DAO(Data Access Object)` DB의 데이터에 접근을 위한 객체를 말합니다. (Repository 또는 Mapper에 해당)
  
  - `BO(Business Object)` 여러 DAO를 활용해 비즈니스 로직을 처리하는 객체를 말합니다. (Service에 해당)
  
  - `DTO(Data Transfer Object)` 각 계층간의 데이터 교환을 위한 객체를 말합니다. (여기서 말하는 계층은 Controller, View, Business Layer, Persistent Layer)
  
  - `VO (Value Object)` 실제 데이터만을 저장하는 객체를 말합니다.