---
layout: post
title:  "[DesignPattern] stratage pattern - 하나의 클래스에서 많은 인터페이스를 가진다면? 많은 소스를 어떻게 리팩토링 할 것인가? "
date:   2023-10-03 13:00:00 +0900
categories: designPattern
---

내가 하고 있는 디자인패턴 스터디 모임에서 이러한 이야기가 나왔다. 전략 패턴을 구성하다가 인터페이스가 너무 많아지면 소스 관리를 어떻게 해야 하나요? 일일이 인터페이스를 규정하는 함수를 만들면 똑같은 패턴의 소스가 많아질 것이고 이 많은 소스를 관리하기는 어려울 것 같습니다. 

먼저, 전략패턴에 대해 알아가야 한다. 전략패턴(strategy pattern)은 인터페이스를 정의하고 상황에 맞게 이 인터페이스의 동작할 행동 요령(전략)을 규정하는 것이다. 이 전략패턴은 따라서 인터페이스를 변경할 수 있는 인터페이스 정의 함수와 이 실행할 함수를 수행해야할 '실행'할 함수를 만들어야 한다. 예를 들어 이러한 방식이다. 

이것을 조금 더 효율적으로 변경할 방법은 __여러개의 interface를 관리할 수 있는 수단을 만드는 것 같아 그런식으로 한번 효율화를 시켜보는 것이 좋다고 느껴졌다.__

{% highlight js %}
// Example can be run directly in your JavaScript console

// Create a function that takes two arguments and returns the sum of those arguments
var adder = new Function("a", "b", "return a + b");

// Call the function
adder(2, 6);
// > 8
{% endhighlight %}

