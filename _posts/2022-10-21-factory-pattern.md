---
layout: post
title:  "[DesignPattern] Factory Pattern"
date:   2022-10-21 13:00:00 +0900
categories: designPattern
---

## What is Factory Pattern 
팩토리 패턴(factory Pattern)은 자주 쓰이는 패턴을 엮어서 집필한 GoF의 '생성 패턴' 중 하나입니다. 팩토리 패턴을 사용한다면 유사한 속성을 가진 다양한 객체에 대한 생성을 컨트롤 할 수 있게 되면서 특정한 클래스와 직접적으로 결합하지 않으면서 느슨한 결합(loosed Coupling)을 만들 수 있습니다. new 연산자를 사용하여 구체적인 클래스를 구현하는 것이 대신 추상화된 클래스로 정의하고 각 구체적인 클래스의 생성은 서브 클래스에 맡기고 매칭되는 클래스를 인스턴스화 시켜 객체 생성을 합니다. 팩토리 패턴은 잘 알려진 2가지의 방법이 있는데 이는 메소드를 이용한 '팩토리 메소드 패턴'과 Composition을 이용한 '추상 팩토리 패턴'이다. 

# Example 1: let's make the Factory Pattern simply
워밍업으로 이번 예제는 팩토리를 가볍게 알아가기 위해 준비했다. 사실, 이번 예제는 '팩토리'는 맞지만 '패턴'의 계열로 불리는 것은 아쉽다. 이 예제는 간단하게 팩토리 클래스를 만들어 그 안에 알맞은 것을 생성시키는 방법이다. 이번 예제에서 알아갈 내용은 특정한 객체를 생성하는 것이 아니라 추상적으로 만든 속성의 집합에 해당되는 객체를 가져오는 것이다. 간단한 피자 주문에 대한 예제이다.

{% highlight js %}
public class SimplePizzaStore {
    
    public Pizza createPizza(String type) {
        if (type.equals("cheese")) {
            pizza = new CheesePizza();
        } else if (type.equals("pepperoni")) {
            pizza = new PepperoniPizza();
        } else if (type.equals("clam")) {
            pizza = new ClamPizza();
        } else if (type.equals("veggie")) {
            pizza = new VeggiePizza();
        }
    }
    
    public Pizza orderPizza(String type) {
        Pizza pizza;

        pizza = factory.createPizza(type);

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }

}
{% endhighlight %}

# Example 2: 팩토리 메소드로 의존성을 변경해보자 
createPizza를 추상적인 메소드로 만들어서 피자 클래스 클래스를 추상화시켜 슈퍼클래스에서 서브클래스로 책임을 넘긴다. 이렇게 되면 생성에 대한 것은 서브클래스로 넘어가고 슈퍼클래스에서는 Pizza가 이미 생성되었다는 가정하에 로직을 작성하게 된다. 따라서 슈퍼클래스에서는 pizza의 구체적인 로직에 대한 의존성을 없앨 수 있다. 

{% highlight js %}
public abstract class PizzaStore {
    public Pizza orderPizza(String type) {
        Pizza pizza;

        pizza = createPizza(type);

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }

    abstract Pizza createPizza(String type);
}

public class NYPizzaStore extends PizzaStore {

    Pizza createPizza(String item) {
        if (item.equals("cheese")) {
            return new NYStyleCheesePizza();
        } else if (item.equals("veggie")) {
            return new NYStyleVeggiePizza();
        } else if (item.equals("clam")) {
            return new NYStyleClamPizza();
        } else if (item.equals("peperoni")) {
            return new NYStylePeperoniPizza();
        } else return null;
    }
}

public class ChicagoPizzaStore extends PizzaStore {

    Pizza createPizza(String item) {
        if (item.equals("cheese")) {
            return new ChicagoStyleCheesePizza();
        } else if (item.equals("veggie")) {
            return new ChicagoStyleVeggiePizza();
        } else if (item.equals("clam")) {
            return new ChicagoStyleClamPizza();
        } else if (item.equals("peperoni")) {
            return new ChicagoStylePeperoniPizza();
        } else return null;
    }
}
{% endhighlight %}

orderpizza와 같은 공용으로 사용하는 메소드는 수퍼클래스에서 처리하였고 생성은 서브클래스에서 담당한다. 서브클래스에서 생성을 다르게 하기 때문에 슈퍼클래스라는 집합 안에 뉴욕스타일, 시카고 스타일의 피자로 나눌 수 있다.

