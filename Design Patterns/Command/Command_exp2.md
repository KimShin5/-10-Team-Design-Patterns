## 커맨드 예제 2
```c++
#include <iostream>
#include <string>
#include <vector>

using namespace std;

// Receiver: 실제 동작을 수행하는 객체
class FileSystem {
public:
    //파일 생성 동작 구현
    void createFile(const string& fileName) {
        cout << "Created file: " << fileName << endl;
    }
    //파일 삭제 동작 구현
    void deleteFile(const string& fileName) {
        cout << "Deleted file: " << fileName << endl;
    }
};

// Command: 동작을 추상화한 인터페이스
class Command {
public:
    virtual void execute() = 0;
    virtual void undo() = 0;
};

// ConcreteCommand: 실제 동작을 구체화한 클래스
//파일 생성 커맨드 구체화
class CreateFileCommand : public Command {
public:
    CreateFileCommand(FileSystem& fileSystem, const string& fileName)
        : fileSystem_(fileSystem), fileName_(fileName) {}

    void execute() override {
        fileSystem_.createFile(fileName_);
    }

    void undo() override {
        fileSystem_.deleteFile(fileName_);
    }

private:
    FileSystem& fileSystem_;
    string fileName_;
};

//파일 삭제 커맨드 구체화
class DeleteFileCommand : public Command {
public:
    DeleteFileCommand(FileSystem& fileSystem, const string& fileName)
        : fileSystem_(fileSystem), fileName_(fileName) {}

    void execute() override {
        fileSystem_.deleteFile(fileName_);
    }

    void undo() override {
        fileSystem_.createFile(fileName_);
    }

private:
    FileSystem& fileSystem_;
    string fileName_;
};

// Invoker: 명령을 실행하는 객체
class FileManager {
public:
    //아래 함수를 통해 명령 실행 및 기록.
    void performOperation(Command& command) {
        command.execute();
        history_.push_back(&command);
    }
    //마지막으로 실행된 명령을 취소할 수 있는 기능.
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
    //동작 수행 객체 생성
    FileSystem fileSystem;

    CreateFileCommand createCommand(fileSystem, "example.txt");
    DeleteFileCommand deleteCommand(fileSystem, "example.txt");

    //명령 실행 및 취소
    FileManager fileManager;

    fileManager.performOperation(createCommand);  // Created file: example.txt
    fileManager.performOperation(deleteCommand);  // Deleted file: example.txt
    fileManager.performUndo();                    // Created file: example.txt (Undo the last command)

    return 0;
}
```

코드 출력 결과는 이렇다.
```c++
Created file: example.txt
Deleted file: example.txt
Created file: example.txt
```