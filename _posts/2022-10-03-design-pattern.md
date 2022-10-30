---
layout: post
title:  "[DesignPattern] 디자인 패턴에 대한 생각정리"
date:   2022-10-03 13:00:00 +0900
categories: designPattern
---

## 실생활에서의 패턴 
 회사에 신입이 들어와 팀에 배정을 받는다. 아마 팀의 상사는 신입에 대해서 아무것도 모를 것이고 어색할 것이다. 신입이 업무에 투입되고 시간이 지나 신입에 대해 많은 것을 알게된다. 성격, 특성, 루틴 등 일의 시너지를 만드는 방법 등 신입과 상호작용 하는 것에 대한 방법이 수립된다. 반면, 신입 또한 상사를 알게된다. 또한, 이제 신입은 회사 일에 적응하여 사원이 되어 상사가 요구하는 부분에 대해 상사가 많은 것을 설명해주지 않더라도 많은 것을 알 수 있다.  
 
 이처럼 상대방의 행동을 이해 한다면 상대방과 굳이 많은 의사소통을 하지않아도 상대방을 의중을 알아차릴 수 있으며 이해를 기초로 상대방에 대한 패턴을 정립한다면 더 많은 시너지를 낼 수 있을 것 같다.

 다만, 이러한 패턴을 맹목적으로 믿으며 시간이 지나도 이 정립된 패턴만을 고집한다면 상사는 그 신입의 발전할 수 있는 개성을 온전히 살릴 수 없을 것이다. 
 
## 컴퓨터의 디자인 패턴 
 컴퓨터 업무 또한 자주 사용하는 방식들 즉, 패턴을 찾고 그에 대한 일반적인 해결법을 만들어 사용하고 있다. 1995년에 집필된 디자인 패턴 베스트 셀러 서적인 **GoF Desgin Pattern**와 1996년 부터 시작해 2007년 집필을 마무리한 **패턴 지향 아키텍처(POSA), Volume 1, 2 ... 5** 등 많은 서적들이 집필되고 패턴들이 정립화되어 현재도 자주 사용되고 있다.

 디자인 패턴의 장점 구현 설계의 베스트 셀러인 **Code Complete**를 읽어보면 복잡성을 줄이고 일반적인 문제의 해결책을 제공하는 등 4가지의 장점을 나열하였고 발표한 **GoF Design Pattern** 에서는 재사용성, 유지관리, 분석, 의사소통에 따른 부분에 많은 이점이 있다고 한다. 

## 패턴을 나누는 3가지 
 GoF Design Pattern은 1995년 GoF(Gang of Four)라고 불리는 프로그래머계의 거장들인 Erich Gamma, Richard Helm, Ralph Johnson, John Vissides가 Object-Oriented-Programming을 토대로 패턴을 정립한 저서이다. 4명의 개발자는 ‘경험’이나 ‘내적인 축적’을 디자인 패턴이라는 형태로 정리하였다. GoF 4인방은 패턴을 나누기 전 3개의 구조(Structural Pattern), 행동(Behavior Pattern), 생성 (Creational Pattern) 3가지의 형식으로 분류하였다. 

 구조 패턴(Structural Pattern)은 클래스나 객체를 조합해 더 큰 **구조를 만드는 패턴**이다.예를 들어 서로 다른 인터페이스를 지닌 2개의 객체를 묶어 단일 인터페이스를 제공하거나 객체들을 서로 묶어 새로운 기능을 제공하는 패턴이다. 

 - 어답터 패턴 (Adapter Pattern)
 - 브릿지 패턴 (Bridge Pattern) 
 - 합성 패턴 (Composition Pattern)
 - 데코레이터 패턴 (Decorator Pattern)
 - 퍼사드 패턴 (Facade Pattern)
 - 플라이웨이트 패턴 (Flyweight Patern) 
 - 프록시 패턴(Proxy Pattern)

 행동 패턴 (Behavior Pattern)은 객체나 클래스 사이의 알고리즘이나 **책임 분배**에 관련된 패턴이다. 한 객체가 수행할 수 없는 작업을 여러 개의 객체로 어떻게 분배하며 객체 사이의 결합도 최소화에 중점을 둔다. 패턴을 주로 클래스에 적용하는지 아니면 객체에 적용하는지에 따라 구분되는 패턴이다.

 - 책임연쇄 패턴(Chain of responsibility)
 - 커맨드 패턴(Command Pattern)
 - 인터프리터 패턴(Interpreter Pattern)
 - 이터레이터 패턴 (Iterator Pattern)
 - 옵저버 패턴(Observer Pattern)
 - 전략 패턴 (Strategy Pattern)
 - 템플릿 메서드 패턴 (Template method pattern)
 - 방문자 패턴 (visitor Pattern)
 - 중재자 패턴 (Mediator Pattern)
 - 상태 패턴 (State Pattern)
 - 기념품 패턴 (Memento Pattern)

 생성 패턴 (Creational Pattern)은 객체 생성에 관련된 패턴으로, 객체의 생성과 조합을 캡슐화해 특정 객체가 생성되거나 변경되어도 프로그램 구조에 영향을 크게 받지 않도록 유연성을 제공한다.

 - 추상화 팩토리 패턴 (Abstract Factory Methods)
 - 빌더 패턴 (Builder Pattern) 
 - 팩토리 패턴 (Factory Method Pattern)
 - 프로토타입 패턴 (Prototype Pattern) 
 - 싱글톤 패턴 (Singleton Pattern) 

## 한편으로는..
**CodeComplete**에 따르면 이러한 패턴이 우리 프로젝트에 많은 이점을 가져올 것이라고 하지만, 한편으로는 이러한 형식화된 패턴을 억지로 끼워 맞추는 것을 조심히라고 한다. 또한 **IONOS blog**에 따르면 디자인 패턴을 사용하는 것은 많은 지식을 요구하며, 기존의 만들어진 문제를 풀 수 있다는 명백한 믿음을 줌으로써 상대방의 창조성과 새로운 것을 만들고자 하는 것을 줄일 수 있다는 욕구를 줄인다는 것이다. 

## 참조 
- https://www.ionos.com/digitalguide/websites/web-development/what-are-design-patterns/
- https://www.gofpatterns.com/
- https://ko.wikipedia.org/wiki/디자인_패턴
- https://en.wikipedia.org/wiki/Design_Patterns