# 의존성 뒤집기 원칙(Dependency Inversion Principle) 
"추상적인 것에 의존하게 만들고 구상 클래스에 의존하지 않게 만든다" 즉, 구상 클래스처럼 구체적인 클래스가 아닌 추상 클래스나 인터페이스와 같이 추상적인 것에 의존하는 코드를 만들어야 합니다.  


# 추상 팩토리 패턴의 정의
추상 팩토리 패턴은(Abstract Factory Pattern)은 구상 클래스에 의존하지 않고도 서로 연관되거나 의존적인 객체로 이루어진 제품군을 생산하는 인터페이스를 제공합니다. 구상 클래스는 서브클래스에서 만듭니다.

{% highlight js %}
public interface PizzaIngredientFactory {

    public Dough createDough();
    public Sauce createSauce();
    public Cheese createCheese();
    public Veggies[] createVeggies();
    public Pepperoni createPepperoni();
    public Clams createClam();

}


class NYPizzaIngredientFactory implements PizzaIngredientFactory {

    @Override
    public Cheese createCheese() {
        return new ThinCrustDough();
    }

    @Override
    public Clams createClam() {
        return new FreshClams();
    }

    @Override
    public Dough createDough() {
        return new ReggianoCheese();
    }

    @Override
    public Pepperoni createPepperoni() {
        return new SlicedPepperoni();
    }


    @Override
    public Sauce createSauce() {
        return new MarinaraSauce();
    }

    @Override
    public Veggies[] createVeggies() {
        Veggies veggies[] = { new Garlic(), new Orion(), new Mushroom(), new RedPepper()};
        return veggies;
    }
    
}

public abstract class NewPizza {
    String name;
    Dough dough;
    Sauce sauce;
    Veggies veggies[];
    Cheese cheese;
    Pepperoni pepperoni;
    Clams clam;

    abstract void prepare();

    void bake() {
        System.out.println("175도에서 25분간 굽기");
    }

    void cut() {
        System.out.println("피자를 사선으로 자르기");
    }

    void box() {
        System.out.println("상자에 피자 담기");
    }

    void setName(String name) {
        this.name = name;
    }

    String getName() {
        return this.name;
    }

    @Override
    public String toString() {
        return "NewPizza [name=" + name + ", dough=" + dough + ", sauce=" + sauce + ", veggies="
                + Arrays.toString(veggies) + ", cheese=" + cheese + ", pepperoni=" + pepperoni + ", clam=" + clam + "]";
    }
}

class CheesePizza extends NewPizza {
    PizzaIngredientFactory ingredientFactory;

    public CheesePizza(PizzaIngredientFactory pizzaIngredientFactory){
        this.ingredientFactory = ingredientFactory;
    }

    void prepare() {
        System.out.println("준비 중"+ name);
        dough = ingredientFactory.createDough();
        sauce = ingredientFactory.createSauce();
        cheese = ingredientFactory.createCheese();
    }
}

{% endhighlight %}



# 팩토리 메소드 vs 추상 팩토리 패턴의 차이점은 무엇인가?
둘 다 객체를 생성하는 것은 동일하나 팩토리 메소드는 서브 클래스로 생성을 하고 추상 클래스는 합성으로 객체 생성을 한다. 팩토리 메소드를 이용하면 상속으로 인한 이점을 누릴 수 있고 추상 클래스로 만드는 것은 합성을 사용한 이점을 누릴 수 있다. 또한, 추상 클래스 팩토리 패턴의 이러한 형태는 보통 많은 제품을 생산하기에 적합하다. 따라서 팩토리 메소드 패턴은 보통 한 개의 생성을 만들 때, 추상 클래스 팩토리 패턴은 여러개의 객체를 만들 때 사용한다.

한편으로 더 생각하자면, 팩토리 메소드는 슈퍼클래스-서브클래스의 관계를 생각해서 일반화-구체화의 방식을 따라야 하는 만큼 종속성에 따른 제한이 있다면 추상 팩토리 클래스는 합성 관계이기 때문에 이러한 종속성보다는 좀 더 자유로울 수 있기 때문에 ingredientFactory클래스 처럼 속성을 주입시키는 것이 팩토리 메소드보다 더 자유로울 것 같다.

