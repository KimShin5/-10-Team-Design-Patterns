# 커맨드
커맨드 분석을 위한 빌더 오픈소스 예제이다.

## Implementation
```c++
protocol Command {
    func execute()
}

class SimpleCommand: Command {
    private var payload: String

    init(_ payload: String) {
        self.payload = payload
    }

    func execute() {
        print("SimpleCommand: See, I can do simple things like printing (" + payload + ")")
    }
}

class ComplexCommand: Command {
    private var receiver: Receiver

    private var a: String
    private var b: String

    init(_ receiver: Receiver, _ a: String, _ b: String) {
        self.receiver = receiver
        self.a = a
        self.b = b
    }

    func execute() {
        print("ComplexCommand: Complex stuff should be done by a receiver object.\n")
        receiver.doSomething(a)
        receiver.doSomethingElse(b)
    }
}

class Receiver {
    func doSomething(_ a: String) {
        print("Receiver: Working on (" + a + ")\n")
    }

    func doSomethingElse(_ b: String) {
        print("Receiver: Also working on (" + b + ")\n")
    }
}

class Invoker {
    private var onStart: Command?
    private var onFinish: Command?

    func setOnStart(_ command: Command) {
        onStart = command
    }

    func setOnFinish(_ command: Command) {
        onFinish = command
    }

    func doSomethingImportant() {
        onStart?.execute()
        onFinish?.execute()
    }
}
```

```c++
let invoker = Invoker()
invoker.setOnStart(SimpleCommand("Say Hi!"))

let receiver = Receiver()
invoker.setOnFinish(ComplexCommand(receiver, "Send email", "Save report"))

// 호출자의 실행 함수를 수정을 할 필요없이 ConcreteCommand 내의 receiver 객체를 통하여 다양한 행위를 할 수 있다.
invoker.doSomethingImportant()

// SimpleCommand: See, I can do simple things like printing (Say Hi!)
// ComplexCommand: Complex stuff should be done by a receiver object.
// Receiver: Working on (Send email)
// Receiver: Also working on (Save report)
```

오픈소스 링크
https://joycestudios.tistory.com/39