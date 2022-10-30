---
layout: post
title:  "[DesignPattern] Decorator Pattern"
date:   2022-10-10 13:00:00 +0900
categories: designPattern
---

## What is Decorator Pattern 
GoF Design Pattern 중 '구조 패턴(클래스나 객체를 더하여 더 큰 구조를 만듦)'에 속하는 데코레이터 패턴은 상속(서브클래스)을 구현하지 않고 객체 작성이라는 형식으로 실행 중에 클래스를 꾸밀 수 있다. 데코레이터 패턴을 사용하면 기존 클래스 코드를 바꾸지 않고도 객체에 새로운 임무를 추가할 수 있습니다.

## SubClass vs Decorator pattern
innos blog를 인용한다면 현재 만들어진 클래스를 이용해서 기능을 확장하는 방법은 '서브클래스'를 만들거나 '데코레이터 패턴'을 두 가지의 선택지가 있다고 한다. 두 가지 중 먼저, 서브클래스는 우리가 사용하는 상속에 대한 이야기이다. 이 블로그에서 확장 시, 서브클래스는 약간의 혼란을 준다고 한다. 위 블로그에서 혼란에 대한 이유를 명확하게 설명하지 않았는데, 내 생각에는 상위클래스의 종속성을 따라야하는 것 때문에 단순한 확장보다는 종속성을 고려한 설계를 고려해야 되기 때문이 아닐까 싶다. 하지만, 데코레이터는 상위 클래스의 종속성을 따르지 않아도 되며 확장 시, 소스의 변경을 요구하지 않기 때문에 서브클래스보다 자유로운 확장이 가능하다.

### Example: SubClass 
HeadFirstDesignPattern의 나온 예제를 한번 보자. 카페의 주문 시스템을 만들고 있다. 여기서는 서브클래스를 두 가지로 나누며, 한 가지의 경우는 각 재료마다 많은 클래스를 만드는 방식, 다른 한 가지의 경우는 모든 특성을 포함하는 하나의 클래스르 만들고 수퍼클래스에 대한 몇 가지의 서브클래스를 만들어 사용한다.


__첫 번째 예제를 한번 확인해보자__
![subclass_1](/assets/img/design-pattern/decorator_subclass.png)

음료에 대해 가격을 오바라이드하여 클래스를 설계하는 형태다. 이러한 형태는 단순히 슈퍼클래스가 형태만 상속할 뿐 내부 로직의 대부분을 서브클래스가 맡는다. 이러한 경우는 새로운 메뉴 확장은 쉬우나 관리할 클래스가 많고 레시피를 코드화 시키지 않은 형태이기 때문에 변화에 대응하기 어렵다. 예를 들어, 우유가격 등 재료 가격이 바뀔 경우 각 서브 클래스를 접근해 가격을 일일이 바꾸어야 한다. 또한, 변경시 해당 클래스에 접근하여 일일이 레시피를 확인하여 대응해야 하는 등 관리에 대한 피로감이 늘어난다. 또한, cost()만으로 이루어진 코드는 레시피를 코드화 시키지 않아 따로 레시피를 문서화하여 명시적으로 관리해주어야 하는 문제가 있습니다. 

__두 번째 예제를 한번 확인해보자__
![subclass_2](/assets/img/design-pattern/decorator_subclass_2.png)

첫 번째 예제보다는 클래스의 개수가 적어지고 재료 가격을 미리 정해놓아 코드로 가격을 표시할 수 있다. 또한 Beverage라는 슈퍼 클래스에서 원재자 가격을 정할 수 있으니 가격 변화에 대한 대응이 쉽다. 다만, 이러한 방식은 사용하지 않는 재료를 서브클래스에서 조작할 수 있는 점은 현실세계의 메뉴판을 완전히 표현할 수 없다. 예를 들어, 아메리카노를 주문하는데 setMilk, setMocha 등 아메리카노와 무관한 함수 및 메소드도 아메리카노에서 사용 가능하다.  

### Example: Decorator 

![decorator](/assets/img/design-pattern/decorator_decorator.png)    
이제는 데코레이턴 예제를 한번 보자. 데코레이터 패턴을 이용해 객체에 추가 요소를 동적으로 더할 수 있다. 재료가 추가/삭제가 되면 하나의 데코레이터를 덮거나 삭제를 하면되고 또한 다른 클래스에는 영향을 미치지 않는다. 그리고 각 원재료의 결합방식은 추가/삭제가 자유롭기 때문에 원재료의 관리가 자유롭다.  

![decorator_2](/assets/img/design-pattern/decorator_decorator_2.png)    
다음은 데코레이터에 대한 클래스 다이어 그램이다. 이 클래스 다이어그램을 보면 새로운 재료가 생성될 때마다 beverage 클래스가 composition하는 방식이다. 즉, 하나의 구현체는 각 클래스로 만들어진 재료를 입장할 때마다 가격과 description 멤버함수를 함쳐서 return 하게 된다.  

{% highlight js %}

abstract class Beverage {
    String description; 

    public String getDescription() {
        return description;
    }

    abstract public double cost(); 
}

