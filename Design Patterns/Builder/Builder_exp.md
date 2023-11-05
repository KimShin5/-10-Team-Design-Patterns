# Builder Pattern
## 개요
빌더 패턴은 디자인패턴중 생성 패턴의 하나로 복잡한 객체를 생성하는 과정을 추상화하여 객체의 구성을 단계적으로 조절하는 방법을 제공한다.
빌더를 통해 객체의 생성 코드를 간결하게 유지 및 가독성 향상에 유연함을 준다. 
자주 사용하지 않는 객체의 특성들을 필요한 객체에만 넣음으로서 보다 쉽고 간편한 객체 생성 및 관리를 가능하게 한다.


빌더 패턴은 복잡한 객체의 생성, 불변 객체를 만들 때 적합하게 사용할 수 있다.

### Director(관리자)
빌더 패턴을 사용할때 ```선택적```으로 사용할 수 있는 기능으로, 객체를 생성하느 과정을 조절하거나 다양한 구성을 하는데 도움을 준다.
디렉터는 클래스로 생성하며, 객체 생성 루틴을 디렉터에게 지시하여 생성된 객체를 받을 수 있다.

### 예시 코드
계획 단계에서 가져온 예시 코드로 해당 코드를 통해 빌더 패턴을 이해 해보자.

```C++
public class Computer {
	//기본이 되는 Computer 클래스

    //객체를 생성할때 필수적으로 정의해야하는 파라미터.
    private String HDD;
    private String RAM;
	
    //그래픽 카드와 블루투스 기능은 옵션.
    private boolean isGraphicsCardEnabled;
    private boolean isBluetoothEnabled;
	
    //아래 코드들은 private 접근 지정자 내부로 타 클래스에서 Computer 클래스로 접근 할 수 있도록 돕는 코드들이다.
    public String getHDD() {
        return HDD;
    }
 
    public String getRAM() {
        return RAM;
    }
 
    public boolean isGraphicsCardEnabled() {
        return isGraphicsCardEnabled;
    }
 
    public boolean isBluetoothEnabled() {
        return isBluetoothEnabled;
    }
	
    //생성자는 private로 선언하여 외부에서 직업 호출할 수 없도록 하였다.
    //Computer의 객체 생성은 무조건 빌더를 통해야만 생성할 수 있음.
    private Computer(ComputerBuilder builder) {
        this.HDD=builder.HDD;
        this.RAM=builder.RAM;
        this.isGraphicsCardEnabled=builder.isGraphicsCardEnabled;
        this.isBluetoothEnabled=builder.isBluetoothEnabled;
    }
	
    //빌더 클래스 생성
    //빌더 클래스를 static으로 만들면 빌더 객체를 생성할 필요가 없음.
    public static class ComputerBuilder{
 
        //필수 파라미터
        private String HDD;
        private String RAM;
 
        //선택 파라미터
        private boolean isGraphicsCardEnabled;
        private boolean isBluetoothEnabled;
		
        //생성자, 파라미터를 입력하게 만들어 놓았다.
        public ComputerBuilder(String hdd, String ram){
            this.HDD=hdd;
            this.RAM=ram;
        }
 
        public ComputerBuilder setGraphicsCardEnabled(boolean isGraphicsCardEnabled) {
            this.isGraphicsCardEnabled = isGraphicsCardEnabled;
            return this;
        }
 
        public ComputerBuilder setBluetoothEnabled(boolean isBluetoothEnabled) {
            this.isBluetoothEnabled = isBluetoothEnabled;
            return this;
        }
		
        //해당 메서드로 인해 Computer의 객체를 생성할 수 있음.
        //객체 생성 후 파라미터를 설정한 뒤 객체를 반환.
        public Computer build(){
            return new Computer(this);
        }
 
    }
 
}
```

객체 생성 예시는 이렇다
```C++
public class TestBuilderPattern {
 
    public static void main(String[] args) {
        //ComputerBuilder 코드를 통한 객체 생성. HDD, RAM의 파라미터는 생성자를 통해 선언한다.
        Computer comp = new Computer.ComputerBuilder("500 GB", "2 GB")
        //아래는 블루투스와 그래픽카드를 true로 설정하므로써 해당 기능을 사용한다고 선언하였음.
                .setBluetoothEnabled(true)
                .setGraphicsCardEnabled(true)
                //.build()를 통한 객체 생성.
                .build();
    }
 
}
```