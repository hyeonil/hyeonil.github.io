---
layout: post
title: "SpringCamp 2017 2/2"
tags: [etc, spring]
description: >
---

2016년, 2017년 2년째 참석하고 있는 스프링 캠프, 첫째날과 둘째날 모두 등록을 했는데 첫째날은 회사 당직근무와 겹치는 바람에 참석을 하지 못했다. 토비님의 발표를 꼭 듣고 싶었는데 매우 아쉽다.(같이 셀카찍었으니 괜찮...)
개인적인 생각으로 작년에는 스칼라 주제가 많아서 큰 인상을 받지 못했는데, 올해는 작년에 비해 많은 인상을 받았다.
최근 많은 개발자들이 `Reactive Programming`에 관심을 가지고 있는데, 이번 `Spring Camp`에서 `Reactive Programming`에 대해 다양한 각도에서 발표를 해주셔서 많은 것을 배우게 됐다.

# Keynote - Naver 정상혁님

정상혁님은 KSUG 일꾼단을했고, 본인이 혹은 주위 사람들이 했던 코드기여에 대해서 보여주셨다.
정상혁님의 키노트의 핵심은 `오픈소스에 코드기여를 해보자!` 인것 같다.

나는 오픈소스 코드 기여에 관심이 많은데, 아직까지 많이 시도를 해보진 못했다.(`Javascript`에 조금밖에.. 이것도 코드수정은 아니라...) 주 언어인 `Java`에 코드기여를 할 그날까지 더 열심히...해야겠지

# Spring 5, Reactive Spring(Track A) - Pivotal 정윤진님

정윤진님은 주로 `Spring 5`와 `Reactive`의 개념에 대해서 발표를 해주셨던 것 같다.

## Spring 5

`Spring 5` 가장 큰 이슈는 `JDK 9` 지원과, `HTTP/2` 지원, `Reactive` 프로그래밍이다.
이에 따라 **Framework의 Baseline** 이 `Servlet 3.0`, `JDK 8+`로 업그레이드 된다고 한다.
또한, `Java 9`의 `Jigsaw Module System`을 지원할 예정이다.
5.0 정식 버전은 2017년 6월 출시할 예정이고, `Java 9`의 내용이 반영된 5.1은 2017년 12월에 출시될 예정이다.

`Jigsaw Module System`을 적용함으로써, 필요하지 않은 라이브러리까지 의존성이 종속되버리는 **의존성 종속 문제를 해결** 할 수 있다고 한다.

`Spring 5`에서는 `Tomcat`, `Jetty`, `Undertow` 등의 `Container`를 내장하는 것이 목표라고 한다.

## Project Reactor

몇년전부터 유행하고 있는 `Micro Service`를 적용하기 위해서는 어플리케이션이 `Reactive`해야한다. `Reactive`해야한다는 것은 `Responsive`하고, `Elastic`하며, `Resilient`를 가지고, `Message Driven`해야한다는 것이다. 이렇게 말해서는 무슨 말인지 모르니 아래에 추가적으로 기술한다.

|Attribute|Description|
|---|---|
|Responsive(응답성)|시스템은 가능한 즉각적으로 응답한다.|
|Elastic(유연성)|시스템이 장애에 직면하더라도 응답성을 유지해야 한다.|
|Resilient(탄력성)|시스템이 작업량이 변화하더라도 응답성을 유지해야 한다.|
|Message Driven(메세지 주도)| 비동기 메시지 전달에 의존하여 구성 요소 사이에서 느슨한 결합, 격리, 위치 투명성 을 보장하는 경계를 형성한다.|

자세한 내용은 [리액티브 선언문][reactive]을 참고하길 바란다.


### Reactive Streams Spec

이부분은.... 적어놓고 기억이 안난다 ㅠㅠ 이 몹쓸놈의 기억력... 알코올치매... 다듬고 싶은데 기억이 나질 않아서 다듬을 수 없어서 메모했던 내용만 그대로 남긴다 ㅠㅠ

