# 추상 팩토리(Abstract Factory)
## 개요
추상 팩토리 패턴 : 객체 생성을 추상화하고 관련된 객체 집합을 생성하는데 사용되는 디자인 패턴 이다. 관련성 있는 여러 종류의 객체를 일관된 방식으로 생성하는 경우에 유용하다. 

추상 팩토리는 추상 팩토리(Abstract Factory), 추상 제품(Abstract Product), 구상 팩토리(Concrete Factory), 구상 제품(Concrete Product)으로 구성된다.
* 추상 팩토리(Abstract Factory): 최상위 공장 클래스, 여러개의 제품들을 생성하는 여러 메소드를 추상화한다.
* 추상 제품(Abstract Product): 각 타입의 제품들을 추상화한 인터페이스를 정의한다.
* 구상 팩토리(Concrete Factory): 추상 팩토리의 생성 매서드를 구현한다. 
* 구상 제품(Concrete Product): 각 타입의 제품 구현체들, 팩토리 객체로부터 생성된다.

## 추상 팩토리의 장점
* 팩토리에서 생성되는 제품들의 상호 호환을 보장할 수 있다.
* 구상 제품들과 클라이언트 코드 사이의 단단한 결합을 피할 수 있다.
* 새로운 팩토리 클래스를 추가하거나 기존 팩토리 클래스를 수정하여 새로운 객체 패밀리를 지원할 수 있다.
* 다른 디자인 패턴과 조합하여 사용될 수 있으며, 객체 생성과 구성을 분리하는 디자인 패턴 중 하나인 빌더 패턴과 조합하여 객체 생성의 복잡성을 관리할 수 있다.

## 예시코드
```C++
#include <iostream>
#include <memory>

//추상 제품을 나타내는 animal 클래스 
class Animal {
public:
    virtual void speak() = 0;
}; 

// Concrete Product 
class Dog : public Animal {
public:
    void speak() override {
        std::cout << "Woof!" << std::endl;
    }
};

// Concrete Product 
class Cat : public Animal {
public:
    void speak() override {
        std::cout << "Meow!" << std::endl;
    }
}; //‘Dog’와 ‘Cat’ 클래스는 Animal클래스를 상속받아 동물을 나타내며 ‘speak’함수를 재정의 하여 소리를 출력한다.

// Abstract Factory
class AbstractAnimalFactory {
public:
    virtual std::unique_ptr<Animal> createAnimal() = 0;
}; //추상 팩토리를 나타내는 class로 하위 class에서는 이 구현하여 동물 객체를 생성해야한다.

// Concrete Factory 
class DogFactory : public AbstractAnimalFactory {
public:
    std::unique_ptr<Animal> createAnimal() override {
        return std::make_unique<Dog>();
    }
};

// Concrete Factory 
class CatFactory : public AbstractAnimalFactory {
public:
    std::unique_ptr<Animal> createAnimal() override {
        return std::make_unique<Cat>();
    }
};

int main() {
    std::unique_ptr<AbstractAnimalFactory> dogFactory = std::make_unique<DogFactory>();
    std::unique_ptr<Animal> dog = dogFactory->createAnimal(); // Dog Factory를 사용하여 Dog 객체 생성
    dog->speak();  //speak함수 호출하여 "Woof!"출력
    

    std::unique_ptr<AbstractAnimalFactory> catFactory = std::make_unique<CatFactory>();
    std::unique_ptr<Animal> cat = catFactory->createAnimal(); //Cat Factory를 사용하여 Cat 객체 생성
    cat->speak();  //speak함수 호출하여 "Meow!"출력
    return 0;
}

오픈소스코드 링크
https://thankfulinfo.co.kr/%ec%b6%94%ec%83%81-%ed%8c%a9%ed%86%a0%eb%a6%ac-%ed%8c%a8%ed%84%b4abstract-factory-pattern-%ec%9d%b4%ed%95%b4%ed%95%98%ea%b8%b0-%ec%89%bd%ea%b2%8c-%ec%84%a4%eb%aa%85%ed%95%9c-%eb%94%94%ec%9e%90/