# 커맨드 패턴

```C++
#include <iostream>
using namespace std;

// 리시버 생성(리시버 : 동작 수행 객체)
class HandLight {
private:
    int LightPower = 0;
public:
    // 손전등 켜기 함수
    void LightOn() { cout << "Light On" << endl; }
    // 손전등 끄기 함수
    void LightOff() { cout << "Light Off" << endl; }
    // 손전등 빛 세기 증가 함수
    void LightPUp() {
        if (LightPower < 5) {
            LightPower++;
            cout << "Light Power Up" << endl;
        }
        else {
            cout << "더이상 증가시킬 수 없다" << endl;
        }
    }
    // 손전등 빛 세기 감소 함수
    void LightPDown() {
        if (LightPower > 0) {
            LightPower--;
            cout << "Light Power Down" << endl;
        }
        else {
            cout << "더이상 감소시킬 수 없다" << endl;
        }
    }
    // 현재 빛 세기 값 반환
    int GetLightPower() { return LightPower; }
};

// 커맨드 인터페이스(커맨드를 실행하기 위함, 단일 메서드만 선언한다)
class MyCommand {
public:
    // 가상함수 선언
    virtual void execution() = 0;
};

// 손전등 켜기 클래스
class TurnOn : public MyCommand {
private:
    HandLight* light;
public:
    TurnOn(HandLight* light) { this->light = light; }
    void execution() { this->light->LightOn(); }
};

// 손전등 끄기 클래스
class TurnOff : public MyCommand {
private:
    HandLight* light;
public:
    TurnOff(HandLight* light) { this->light = light; }
    void execution() { this->light->LightOff(); }
};

// 손전등 빛 세기 증가 클래스
class TurnUp : public MyCommand {
private:
    HandLight* light;
public:
    TurnUp(HandLight* light) { this->light = light; }
    void execution() { this->light->LightPUp(); }
};

// 손전등 빛 세기 감소 클래스
class TurnDown : public MyCommand {
private:
    HandLight* light;
public:
    TurnDown(HandLight* light) { this->light = light; }
    void execution() { this->light->LightPDown(); }
};

// 손전등 켜기 버튼(involker, 커맨드의 요청을 시작하는 역할)
class Onbutton {
private:
    MyCommand* TurnOnBt;
public:
    Onbutton(MyCommand* TurnOnBt) {
        this->TurnOnBt = TurnOnBt;
    }
    void execute() {
        // 손전등 스위치 켜기
        this->TurnOnBt->execution();
    }
};

// 손전등 끄기 버튼(involker, 커맨드의 요청을 시작하는 역할)
class Offbutton {
private:
    MyCommand* TurnOffBt;
public:
    Offbutton(MyCommand* TurnOffBt) {
        this->TurnOffBt = TurnOffBt;
    }
    void execute() {
        // 손전등 스위치 끄기
        this->TurnOffBt->execution();
    }
};

// 손전등 빛 세기 증가 버튼(involker, 커맨드의 요청을 시작하는 역할)
class Upbutton {
private:
    MyCommand* TurnUpBt;
public:
    Upbutton(MyCommand* TurnUpBt) {
        this->TurnUpBt = TurnUpBt;
    }
    void execute() {
        // 손전등 빛 세기 올리기
        this->TurnUpBt->execution();
    }
};

// 손전등 빛 세기 감소 버튼(involker, 커맨드의 요청을 시작하는 역할)
class Downbutton {
private:
    MyCommand* TurnDownBt;
public:
    Downbutton(MyCommand* TurnDownBt) {
        this->TurnDownBt = TurnDownBt;
    }
    void execute() {
        // 손전등 빛 세기 줄이기
        this->TurnDownBt->execution();
    }
};


int main() {
    HandLight mk1;
    TurnOn TurnOnBt(&mk1);
    TurnOff TurnOffBt(&mk1);
    TurnUp TurnUpBt(&mk1);
    TurnDown TurnDownBt(&mk1);
    
    Onbutton OnPush(&TurnOnBt);
    Upbutton UpPush(&TurnUpBt);
    Downbutton DownPush(&TurnDownBt);
    Offbutton OffPush(&TurnOffBt);

    OnPush.execute();   // 손전등 켜기
    UpPush.execute();   // 손전등 빛 세기 증가
    UpPush.execute();
    UpPush.execute();
    cout << "LightPower: " << mk1.GetLightPower() << endl;
    DownPush.execute(); // 손전등 빛 세기 감소
    cout << "LightPower: " << mk1.GetLightPower() << endl;
    OffPush.execute();


    return 0;
}
```