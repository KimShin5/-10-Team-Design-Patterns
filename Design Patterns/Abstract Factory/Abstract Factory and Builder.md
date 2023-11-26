# 추상팩토리와 빌더
## 장점
* 객체 생성의 추상화와 세부 구현의 분리가 가능하다. 추상 팩토리는 객체 생성의 추상화하여 클라이언트가 구체적인 클래스를 알지 못해도 객체를 생성할 수 있게하며  빌더는 생성 과정을 세부적으로 다루면서도 그 과정을 추상화하여 클라이언트가 구체적인 단계를 알지 못해도 객체를 조립하게 한다.
* 빌더 패턴은 객체를 단계적으로 생성하게 하며 이로서 객체 생성에 여러 단계가 있을때 유용하며 객체를 조립하면서 객체의 일관성을 유지할 수 있다.
* 새로운 제품군이나 객체의 추가가 용이한다
* 추상 팩도리와 빌더의 패턴을 사용하면 클라이언트는 구체적인 클래스와의 직접적인 결합을 피할 수 있다. 

```C++
#include <iostream>
#include <string>

using namespace std;

// 추상 팩토리: 음료를 생성하는 인터페이스
class BeverageFactory {
public:
    virtual void createBase() = 0;
    virtual void addFlavor() = 0;
};

// 추상 빌더: 음료를 조립하여 최종 제품을 만드는 인터페이스
class BeverageBuilder {
public:
    virtual void buildBase() = 0;
    virtual void addFlavor() = 0;
    virtual void getResult() const = 0;
};

// 제품 클래스: 음료
class Beverage {
public:
    void showInfo() const {
        cout << "Beverage Information:" << endl;
        cout << "Base: " << base << endl;
        cout << "Flavor: " << flavor << endl;
    }

    void setBase(const string& b) {
        base = b;
    }

    void setFlavor(const string& f) {
        flavor = f;
    }

private:
    string base;
    string flavor;
};

// 팩토리 클래스: 커피 음료를 생성
class CoffeeFactory : public BeverageFactory {
public:
    void createBase() override {
        cout << "Creating coffee base" << endl;
    }

    void addFlavor() override {
        cout << "Adding coffee flavor" << endl;
    }
};

// 팩토리 클래스: 차 음료를 생성
class TeaFactory : public BeverageFactory {
public:
    void createBase() override {
        cout << "Creating tea base" << endl;
    }

    void addFlavor() override {
        cout << "Adding tea flavor" << endl;
    }
};

// 빌더 클래스: 커피 음료를 조립
class CoffeeBuilder : public BeverageBuilder {
private:
    Beverage beverage;

public:
    void buildBase() override {
        beverage.createBase();
    }

    void addFlavor() override {
        beverage.addFlavor();
    }

    void getResult() const override {
        beverage.showInfo();
    }
};

// 빌더 클래스: 차 음료를 조립
class TeaBuilder : public BeverageBuilder {
private:
    Beverage beverage;

public:
    void buildBase() override {
        beverage.createBase();
    }

    void addFlavor() override {
        beverage.addFlavor();
    }

    void getResult() const override {
        beverage.showInfo();
    }
};

// 클라이언트 코드: 팩토리와 빌더를 조합하여 음료 생성
int main() {
    BeverageBuilder* builder;
    BeverageFactory* factory;

    // 커피 음료 생성
    factory = new CoffeeFactory();
    builder = new CoffeeBuilder();
    builder->buildBase();
    builder->addFlavor();
    builder->getResult();

    // 차 음료 생성
    factory = new TeaFactory();
    builder = new TeaBuilder();
    builder->buildBase();
    builder->addFlavor();
    builder->getResult();

    return 0;
}
```
* 'BeverageFactory'클래스와 그 하위 클래스는 추상 팩토리 패턴을 따른다. 팩토리 패턴은 관련 객체 그룹을 생성하는 역할을 한다.
* 'BeverageBuilder'클래스와 그 하위 클래스는 빌더 패턴을 따르는데 복잡한 객체 생성과정을 캡슐화하고 단계벼로 객체를 생성하게 한다. 'buildBase()', 'addFlavor()', 'getResult()'등을 통해 단계별로 음료를 조립하고 빌더 클래스는 조립 세부 사항을 다르게 구현한다. 빌더 패턴은 객체 생성의 세부 사항을 다향화 하면서도 일관된 인터페이스를 제공한다. 
* 사용자는 특정 객체 생성과정을 몰라도 되며 동시에 새로운 객체를 추가하거나 변경이 상대적을 쉽게 가능하다.