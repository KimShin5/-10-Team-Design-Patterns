# 복합체 코드
```C++
#include <iostream>
#include <string>

using namespace std;

// 크리스마스 트리 요소를 나타내는 인터페이스 클래스
class ChristmasTreeElement {
public:
    // 크리스마스 트리 요소 정보를 출력하는 가상 함수
    virtual void draw() const = 0;
};

// 리프 클래스: 장식
class Decoration : public ChristmasTreeElement {
private:
    string decorationName;

public:
    // 장식 객체 생성자
    Decoration(const string& name) : decorationName(name) {}

    // 장식 정보를 출력하는 함수
    void draw() const override {
        cout << "장식: " << decorationName << endl;
    }
};

// 복합 클래스: 크리스마스 트리
class ChristmasTree : public ChristmasTreeElement {
private:
    Decoration* star;
    Decoration* ribbon;

public:
    // 크리스마스 트리 객체 생성자, 객체를 가르키는 포인터
    ChristmasTree(const string& starName, const string& ribbonName)
        : star(new Decoration(starName)), ribbon(new Decoration(ribbonName)) {}

    // 크리스마스 트리와 포함된 내용을 출력하는 함수
    void draw() const override {
        cout << "크리스마스 트리" << endl;
        star->draw();
        ribbon->draw();
    }

    // 소멸자에서 동적으로 할당된 객체들을 삭제
    ~ChristmasTree() {
        delete star;
        delete ribbon;
    }
};

int main() {
    // 크리스마스 트리 생성
    ChristmasTree christmasTree("빨간 별", "금색 리본");

    // 크리스마스 트리와 포함된 내용을 출력
    christmasTree.draw();

    return 0;
}

