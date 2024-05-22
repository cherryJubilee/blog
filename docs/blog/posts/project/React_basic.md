---
draft: false
authors:
    - hyewon
date: 2024-02-26
categories:
    - project
---

# Movie Plate

> 다양한 영화 리뷰를 모아보고 대여할 수 있는 플랫폼

<!-- more -->

## 기술

> 언어

-   TypeScript

> 프론트엔드

-   React
-   NextJS
-   Redux Toolkit

> 백엔드

-   NestJS
-   Prisma ORM
-   AWS RDS

## 프로젝트 구조

## 프로젝트 구축

```bash
sudo npm i -g @nestjs/cli
nest new ./
```

-   프로젝트 폴더 생성

```bash
nest g reaource
```

-   프리즈마 연결

```bash
npm i prisma -D
npx prisma init

npx prisma migrate dev
npx prisma studio
```
