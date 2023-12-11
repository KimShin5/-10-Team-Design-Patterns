## 빌더 예제 2
```c++
#include <iostream>
#include <string>

using namespace std;

// Product: 생성할 객체의 클래스
// 컴퓨터에 필요한 CPU, RAM, Storage 등이 있지만, 그 종류는 다 다를 것이다.
class Computer {
public:
    void setCPU(const string& cpu) {
        cpu_ = cpu;
    }

    void setRAM(const string& ram) {
        ram_ = ram;
    }

    void setStorage(const string& storage) {
        storage_ = storage;
    }

    void display() const {
        cout << "Computer: CPU=" << cpu_ << ", RAM=" << ram_ << ", Storage=" << storage_ << endl;
    }

private:
    string cpu_;
    string ram_;
    string storage_;
};

// Builder: 객체 생성 단계를 추상화하는 인터페이스
class ComputerBuilder {
public:
    //각 객체 설정 가상함수
    virtual void buildCPU() = 0;
    virtual void buildRAM() = 0;
    virtual void buildStorage() = 0;
    //최종 객체 반환 가상함수
    virtual Computer getResult() const = 0;
};

// ConcreteBuilder: Builder 인터페이스를 구현하여 실제 객체를 생성하는 클래스
class GamingComputerBuilder : public ComputerBuilder {
public:
// 각 객체들에 값을 입력
    void buildCPU() override {
        computer_.setCPU("High-end Gaming CPU");
    }

    void buildRAM() override {
        computer_.setRAM("32GB Gaming RAM");
    }

    void buildStorage() override {
        computer_.setStorage("1TB SSD");
    }

    Computer getResult() const override {
        return computer_;
    }

private:
    Computer computer_;
};

// Director: Builder를 사용하여 객체 생성의 순서 및 방법을 정의하는 클래스
class ComputerDirector {
public:
    //디렉터, 빌더를 통한 객체 생성 방법 정의. getResult 함수를 통해 최종 객체 반환.
    Computer construct(ComputerBuilder& builder) {
        builder.buildCPU();
        builder.buildRAM();
        builder.buildStorage();
        return builder.getResult();
    }
};

int main() {
    // 클라이언트 코드
    ComputerDirector director; //디렉터 호출
    GamingComputerBuilder gamingBuilder; //원하는 빌더를 통한 객체 생성

    Computer gamingComputer = director.construct(gamingBuilder);

    cout << "Built a Gaming Computer:" << endl;
    gamingComputer.display();

    return 0;
}
```

해당 코드의 출력 값은 이렇다.
```c++
Built a Gaming Computer:
Computer: CPU=High-end Gaming CPU, RAM=32GB Gaming RAM, Storage=1TB SSD
```