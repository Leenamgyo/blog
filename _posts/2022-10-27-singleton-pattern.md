---
layout: post
title:  "[DesignPattern] Singleton Pattern"
date:   2022-10-21 13:00:00 +0900
categories: designPattern
---

# What it Singleton pattern
싱글톤(singleton)이란 인스턴스는 오직 하나만 생성할 수 있는 클래스를 말한다. 싱글톤의 전형적인 예로는 함수와 같은 무상태 객체나 설계상 유일해야 하는 시스템 컴포넌트를 들 수 있다. 구체적인 예로는 싱글톤 방식은 스레드풀, 캐시, 대화상자, 사용자 설정, 레지스트리 설정 객체, 로그 기록 객체, 디바이스 드라이브 객체 등 이러한 경우에 사용된다. 싱글톤 생성 시 중요한 것 중 하나는 "어떻게 멀티스레드 환경에서 한개의 객체를 효울적으로 생성할 것인가?"가 쟁점인 듯 싶다.
<br><br>

### Example 1: 먼저, 유일한 객체를 한번 만들어보자. 
{% highlight js %}
public class Singleton {
    private static Singleton uniqueInstance;
    private Singleton() {}
    public static Singleton getInstance() {
        if (uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }

        return uniqueInstance;
    }
}
{% endhighlight %}

단순하게 싱글톤 구현에만 초점이 맞춰져 있는 고전적인 싱글톤 구현방법이라고 한다. 위 코드는 private로 생성자를 선언함에 따라 default 생성자가 생기는 것을 방지하고 외부에서 생성을 컨트롤 할 수 없게 만든다. 즉, 이 클래스의 객체의 생성 및 접근은 getInstance 메소드로만 진행되며 이러한 접근을 통해 객체 갯수의 유일성을 얻게 된다. 한편, 이렇게 구현할 시 동시적으로 접근이 가능한 멀티스레드 환경에서 getInstance의 동시호출로 인해 한번에 여러개의 객체가 생성될 수 있어 유일한 객체라는 싱글톤의 특성을 보장할 수 없다. 
<br><br>


### Example 2: 동시접근, 어떻게 방어해야되지?
자바에서는 편리한 thread-safe 방법인 locking을 구현한 synchronized라는 것을 지원합니다. 

{% highlight js %}
public class Singleton {
    private static Singleton uniqueInstance;
    private Singleton() {}
    public static synchronized Singleton getInstance() {
        if (uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }

        return uniqueInstance;
    }
}
{% endhighlight %}

한편, 객체가 생성된 이후 즉, 객체가 하나라는 것을 보장된 이후도 thread-safe한 행동을 하는 것은 오버 엔지니어링이 될 수 있습니다. 이러한 결과는 약 100배 정도의 성능이 저하될 수 있다고 합니다. 
<br><br>

### Exmaple 3: 동시접근은 알겠고.. 성능 개선은 어떻게 하지?.
DCL(Double Checked Locking) 방식이라고 하며 동기화할때마다 호출할 때마다 동기화하는 방식이 아니라 생성된 인스턴스가 존재하지 않을 때만 lock을 걸어 인스턴스 생성 과정에서 발생할 수 있는 동시성 문제를 해결하는 방식입니다. 단순하게 접근하자면 synchronized 선언을 전체에서 부분으로 줄이는 방식입니다. 따라서 전체적으로 동작하는 synchronized 동작을 부분으로 줄일 수 있습니다.

{% highlight js %}
public class Singleton {
    private violate static Singleton uniqueInstance;
    private Singleton() {}
    public static Singleton getInstance() {
        if (uniqueInstance == null) {
            synchronized(Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }        
        }
        return uniqueInstance;
    }
}
{% endhighlight %}

이러한 로직은 synchronized가 동작하는 범위를 전체에서 부분으로 줄임으로써 synchronized로 동작하는 부분을 획기적으로 줄일 수 있습니다. 다만,
<br><br>

### Example 4: class Loader를 속이자 

{% highlight js %}
public class Singleton {
    private Singleton() {}

    public static Singleton getInstance() {
        return LazyHolder.INSTANCE;
    }

    private static class LazyHolder() {
        private static final Singleton INSTANCE = new Singleton();
    }
}
{% endhighlight %}

### Example 5: Enum의 특성을 이용해보는 건 어때?
접근제한자로 클래스의 생성을 방지하는 방법은 리플렉션 클래스, 직렬화/역직렬화에 취약합니다. 따라서 enum 클래스를 이용하면 코드도 간단할 뿐더러 이 취약한 부분을 해결할 수 있습니다. 또한, 동시성 문제, 클래스 로딩 문제 등 다양한 문제에 사용 가능합니다. 
{% highlight js %}
enum Singleton {
    UNIQUE_INSTANCE;
}
{% endhighlight %}

하지만 인스턴스 생성 외에 다른 부분에 대해 thread-safe를 보장하려면 개발자가 직접 구현해야 한다는 단점이 있습니다.
<br><br>

# 결론
아마 enum 클래스는 역직렬화를 수행할 때마다 객체 생성을 한다거나 리플렉션으로 접근제한자에 변경에 대한 상황을 방어할 수 있다. 다만, lazy


https://refactoring.guru/design-patterns/singleton
https://en.wikipedia.org/wiki/Singleton_pattern
https://dev-sanghun.tistory.com/38