* 인프라간의 상호 운영성에 집중(웹서버, 데이터 저장소 드라이버 및 웹 프레임워크)
  * Message Driven System
* 논-블러킹 백프레셔를 제공하는 비동기 스트림 프로세싱 표준
* 단순한 API 구조
  * Back pressure가 지원되는 Publisher + Subscriber(혹은 Subscription)
  * Java 9 의 java.util.concurrent.Flow 으로 다시 패키징 됨

### Reactive Streams Process

Mono방식과 Flux방식이 존재한다.

### Example

[Flux Example][fluxexample]

MongoDB가 필요하다. Docker를 통해 설정  

```bash
docker pull mongo
docker run -p 127.00.1:27017:27017 mongo
```


# Event Sourcing(Track A) - MS MVP 이규원님

이규원님은 이번 발표에서 스프링 캠프 최초를 많이 하신 것 같다. 스프링 캠프 최초 발표전 포토타임, 스프링 캠프 최초 구직광고... 뭔가 하나 더 있었는데... 참 유쾌하신 분인 것 같다.

[발표자료][session2]

## Event Sourcing

`Event Sourcing`은  메세지를 주고받는 것이 중심이 아니라 메세지를 저장하는 것이 핵심이다. 기존의 많은 어플리케이션은 어플리케이션이 가진 마지막 상태를 데이터베이스에 저장한다. 마지막 상태 외에 다른 정보들을 보고자 할때는 마지막 상태를 보관하는 것과 함께 관심부에 대해 로깅을 한다. 이러한 상태저장과 로깅은 대부분 `Transactional`하지 않아서 데이터 유실이 발생할 가능성이 있다. `Event Sourcing`은 저장소에 저장된 이벤트를 바탕으로 이를 다시 재생하여 상태를 만들어낸다. 만약 삭제가 필요 시 실제 삭제를 하는 것이 아닌 삭제 라는 이벤트를 저장하는 것이다.

`Event Sourcing`은 명령형 기반이라 명령을 검증해야 한다. 명령어는 보통 명령형 동사를 사용한다. 이벤트는 명령에 의해 실행시키는 것이기 때문에 검증 대상이 아니다. 이벤트는 실행되면 돌이킬 수 없다. 따라서 이벤트를 재생하는 것은 실패하지 않는다. 이벤트는 이미 발생한 일이기 때문에 과거형 동사를 사용한다.

1~n의 버전이 존재할 때 최종 상태는 1~n까지 모든 이벤트의 합이된다. 많은 이벤트 버전이 존재한다고 할 때, 1~n까지 이벤트의 합을 구하는 것은 많은 부하가 생길 수 있으므로 `Rolling Snapshot`을 사용하고, 이를 통해서 복원 속도를 올린다. `Rolling Snapshot`은 중간에 Key Value 형태로 상태를 저장하는 것이다.

## Messaging

> **2.** 메세지를 정확히 한번만 배달  
> **1.** 메세지 순서 보장  
> **2.** 메세지를 정확히 한번만 배달

메세지는 **최대 1번** 배달되어야 하고, **최소 1번** 배달되어야 한다. 이것은 메세지 유실이 발생할 수 있고, 문제 발생 시 메세지를 1번 이상 보낼 수 있다는 것을 의미한다.

사실상 분산 트랜잭션에서 정확히 한번만 배달하는 것이 어렵다. 분산 트랜잭션 구현 자체가 힘들기 때문이다.

이벤트 소싱은 도메인 모델에 대해서 이벤트 저장소에 저장 후 `Message Queue`에 저장한다.
저장소에 저장되어 있기 때문에 메세지 큐에 전달하면서 문제 발생 시 다시 보낼 수 있기 때문에 정확히 한번은 어렵더라도 최소 1번은 구현하기 쉽다.

