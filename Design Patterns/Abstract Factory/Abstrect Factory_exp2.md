# 예시코드
```C++
#include <iostream>

using namespace std;

// 추상제품 A
class Button {
public:
    virtual void render() = 0;
    virtual ~Button() = default;
};

//구상제품 A1
class WindowsButton : public Button {
public:
    void render() override {
        cout << "Rendering Windows Button" << endl;
    }
};

// 구상제품 A2
class MacButton : public Button {
public:
    void render() override {
        cout << "Rendering Mac Button" << endl;
    }
};

// 추상제품 B
class TextBox {
public:
    virtual void display() = 0;
    virtual ~TextBox() = default;
};

// 구상제품 B1
class WindowsTextBox : public TextBox {
public:
    void display() override {
        cout << "Displaying Windows TextBox" << endl;
    }
};

// 구상제품 B2
class MacTextBox : public TextBox {
public:
    void display() override {
        cout << "Displaying Mac TextBox" << endl;
    }
};

// 추상팩토리
class WidgetFactory {
public:
    // 제품을 생성하는 가상함수
    virtual Button* createButton() = 0;
    virtual TextBox* createTextBox() = 0;
    virtual ~WidgetFactory() = default;
};

// 구상팩토리 1
class WindowsWidgetFactory : public WidgetFactory {
public:
    Button* createButton() override {
        return new WindowsButton();
    }

    TextBox* createTextBox() override {
        return new WindowsTextBox();
    }
};

// 구상 팩토리 2
class MacWidgetFactory : public WidgetFactory {
public:
    Button* createButton() override {
        return new MacButton();
    }

    TextBox* createTextBox() override {
        return new MacTextBox();
    }
};

int main() {
    // 사용할 팩토리 선택
    WidgetFactory* windowsFactory = new WindowsWidgetFactory();
    WidgetFactory* macFactory = new MacWidgetFactory();

    // 팩토리를 통해 각각의 제품 생성
    Button* windowsButton = windowsFactory->createButton();
    TextBox* windowsTextBox = windowsFactory->createTextBox();

    Button* macButton = macFactory->createButton();
    TextBox* macTextBox = macFactory->createTextBox();

    // 생성된 제품 사용
    windowsButton->render();
    windowsTextBox->display();

    macButton->render();
    macTextBox->display();
    
    return 0;
}
