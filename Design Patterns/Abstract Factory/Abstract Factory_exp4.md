# 추상 팩토리와 Prototype패턴
* 추상 팩토리 패턴은 한번에 여러 제품을 생성할 수 있고 프토로타입은 각 제품을 복제하는데 유용하다. 이로서 다양한 제품을 효과적으로 처리할 수 있다.
* 추상팩토리는 관련된 객체를 생성하는데 사용되며, 프로토타입은 객체를 복제하는데 사용된다. 이를 통해 복잡한 객체 생성을 캡슐화 하며 사용자는 세부 사항을 모르고도 객체 생성 및 복제가 가능하다.

```C++
#include <iostream>
#include <cstring>

// 자료형의 통일을 위한 추상 클래스.
class CodeGenerator {
public:
    // 기계어로 변환해주는 가상 함수.
    virtual CodeGenerator* Clone() const = 0;
};

// 각 운영체제에 맞게 기계어를 변환해주는 클래스.
// CodeGenerator 클래스를 상속 받는다.
class WindowsCodeGenerator : public CodeGenerator {
public:
    CodeGenerator* Clone() const override {
        return new WindowsCodeGenerator(*this);
    }
};

class LinuxCodeGenerator : public CodeGenerator {
public:
    CodeGenerator* Clone() const override {
        return new LinuxCodeGenerator(*this);
    }
};

class MacCodeGenerator : public CodeGenerator {
public:
    CodeGenerator* Clone() const override {
        return new MacCodeGenerator(*this);
    }
};

class Scanner {
public:
    // 입력을 도와주는 함수.
    virtual Scanner* Clone() const = 0;
};

// 각 운영체제에 맞게 입력을 도와주는 클래스.
// Scanner 클래스를 상속 받는다.
class WindowsScanner : public Scanner {
public:
    Scanner* Clone() const override {
        return new WindowsScanner(*this);
    }
};

class LinuxScanner : public Scanner {
public:
    Scanner* Clone() const override {
        return new LinuxScanner(*this);
    }
};

class MacScanner : public Scanner {
public:
    Scanner* Clone() const override {
        return new MacScanner(*this);
    }
};

// 자료형의 통일을 위한 추상 클래스.
class Compiler {
private:
    CodeGenerator* m_pCodeGenerator;
    Scanner* m_pScanner;

public:
    Compiler(CodeGenerator* pCodeGenerator, Scanner* pScanner)
        : m_pCodeGenerator(pCodeGenerator), m_pScanner(pScanner) {}

    // 기계어 생성기 복제
    CodeGenerator* CreateCodeGenerator() const {
        return m_pCodeGenerator->Clone();
    }

    // 입력 도우미 복제
    Scanner* CreateScanner() const {
        return m_pScanner->Clone();
    }
};

// 메인 함수.
int main() {
    char myOS[10] = "Windows";  // 내 컴퓨터의 운영체제.
    Compiler* pCompiler = nullptr;  // 컴파일러 생성.

    // 사용자의 운영체제에 맞게 컴파일러 설정
    if (strcmp(myOS, "Windows") == 0) {
        WindowsCodeGenerator codeGenerator;
        WindowsScanner scanner;
        pCompiler = new Compiler(&codeGenerator, &scanner);
    } else if (strcmp(myOS, "Linux") == 0) {
        LinuxCodeGenerator codeGenerator;
        LinuxScanner scanner;
        pCompiler = new Compiler(&codeGenerator, &scanner);
    } else if (strcmp(myOS, "MAC") == 0) {
        MacCodeGenerator codeGenerator;
        MacScanner scanner;
        pCompiler = new Compiler(&codeGenerator, &scanner);
    } else {
        std::cout << "지원하지 않아요." << std::endl;
    }

    CodeGenerator* pCodeGenerator = pCompiler->CreateCodeGenerator();  // 기계어 코드를 반환해주는 객체 생성.
    Scanner* pScanner = pCompiler->CreateScanner();

    // 메모리 누수 방지를 위해 동적 할당된 객체들을 삭제
    delete pCompiler;
    delete pCodeGenerator;
    delete pScanner;

    return 0;
}
```
* 추상클래스
CodeGenerator, Scanner, Compiler 클래스는 추상 클래스로, 각각 기계어 생성, 입력 도우미, 컴파일러를 나타낸다.
Clone()함수를 순수 가상 함수로 정의하여, 이후에 파생 클래스에서 구현한다.
* 파생클래스
WindowsCodeGenerator, LinuxCodeGenerator, MacCodeGenerator, WindowsScanner, LinuxScanner, MacScanner 클래스들은 추상 클래스를 상속받아 구체적인 운영체제에 맞는 코드 생성 및 입력 도우미를 구현한다.
각 클래스는 Clone() 함수를 구현하여 복사 생성을 지원함.
* 메인 함수
Compiler 클래스는 사용자가 선택한 코드 생성기와 스캐너를 가지고 있다.
CreateCodeGenerator() 및 CreateScanner() 함수를 통해 각각 코드 생성기와 스캐너의 복사본을 만든다.
이런 식으로 Abstract Factory + Prototype 패턴을 결합하여 사용하면 지원하는 운영체제가 늘어나도Concrete Factory클래스가 추가 되지 않는다.  허나 미리 생성할 제품의 종류별로 한개씩 객체를 생성해서 Factory(위 코드상 Compiler) 클래스에 등록해두어야 하는 비용이 발생한다. 특히 생성해야 할 제품이 종류가 많은 경우 Factory(위 코드상 Compiler)클래스의 생성자인터페이스도 복잡해지고 객체 저장에 따른 비용도 커질수있다. 또한 같은 제품군(운영체제)에 속하는 제품(기능)을 생성하려면 처음에 Factory클래스에 등록시키는 객체가 같은 제품군(운영체제)에 속하는 객체여야 한다. 만약 그렇지 않으면 생성되는 객체가 서로 다른 제품군(운영체제)에 속하는 제품(기능)이 될 가능성도 있다.

출처: https://cinrueom.tistory.com/41 