abstract class CondimentDecorator extends Beverage {
    Beverage beverage;
    public abstract String getDescription();
}

class Espresso extends Beverage {

    public Espresso() {
        description = "에스프레소";
    }

    public double cost() {
        return 1.99;
    }
}

class HouseBlend extends Beverage {
    public HouseBlend() {
        description = "하우스 블렌드 커피";
    }

    public double cost() {
        return .89;
    }
}


class DarkRoast extends Beverage {
    public DarkRoast() {
        description = "하우스 블렌드 커피";
    }

    public double cost() {
        return .79;
    }
}


class Mocha extends CondimentDecorator {
    
    public Mocha(Beverage beverage) {
        this.beverage = beverage;
    }

    public String getDescription() {
        return beverage.getDescription() + ", 모카";
    }
    
    public double cost() {
        return beverage.cost() + .20;
    }
}

class Soy extends CondimentDecorator {
    
    public Soy(Beverage beverage) {
        this.beverage = beverage;
    }

    @Override
    public String getDescription() {
        // TODO Auto-generated method stub
        return beverage.getDescription()+ ", 두유";
    }

    @Override
    public double cost() {
        // TODO Auto-generated method stub
        return beverage.cost() + .31;
    } 

}

class Whip extends CondimentDecorator { 
    public Whip(Beverage beverage) {
        this.beverage = beverage;
    }

    @Override
    public String getDescription() {
        // TODO Auto-generated method stub
        return beverage.getDescription() + ", 크림";
    }

    @Override
    public double cost() {
        // TODO Auto-generated method stub
        return beverage.cost() + .30;
    }

    
}

public class StarbuzzCoffee {
    public static void main(String[] args) throws Exception {
        Beverage beverage = new Espresso();
        System.out.println(beverage.getDescription() + " $" + beverage.cost());
    
        Beverage beverage2 = new DarkRoast();
        beverage2 = new Mocha(beverage2);
        beverage2 = new Mocha(beverage2);
        beverage2 = new Whip(beverage2);
        System.out.println(
            beverage2.getDescription() + " $"+ beverage2.cost()
        );

        Beverage beverage3 = new HouseBlend();
        beverage3 = new Soy(beverage3);
        beverage3 = new Mocha(beverage3);
        beverage3 = new Whip(beverage3);
        System.out.println(beverage3.getDescription()
            + " $"+ beverage3.cost()
        );
    }
}

{% endhighlight %}




## The Relationship between the Decorator Pattern and OCP(Open Closed Principle)
유지보수와 확장이 쉬운 소프트웨어를 개발하는데 도움이 되는 원칙인 '객체 지향의 5원칙'이 있다. 5개의 원칙 중 한가지인 OCP의 원칙은 "클래스는 확장에는 열려 있어야 하지만 변경에는 닫혀있어야 한다는 것"이다. 이러한 데코레이터 패턴은 함수 확장시 내부 기능을 수정하지 않고 다른 기능을 접합하고 뗄 수 있는 특성은 확장에 열려있다는 것이며, 변경에 닫혀있다는 소리가 된다. 즉, 데코레이터 패턴은 OCP 원칙과 잘 부합하며 이는 유지보수 확장이 쉬운 소프트웨어를 개발하는데 장점이 될 수 있다. 

## Well-known example: Java I/O class 
![java_decorator](/assets/img/design-pattern/decorator_java_io.png)

Java I/O 클래스는 Decorator class를 이용해 만들어진 대표적인 예제이다. 
{% highlight js %}

import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.FilterInputStream;
import java.io.IOException;
import java.io.InputStream;

class LowerCaseInputStream extends FilterInputStream {

    public LowerCaseInputStream(InputStream in) {
        super(in);
    }
    
    public int read() throws IOException { 
        int c = in.read();
        return (c == -1 ? c : Character.toLowerCase((char)c));
    }

    public int read(byte[] b, int offset, int len) throws IOException {
        int result = in.read(b, offset, len);
        for (int i = offset; i < offset+result; i++) {
            b[i] = (byte)Character.toLowerCase((char)b[i]);
        }

        return result;
    }

}


public class JavaIOExample {
    public static void main(String[] args) {
        int c;
        
        try {
            InputStream in = 
                new LowerCaseInputStream(
                    new BufferedInputStream(
                        new FileInputStream("text.txt")));

            while ((c = in.read()) >=0) {
                System.out.println((char)c);
            }
            
            in.close();

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}


{% endhighlight %}

## 결론
 기존의 소스를 변경하지 않고 얹히는 듯한 이 데코레이터 디자인 패턴을 이용한다면 객체 지향 5원칙 중 하나인 OCP에 충실한 유연한 디자인을 만들 수 있으나 자잘한 클래스가 많이 추가되어 소스 이해의 난이도가 증가할 수도 있다. 또한 특정 클라이언트에 의존해야하는 경우에도 데코레이터를 오용하는 경우는 피해야 한다. 또한 IONOS 블로그에 따르면 디버깅이 어려워진다는 단점이 있다.  
 
## 참조 
 - https://www.ionos.com/digitalguide/websites/web-development/what-is-the-decorator-pattern
 - Head First Design Pattern 