순서 보장은 해당 이벤트내에서만 순서를 지키면 문제가 없다.
하나의 이벤트 스트림에 대해서는 동시성이 일어나지 않기 때문에 메세지 순서를 보장할 수 있다.


## CQRS

> 재고가 10개 미만인 상품 목록이 필요합니다.

만약 위와 같은 조회 조건이 발생했을 때, 이벤트 저장소를 풀스캔하면 절대 안된다.

이벤트소싱은 이론적으로는 `CQRS`에 종속되지 않지만 **현실적으로는 종속**된다.

CQRS: Command Query Responsibility Segregation,
**조회** 와 **변경** 명령을 분리한다.
시스템의 커맨드부와 쿼리부를 나눈다.


## 고려사항

`Event Sourcing`을 하기 위해 고려할 사항은 다음과 같다.

* 익숙하지 않음
* 가파른 학습 곡선
* 일시적으로 데이터가 맞지 않을 수 있다.
* 과도한 엔지니어링
* 유일성을 제약하기 어렵다.
* 도구가 부족

## Example

[.Net으로 만든 Event Sourcing 예제][dotnetsourcing]

# Implementing Event Sourcing & CQRS(Track A) - 쿠팡 심천보님

심천보님은 앞에 이규원님과 같은 주제의 구현부 발표를 진행해 주셨다. `Event Sourcing`과 `CQRS`, 인터넷 블로그에 있는 글을 읽을때는 이해하기 어려웠는데, 말로 설명을 들으니 금방 이해가 되고, 속이 뻥 뚫린것 같은 느낌이 들었다.

## Event Sourcing

`Event Sourcing`은 데이터 저장 방식의 새로운 패턴이다. 모든 상태 변화를 Event로 관리하고 **불변** 이며, **Append Only** 이다. 반드시 영속성을 가져야 한다. 데이터 복원 시 Event를 Replay한다.

## CQRS

명령과 조회의 책임을 분리한다. 상태 처리 모델과 조회 처리 모델을 분리한다.

조회 전용 모델이 별도로 필요하게 된다.

## Implementation

Event Sourcing Framework를 이용할 수 있으나, 흐름을 이해하기 어려울 수 있다며 직접 구현한 부분을 보여주셨다.

> 1. Command 객체 생성 & Validation
> 1. Service (Command Handler)
> 1. Aggregate 생성 -> Event 조회 -> Snapshot조회&병합 -> Event Replay
> 1. Doman 로직 수행,  Event 깩체 생성
> 1. save() 이벤트 저장, 스냅샷 생성 혹은 저장
> 1. getEvents() saveEvents(), Event Publisher EventProjector

[Event Sourcing Example][eventsourcingexample]

## 장점

객체/관계 불일치 해소  
변경사항에 대한 완벽한 이력 저장  
디버그 용이성  
탁월한 쓰기 성능

## 단점

익숙하지 않다.  
단순 모델에 적합하지 않음  
도구 부족 & 성숙되지 않은 기술  
Axon/Eventuate  
일반적인 쿼리 조회가 불가하므로 운영시에 불편하다(CQRS로 해소)


# Reactive Programming with RxJava(Track A) - 김인태님

RxJava, 공부하려고 책만 사놓고 아직까지 보질않고있다...ㅠㅠ 써야지, 공부해야지 하면서도 이상하게 자꾸 손이 안간다ㅠㅠ

김인태님은 많은 내용을 한 세션에 담으려다 보니 많은 내용을 생략하신 것 같은 느낌이 많이 들었다. 이런게 있는데 시간이 없어서 자세히 설명은 못하고 공부해서 써보세요~ 같은 느낌이랄까... 그래도 RxJava를 이해하기 위해서 비동기를 이해해야된다며 `Thread`를 통해서 그부분에 대해서 자세히 설명해주시는 모습이 좋았다. 마지막 질문에 동문서답하신건 안비밀... 마지막 질문이 Spring Reactor와 RxJava의 차이에 대한 질문을 누군가 했었는데, Spring Data와 RxJava의 차이에 대해서 답변을 하셧...응...?

