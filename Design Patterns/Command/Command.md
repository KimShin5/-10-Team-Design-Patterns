# 커맨드
커맨드 분석을 위한 빌더 오픈소스 예제이다.

## Implementation
```c++
#pragma once
#include <stdio.h
>#include <iostream>
using namespace std;

class Light {
    public:  
    void on() {    
       cout << "Power on!" << endl;  
    }    
    void off() {    
        cout << "Power off!" << endl; 
     }
};
class ICommand {
public:  
    virtual void execute() = 0;
   };
class TurnOnCommand : public ICommand {
private:  
    Light* light;
public:  
    TurnOnCommand(Light* light) {
        this->light = light;  
    }  
    void execute() {   
        this->light->on();  
    }
};
class TurnOffCommand : public ICommand {
private:  
    Light* light;
public:  
    TurnOffCommand(Light* light) {    
        this->light = light;  
    }  
    void execute() {       
        this->light->off();  
    }
};
class Switch {
private:  
    ICommand* turnOnCmd;  
    ICommand* turnOffCmd;
public:  
    Switch(ICommand* turnOnCmd, ICommand* turnOffCmd) {     
        this->turnOnCmd = turnOnCmd;    
        this->turnOffCmd = turnOffCmd;  
    } 
    void run() {    
        this->turnOnCmd->execute();      
        cout << "It turns off after 5 minutes." << endl;    
        cout << "It turns off after 4 minutes." << endl;    
        cout << "It turns off after 3 minutes." << endl;    
        cout << "It turns off after 2 minutes." << endl;    
        cout << "It turns off after 1 minutes." << endl;     
        this->turnOffCmd->execute();  
    }
};
int main() {  
    Light light;  
    
    TurnOnCommand turnOnCmd(&light); 
    TurnOffCommand turnOffCmd(&light);  
    Switch button(&turnOnCmd, &turnOffCmd);  

    button.run();   
    
    return 0;
}
```

출처: https://nowonbun.tistory.com/458 [명월 일지:티스토리]