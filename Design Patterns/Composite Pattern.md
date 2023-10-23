# 컴포지트 패턴(복합체 패턴)
컴포지트 분석을 위한 빌더 오픈소스 예제이다.

## 1. Base Component
### Shape.java
```c++
public interface Shape {
	
    public void draw(String fillColor);
}
```

## 2. Leaf Objects
### Triangle.java
```c++
public class Triangle implements Shape {
 
    @Override
    public void draw(String fillColor) {
        System.out.println("Drawing Triangle with color "+fillColor);
    }
}
```

### Circle.java
```c++
public class Circle implements Shape {
 
    @Override
    public void draw(String fillColor) {
        System.out.println("Drawing Circle with color "+fillColor);
    }
}
```

## 3. Composite Objects
### Drawing.java
```c++
public class Drawing implements Shape {
 
    //collection of Shapes
    private List<Shape> shapes = new ArrayList<Shape>();
	
    @Override
    public void draw(String fillColor) {
        for(Shape sh : shapes) {
            sh.draw(fillColor);
        }
    }
	
    //adding shape to drawing
    public void add(Shape s) {
        this.shapes.add(s);
    }
	
    //removing shape from drawing
    public void remove(Shape s) {
        shapes.remove(s);
    }
	
    //removing all the shapes
    public void clear() {
        System.out.println("Clearing all the shapes from drawing");
        this.shapes.clear();
    }
}
```

## TestCompositePattern.java
```c++
public class TestCompositePattern {
 
    public static void main(String[] args) {
        Shape tri = new Triangle();
        Shape tri1 = new Triangle();
        Shape cir = new Circle();
		
        Drawing drawing = new Drawing();
        drawing.add(tri1);
        drawing.add(tri1);
        drawing.add(cir);
		
        drawing.draw("Red");
		
        List<Shape> shapes = new ArrayList<>();
        shapes.add(drawing);
        shapes.add(new Triangle());
        shapes.add(new Circle());
        
        for(Shape shape : shapes) {
            shape.draw("Green");
        }
    }
}
```

## 결과
```c++
Drawing Triangle with color Red
Drawing Triangle with color Red
Drawing Circle with color Red
Drawing Triangle with color Green
Drawing Triangle with color Green
Drawing Circle with color Green
Drawing Triangle with color Green
Drawing Circle with color Green
```

오픈소스 링크
https://readystory.tistory.com/131