## Introduction

Responsive, Resilient, Elastic, Message-driven

Iterable, Future, Observable => Push 방식

T -> Observable< T > -> Observable< R > -> Observable< T > -> T


# Spring Data Envers for Entity History(Track A) - 우아한형제들 김영한님

개인적으로 김영한님이 쓰신 `JPA 프로그래밍`을 정독하고 문화적 충격을 받았었고, 많은 도움을 받았어서 책을 가져가서 저자싸인을 받아보려 했는데... 이놈의 소심한 성격이 또 나와서 책만 가져가고 싸인을 받지 못하고 돌아왔다 ㅠㅠ 이번 역시 김영한님의 발표가 많은 도움이 됐다.

김영한님이 발표하면서 말하셨던 뭔가 어떻게하면 될 것 같은데 안되는... 그부분을 많이 고민하고있었는데, 역시 세상엔 똑똑한 사람이 많다. 이미 만들어져있을줄이야...

히스토리 남기는 부분은 정말 내가 이러려고 개발자했나 자괴감이 들 정도로 단순 반복 노동 작업인데, 지금까지는 단순 반복 노동을 엄청 했지만, 앞으로는 많은 양을 줄이고, 편하게 할 수 있을 것 같다.

## 데이터 관점의 공통 관심사

누가? 언제? 데이터를 변경했나?
변경 이력을 남겨야한다.
악덕 기획자를 만나면 피곤해진다.

## Spring Data Auditing

누가? 언제? 데이터를 생성하거나 변경했는지 검사

`@EnableJpaAuditing`

Entity에 `@CreatedDate`, `@LastModifiedDate`, `@CreatedBy`, `@LastModifiedBy` 를 사용

`@MappedSuperclass`를 통해 귀찮은 단순 반복작업을 피하자

## Hibernate Envers

하이버네이트 핵심 모듈
JPA 스펙에 정의된 모든 매핑 감사

* 엔티티의 변경 이력을 자동으로 관리
* XXX 테이블 -> XXX_AUD
* 히스토리를 계속 쌓는 방식으로 관리
* REV == Revision 식별자
* REVTYPE
  * 0: 등록
  * 1: 수정
  * 2: 삭제


`@Audited` -> Class or Method 에 사용

## 특정 트랜잭션 안에서 함께 변경된 히스토리를 보고 싶을 때

* REVINFO 테이블사용
* 트랜잭션 단위의 통합 Revision 키 관리

## 고급기능

* 필드마다 수정 상태 컬럼 추가

@Audited(withModifiedFlag = true)
FieldName_MOD

###  같은 트랜잭션에서 함께 변경된 엔티티를 검색

```plain
org.hibernate.envers.track_entities_changed_in_revision: true
```

Spring Data Envers를 사용하면 엄청 편하게 사용할 수 있다!


# 마치며...

개인적으로는 이번 모든 세션 발표 내용이 주옥같은 내용들이었다. 이번 스프링 캠프에서 정말 많은, 주옥같은 꿀같은 정보를 많이 얻게되어 너무 좋았다. 세션도 대놓고 서로 연관이 있도록 배치돼있는 것 같은 느낌이 드는것이, 주최자분들이 꽤나 고생을 하신것 같다. 첫날 참석 못한것이 아직도 많이 아쉽기는 하지만 내년 스프링 캠프를 기대하며 이만...


[reactive]: http://www.reactivemanifesto.org
[fluxexample]: https://github.com/joshlong/flux-flix-service
[session2]: https://doc.co/fggswS
[dotnetsourcing]: https://github.com/Reacture/Khala.EventSourcing/
[eventsourcingexample]: https://github.com/jaceshim/springcamp2017
