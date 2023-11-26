## 예제
```c++
#include <iostream>
#include <vector>
#include <string>

using namespace std;

// Receiver, 실제 동작을 수행하는 객체
class Drawing {
public:
    void drawCircle() {
        cout << "Drawing a circle" << endl;
    }

    void drawRectangle() {
        cout << "Drawing a rectangle" << endl;
    }

    void erase() {
        cout << "Erasing the last shape" << endl;
    }
};

// Command, 동작을 추상화한 인터페이스
class Command {
public:
    virtual void execute() = 0;
    virtual void undo() = 0;
};

// ConcreteCommand, 실제 동작을 구체화한 클래스
class DrawCircleCommand : public Command {
public:
    DrawCircleCommand(Drawing& drawing) : drawing_(drawing) {}

    void execute() override {
        drawing_.drawCircle();
    }

    void undo() override {
        drawing_.erase();
    }

private:
    Drawing& drawing_;
};

class DrawRectangleCommand : public Command {
public:
    DrawRectangleCommand(Drawing& drawing) : drawing_(drawing) {}

    void execute() override {
        drawing_.drawRectangle();
    }

    void undo() override {
        drawing_.erase();
    }

private:
    Drawing& drawing_;
};

// MacroCommand, 여러 개의 커맨드를 조합하는 클래스
class MacroCommand : public Command {
public:
    void addCommand(Command* command) {
        commands_.push_back(command);
    }

    void execute() override {
        for (Command* command : commands_) {
            command->execute();
        }
    }

    void undo() override {
        for (auto it = commands_.rbegin(); it != commands_.rend(); ++it) {
            (*it)->undo();
        }
    }

private:
    vector<Command*> commands_;
};

// Invoker, 명령을 실행하는 객체
class Artist {
public:
    void performOperation(Command& command) {
        command.execute();
        history_.push_back(&command);
    }

    void performUndo() {
        if (!history_.empty()) {
            Command* lastCommand = history_.back();
            lastCommand->undo();
            history_.pop_back();
        }
    }

private:
    vector<Command*> history_;
};

int main() {
    Drawing canvas;
    Artist artist;

    DrawCircleCommand drawCircleCommand(canvas);
    DrawRectangleCommand drawRectangleCommand(canvas);

    MacroCommand drawShapes;
    drawShapes.addCommand(&drawCircleCommand);
    drawShapes.addCommand(&drawRectangleCommand);

    artist.performOperation(drawShapes); 
    artist.performUndo();                 

    return 0;
}
```

Drawing 클래스가 도형 그리기 및 지우기 동작을 수행한다.
DrawCircleCommand, DrawRectangleCommand 클래스가 동작을 추상화.
MacroCommand 클래스가 여러 개의 동작을 조합할 수 있도록 여러 도형을 그리거나 지울 수 있게 함.
Artist 클래스를 사용하여 명령 실행 및 취소.