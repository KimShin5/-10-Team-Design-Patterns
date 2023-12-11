# 복합체(Composite)
## 개요 
복합체 패턴: 객체를 tree와 같은 구조로 구성하여 전체-부분 계층 구조를 나타내는 디자인 패턴, 단일 객체(leaves)와 복합 그룹(composite)을 일관되게 다룰 수 있도록 만들어주기 때문에 복잡한 객체 구조의 상호작용과 제어를 용이하게 한다.

복합체는 Component, Composite, Leaf 3가지 주요 컴포넌트로 구성되어있다.
* Component(구성 요소): Leaf와 Compsite를 묶는 공통적 상위 인터페이스
* Composite(복합 개체): 복합 개체로서, Leaf 역할이나 Composite 역할을 넣어 관리하는 역할을 한다.
* Leaf(단일 객체):'Component'를 구현하는 클래스 중 단일 객체에 해당하는 클래스, 실제 작업을 수행한다.

# 예시코드
```C++
#include <iostream>
#include <vector>

using namespace std;

// Component interface
class Graphic {
public:
    // 순수 가상 함수
    virtual void draw() = 0;
};

class Circle : public Graphic {
public:
    // 부모 클래스에서 정의된 순수 가상 함수를 구현
    void draw() override {
        cout << "Drawing a Circle..." << endl;
    }
};

class Rectangle : public Graphic {
public:
    // 부모 클래스에서 정의된 순수 가상 함수를 구현
    void draw() override {
        cout << "Drawing a Rectangle..." << endl;
    }
};

// Composite 클래스: Drawing
class Drawing : public Graphic {
public:
    // 부모 클래스에서 정의된 순수 가상 함수를 구현
    void draw() override {
        cout << "Drawing:" << endl;
        for (auto shape : shapes) {
            shape->draw();
        }
    }

    // 도형추가
    void addShape(Graphic* shape) {
        shapes.push_back(shape);
    }

private:
    vector<Graphic*> shapes; // 도형들을 저장하는 벡터
};

int main() {
    Circle circle;
    Rectangle rectangle;

    Drawing drawing;
    drawing.addShape(&circle);
    drawing.addShape(&rectangle);

    drawing.draw(); 

    return 0;
}