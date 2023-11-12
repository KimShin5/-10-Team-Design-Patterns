# 커맨드
## 개요
커맨드는 메서드 호출, 연산, 요청 등을 객체로 캡슐화 하여 매개 변수화된 클라이언트 측에서 서로 다른 요청을 매개 변수화된 객체를 사용해 처리하는 기능이다.
실행될 기능을 캡슐화 함으로 주어진 여러 기능을 실행할 수 있는 재사용성이 높은 클래스를 설계.

명령 저장, 실행 등의 역할을 수행하는 ```invoker(발송자)``` 클래스, 작업을 수행하는 ```Receiver(수신자)```클래스(수신자는 거의 모든 객체가 수신자의 역할을 수행.)
등의 주요 구조가 존재한다.
### 예시 코드
```c++
//전처리기 지시문 중 하나로 같은 헤더 파일이 여러 번 포함 되는 것을 방지. 
#pragma once
//Standard Input and Output Library 헤더파일. printf, scanf, fprintf, fscanf 등의 명령어가 있음.
#include <stdio.h>
#include <iostream>
using namespace std;
// 전등 클래스(reciever) 생성.
class Light {
    public:  
    // 전등 켜질 때 함수  
    void on() {    
        // 콘솔 출력    
        cout << "Power on!" << endl;  
    }  
    // 전등 꺼질 때 함수  
    void off() {   
        // 콘솔 출력    
        cout << "Power off!" << endl; 
     }
};
// 커맨드 인터페이스, 커맨드를 실행하기 위한 단일 메서드 선언.
class ICommand {
public:  
    // 함수 추상화, virtual을 이용한 가상함수.  
    virtual void execute() = 0;
   };
// 전등 키는 클래스, 커맨드 인터페이스 상속함.
class TurnOnCommand : public ICommand {
private:  
    // 전등 인스턴스 맴버 변수  
    Light* light;
public:  
    // 생성자  
    TurnOnCommand(Light* light) {
        // 맴버 변수 설정    
        this->light = light;  
    }  
    // 실행 함수 재정의  
    void execute() {   
        // 전등 켜는 함수 실행    
        this->light->on();  
    }
};
// 전등 끄는 클래스, 커맨드 인터페이스 상속
class TurnOffCommand : public ICommand {
private:  
    // 전등 인스턴스 맴버 변수  
    Light* light;
public:  
    // 생성자  
    TurnOffCommand(Light* light) {    
        // 맴버 변수 설정    
        this->light = light;  
    }  
    // 실행 함수 재정의  
    void execute() {    
        // 전등 끄는 함수 실행    
        this->light->off();  
    }
};
// 스위치(invoker)
class Switch {
private:  
    // 커맨드 맴버 변수  
    ICommand* turnOnCmd;  
    ICommand* turnOffCmd;
public:  
    // 생성자  
    Switch(ICommand* turnOnCmd, ICommand* turnOffCmd) {    
        // 맴버 변수 설정    
        this->turnOnCmd = turnOnCmd;    
        this->turnOffCmd = turnOffCmd;  
    }  // 실행 함수  
    void run() {    
        // 스위치 켜는 커맨드 실행    
        this->turnOnCmd->execute();    
        // 콘솔 출력    
        cout << "It turns off after 5 minutes." << endl;    
        cout << "It turns off after 4 minutes." << endl;    
        cout << "It turns off after 3 minutes." << endl;    
        cout << "It turns off after 2 minutes." << endl;    
        cout << "It turns off after 1 minutes." << endl;    
        // 스위치 끄는 커맨드 실행    
        this->turnOffCmd->execute();  
    }
};// 실행 함수
int main() {  
    // 전등 인스턴스 생성  
    Light light;  
    // 커맨드 인스턴스 생성  
    TurnOnCommand turnOnCmd(&light); 
    TurnOffCommand turnOffCmd(&light);  
    Switch button(&turnOnCmd, &turnOffCmd);  
    // 실행  
    button.run();   
    
    return 0;
}
```